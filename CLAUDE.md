# CLAUDE.md – Arbeitsgrundlage für Claude Code

Dieses Repository ist ein **Konzept- und Strategierepo** für ein LangGraph-basiertes Agentensystem.
Dein Auftrag: Lies dieses Dokument vollständig bevor du mit einer Aufgabe beginnst.

---

## 1. Worum geht es hier?

Dieses Repo beschreibt ein Framework für KI-Agenten mit LangChain und LangGraph.
Das Kernprinzip: **Workflow first** – nicht Agenten first.

Die fünf Leitregeln des Konzepts:
1. Workflow first, Agenten second
2. Spezialisierte Agenten statt Allround-Agenten
3. Gezieltes Kontext-Loading statt Kontext-Dumping
4. Lokal by default, zentral by exception
5. Datenschutz als Architektur, nicht als Feature

Bausteine des Modells:
- **Agent** = Rolle mit klaren Instruktionen, erlaubten Tools, Grenzen
- **Workflow** = `StateGraph` mit Nodes, Edges, kompiliertem Graphen
- **Tool** = atomar, ein LLM-Call entscheidet wann es läuft
- **Subgraph** = mehrere Schritte, eigener State, wiederverwendbar
- **State** = `TypedDict`, trägt Kontext durch den Graphen
- **Projektwissen** = bevorzugt als Knowledge Graph (Graphify/Graphiti), Fallback Markdown

Entscheidungsregel **Tool vs. Subgraph**:
> Tool = atomare Aktion, ein Schritt, LLM entscheidet wann.
> Subgraph = mehrere Schritte, eigener State, eigenes Routing.

---

## 2. Repository-Struktur

```
.project_knowlage/      ← Arbeitsgrundlage für Agenten (hier zuerst lesen)
  00_overview.md        ← Einstieg, Navigation
  01_core_model.md      ← Kernmodell, Bausteine, Architekturregeln
  02_repository_map.md  ← Wo liegt was
  03_agent_roles.md     ← Agentenrollen
  04_workflow_patterns.md
  05_knowledge_strategy.md  ← Graph before flat file
  06_writing_rules.md
  07_open_questions.md
  08_roadmap.md

docs/                   ← Ausführliche Hauptdokumentation
examples/               ← Konzeptbeispiele
templates/              ← Wiederverwendbare Vorlagen
faq/                    ← Häufige Fragen
PITCH.md                ← Executive Summary, Mermaid-Diagramme
README.md               ← Einstieg für menschliche Leser
```

**Schreibregeln:**
- Agenten schreiben NICHT auf Root-Ebene außer `PITCH.md` und `README.md`
- Neue Konzeptinhalte gehören in `docs/`
- Kompakte Steuerungsinformationen in `.project_knowlage/`
- Kein Code in diesem Repo – es ist ein reines Konzept-Repo

---

## 3. LangGraph – Kernprimitiven

### StateGraph Grundaufbau

```python
from typing import TypedDict, Annotated
from langgraph.graph import StateGraph, START, END
from langgraph.graph.message import add_messages

class State(TypedDict):
    messages: Annotated[list, add_messages]
    project_context: str
    task: str
    result: str

def planner_node(state: State) -> State:
    # LLM-Call, gibt State-Update zurück
    return {"result": "..."}

graph = StateGraph(State)
graph.add_node("planner", planner_node)
graph.add_edge(START, "planner")
graph.add_edge("planner", END)
app = graph.compile()
```

### Agent mit Tools (ReAct-Pattern)

```python
from langchain_anthropic import ChatAnthropic
from langchain_core.tools import tool
from langgraph.prebuilt import create_react_agent

@tool
def read_file(path: str) -> str:
    """Liest eine Datei aus dem Projektverzeichnis."""
    with open(path) as f:
        return f.read()

llm = ChatAnthropic(model="claude-3-5-sonnet-20241022")
agent = create_react_agent(llm, tools=[read_file])
```

### ToolNode (automatisches Tool-Ausführen)

```python
from langgraph.prebuilt import ToolNode

tool_node = ToolNode(tools=[read_file, write_file])
graph.add_node("tools", tool_node)

# Routing: wenn LLM tool_calls hat → tools, sonst END
def should_use_tools(state: State):
    last = state["messages"][-1]
    return "tools" if last.tool_calls else END

graph.add_conditional_edges("agent", should_use_tools)
```

### Human-in-the-Loop mit interrupt()

```python
from langgraph.checkpoint.memory import MemorySaver
from langgraph.types import interrupt

def review_node(state: State) -> State:
    # Pausiert Workflow, wartet auf menschliche Freigabe
    human_input = interrupt({
        "question": "Bitte Ergebnis prüfen:",
        "result": state["result"]
    })
    return {"approved": human_input["approved"]}

memory = MemorySaver()
app = graph.compile(checkpointer=memory, interrupt_before=["review_node"])

# Fortsetzen nach Freigabe:
app.invoke(Command(resume={"approved": True}), config=config)
```

### Subgraph einbinden

```python
# Subgraph ist selbst ein kompilierter Graph
subgraph = build_load_context_subgraph().compile()

# Im Hauptgraphen als Node einbinden
main_graph.add_node("load_context", subgraph)
```

### Supervisor-Pattern (Multi-Agent)

```python
from langgraph_supervisor import create_supervisor

supervisor = create_supervisor(
    agents=[planner_agent, coder_agent, reviewer_agent],
    model=llm,
    prompt="Du koordinierst Planner, Coder und Reviewer..."
).compile()
```

---

## 4. Häufige Fehler – NICHT machen

```python
# FALSCH: State direkt mutieren
def bad_node(state: State):
    state["result"] = "x"  # ❌ Mutation
    return state

# RICHTIG: neues Dict zurückgeben
def good_node(state: State):
    return {"result": "x"}  # ✓ nur geänderte Keys

# FALSCH: interrupt() ohne Checkpointer
app = graph.compile()  # ❌ kein Checkpointer
interrupt({"question": "..."})  # crasht

# RICHTIG:
app = graph.compile(checkpointer=MemorySaver())  # ✓

# FALSCH: LLM direkt in add_conditional_edges
graph.add_conditional_edges("agent", llm.invoke)  # ❌

# RICHTIG: Routing-Funktion die State auswertet
def route(state): return "tools" if state["messages"][-1].tool_calls else END
graph.add_conditional_edges("agent", route)  # ✓

# FALSCH: Alle Tools jedem Agenten geben
agent = create_react_agent(llm, tools=ALL_TOOLS)  # ❌

# RICHTIG: Nur relevante Tools pro Agent
coder = create_react_agent(llm, tools=[read_file, write_file, run_tests])  # ✓
```

---

## 5. Projektwissen laden: Graph before Flat File

```python
def load_project_context(state: State) -> State:
    """
    Reihenfolge:
    1. Graphify-Graph vorhanden? → Subgraph-Query
    2. Graphiti vorhanden? → temporale Fakten
    3. Fallback: Markdown aus project-knowledge/
    """
    project_path = state["project_path"]
    graph_path = Path(project_path) / "project-knowledge" / "graph"

    if graph_path.exists():
        # Graphify: statischer Strukturgraph
        context = graphify_query(graph_path, entity=state["task"])
    elif graphiti_available():
        # Graphiti: temporale Fakten
        context = graphiti_search(query=state["task"])
    else:
        # Fallback
        context = load_markdown_context(project_path)

    return {"project_context": context}
```

**Regel:** Nur den relevanten Subgraph laden – niemals ganze Dokumente in den Prompt.

---

## 6. Zieldateistruktur für ein Implementierungs-Projekt

Wenn du ein neues Implementierungs-Projekt vorbereiten sollst, lege diese Struktur an:

```
mein-projekt/
├── CLAUDE.md                     ← diese Datei fürs Projekt anpassen
├── pyproject.toml                ← uv-managed, Dependencies
├── project-knowledge/
│   ├── graph/                    ← Graphify-Output (nach erstem Scan)
│   ├── architecture.md
│   ├── coding_rules.md
│   ├── known_issues.md
│   └── repo_config.yaml
├── agents/
│   ├── __init__.py
│   ├── planner.py                ← Aufgabe analysieren, Teilschritte planen
│   ├── coder.py                  ← Code schreiben, Dateien ändern
│   ├── reviewer.py               ← Qualität prüfen, Tests, Feedback
│   └── tools/
│       ├── __init__.py
│       ├── file_tools.py
│       ├── git_tools.py
│       └── test_tools.py
├── workflows/
│   ├── __init__.py
│   ├── state.py                  ← TypedDict State-Definition
│   ├── bugfix_flow.py            ← Beispielworkflow
│   └── subgraphs/
│       ├── load_context.py       ← Projektwissen laden
│       └── human_review.py       ← Freigabe-Subgraph
├── evals/
│   ├── eval_harness.py
│   └── fixtures/
├── .claude/
│   └── commands/
│       ├── new-workflow.md
│       ├── new-agent.md
│       └── run-evals.md
└── .env.example
```

---

## 7. Arbeitsregeln für Claude Code

### Vor jeder Aufgabe
1. `.project_knowlage/00_overview.md` lesen
2. `.project_knowlage/01_core_model.md` lesen
3. Relevante `docs/`-Datei lesen wenn die Aufgabe einen spezifischen Bereich betrifft
4. Bestehende Dateien suchen bevor neue erstellt werden (`Glob`, `Grep`)

### Beim Schreiben von Code
- **uv** für Package-Management (`uv add`, `uv run`)
- **ruff** für Linting und Formatting (`ruff check`, `ruff format`)
- Type Hints überall (`str`, `dict`, `TypedDict`, `Annotated`)
- Docstrings für alle Tools (LangChain nutzt sie als Tool-Beschreibung)
- Kein God-Agent: ein Agent = eine Rolle
- Kein God-State: nur tatsächlich benötigte Felder im TypedDict

### Beim Schreiben von Dokumentation
- Deutsch als Sprache
- Kompakt und präzise – kein Fülltext
- Mermaid für Diagramme
- Keine doppelten Inhalte zwischen `.project_knowlage/` und `docs/`

### Was NIE gemacht werden darf
- Projektwissen fest in Agenten verdrahten
- Ganzen Datei-Inhalt in den Prompt laden wenn ein Subgraph reicht
- State direkt mutieren
- `interrupt()` ohne Checkpointer verwenden
- Alle Tools allen Agenten geben
- Vendor-Lock-in durch proprietäre Cloud-only Dienste einführen

---

## 8. MCP-Server und Tools

Folgende MCP-Server sind für dieses Projekt relevant:

| MCP-Server | Zweck |
|---|---|
| **mcpdoc** (langchain-ai/mcpdoc) | LangGraph/LangChain Docs live nachladen via llms.txt |
| **context7** | Python-Library-Docs on-demand |
| **filesystem** | Lokaler Dateizugriff mit konfigurierten Verzeichnissen |
| **github** | GitHub-Zugriff für Issues, PRs, Commits |

MCP-Konfiguration liegt in `.claude/mcp.json` (für dieses Repo) bzw. in `~/.claude/mcp.json` (global).

---

## 9. Wichtige Referenzen

- LangGraph Docs: https://langchain-ai.github.io/langgraph/
- LangGraph llms.txt: https://langchain-ai.github.io/langgraph/llms.txt
- LangChain Docs: https://python.langchain.com/
- Graphify: https://github.com/safishamsi/graphify
- Graphiti: https://github.com/getzep/graphiti
- Langfuse: https://langfuse.com/docs
- uv: https://docs.astral.sh/uv/
- ruff: https://docs.astral.sh/ruff/

---

## 10. Konzept-Kurzreferenz

> **Agenten liefern Fähigkeiten.**
> **Workflows steuern Abläufe.**
> **Tools führen Aktionen aus.**
> **Subgraphs kapseln Teilabläufe.**
> **State trägt den Kontext durch den Graphen.**
> **Projektwissen liefert Kontext – bevorzugt als Graph.**
> **Policies begrenzen Macht und Risiko.**
> **Graphify liefert die Struktur. Graphiti das Laufzeitgedächtnis. Markdown ist der Fallback.**
