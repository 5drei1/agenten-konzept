# /new-agent

Atomarer Baustein: Legt eine einzelne Agenten-Datei an.

> Dieser Befehl ist ein **Ausführungsprimitiv**.
> Normalerweise wird er von `/build-agents` intern aufgerufen.
> Du kannst ihn auch direkt nutzen, wenn du gezielt eine Rolle ergänzen willst.

## Verwendung

```bash
/new-agent [rolle] [tools]
```

Beispiel:
```bash
/new-agent reviewer "read_file,run_tests,post_review"
/new-agent coder "read_file,write_file,run_tests"
```

---

## Dialogregel

> Claude stellt so viele Rückfragen wie nötig – aber jede Frage muss eine Entscheidung im Design verändern.
> Fragen die keinen Einfluss auf Rolle, Tools oder System-Prompt hätten, werden weggelassen.
> Fragen die Claude selbst aus dem Kontext ableiten kann, werden nicht gestellt.
> Ziel ist ein sauber definierter Agent – nicht ein kurzer Dialog.

Nie mehrere Fragen auf einmal stellen. Eine Frage, Antwort abwarten, nächste Frage.

---

## Wissensquelle

**Wenn aus `/build-agents` aufgerufen:**
Rollen-Definition, Verantwortlichkeit und Tool-Liste kommen aus der ADR-Datei
(`adr/[name]_[datum].md`, Abschnitt "Architektur").
Claude liest diese zuerst, bevor Code generiert wird.

**Wenn direkt aufgerufen:**
Claude klärt so lange bis klar ist:
- Was ist die genaue Verantwortlichkeit dieser Rolle?
- Was ist ausdrücklich nicht ihre Aufgabe?
- In welchen State wird der Agent eingebunden?
- Welche Tools braucht diese Rolle wirklich?

---

## Was dieser Befehl anlegt

### `agents/[rolle].py`

```python
from langchain_anthropic import ChatAnthropic
from langchain_core.tools import tool
from langgraph.prebuilt import create_react_agent
from workflows.state import State

# Tools dieser Rolle
@tool
def beispiel_tool(input: str) -> str:
    """Kurze Beschreibung – LangChain nutzt sie als Tool-Beschreibung fürs LLM."""
    ...

# Agent
llm = ChatAnthropic(model="claude-3-5-sonnet-20241022")

SYSTEM_PROMPT = """
Du bist [Rolle].
Deine Aufgabe: ...
Deine Grenzen: ...
Erwartetes Ausgabeformat: ...
"""

agent = create_react_agent(
    llm,
    tools=[beispiel_tool],
    prompt=SYSTEM_PROMPT
)

# Node-Funktion für LangGraph
def [rolle]_node(state: State) -> State:
    result = agent.invoke({"messages": state["messages"]})
    return {"messages": result["messages"]}
```

### `agents/__init__.py`

Die neue Rolle wird eingetragen.

---

## Regeln

- **Ein Agent = eine Rolle** – keine Allrounder
- **System-Prompt explizit:** was der Agent tun soll, was nicht, welches Format erwartet wird
- **Keine Projektwissen-Logik im Agenten** – Kontext kommt aus dem Workflow-State
- **Docstrings für alle Tools** – LangChain nutzt sie als Beschreibung fürs LLM
- **Nur die Tools binden die diese Rolle wirklich braucht**
- **Bei Pattern-Unsicherheit:** `mcpdoc` für aktuelle LangGraph/LangChain Docs nutzen
- **`context7` nutzen** wenn Tool-Libraries konkret implementiert werden

---

## Nicht machen

```python
# FALSCH: Alle Tools dem Agenten geben
agent = create_react_agent(llm, tools=ALL_TOOLS)  # ❌

# RICHTIG: Nur rollenbezogene Tools
agent = create_react_agent(llm, tools=[read_file, run_tests])  # ✓

# FALSCH: Projektwissen im System-Prompt hard-coden
SYSTEM_PROMPT = "Das Projekt verwendet FastAPI und..."  # ❌

# RICHTIG: Kontext kommt zur Laufzeit aus dem State
def node(state: State): ...  # state["project_context"] enthält das Wissen  # ✓
```
