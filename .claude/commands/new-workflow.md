# /new-workflow

Atomarer Baustein: Legt eine Workflow-Datei und den zugehörigen State an.

> Dieser Befehl ist ein **Ausführungsprimitiv**.
> Normalerweise wird er von `/build-agents` intern aufgerufen.
> Du kannst ihn auch direkt nutzen, wenn du gezielt einen Workflow ergänzen willst.

## Verwendung

```bash
/new-workflow [name] [beschreibung]
```

Beispiel:
```bash
/new-workflow bugfix "Analysiert Bug, schreibt Fix, reviewed Ergebnis"
/new-workflow angebot "Erstellt Angebot aus Kundenanfrage, prüft intern, sendet ab"
```

---

## Dialogregel

> Claude stellt so viele Rückfragen wie nötig – aber jede Frage muss eine Entscheidung im Design verändern.
> Fragen die keinen Einfluss auf Pattern, State-Felder, Routing oder HITL-Punkte hätten, werden weggelassen.
> Fragen die Claude selbst aus dem Kontext ableiten kann, werden nicht gestellt.
> Ziel ist ein sauber definierter Workflow – nicht ein kurzer Dialog.

Nie mehrere Fragen auf einmal stellen. Eine Frage, Antwort abwarten, nächste Frage.

---

## Wissensquelle

**Wenn aus `/build-agents` aufgerufen:**
Pattern, State-Felder, Rollen und HITL-Punkte kommen aus der ADR-Datei
(`adr/[name]_[datum].md`, Abschnitte "Architektur" und "Datenfluss").
Claude liest diese zuerst.

**Wenn direkt aufgerufen:**
Claude klärt so lange bis klar ist:
- Welches Pattern passt? (Sequential / Fan-Out / Review Loop / Conditional Routing)
- Welche Agenten-Rollen sind bereits vorhanden?
- Welche State-Felder werden wirklich gebraucht?
- Gibt es Human-in-the-Loop Punkte, wenn ja wo?

**Bei Pattern-Unsicherheit:** `mcpdoc` für aktuelle LangGraph Docs aufrufen.

---

## Was dieser Befehl anlegt

### `workflows/state.py` (falls noch nicht vorhanden)

```python
from typing import TypedDict, Annotated
from langgraph.graph.message import add_messages

class State(TypedDict):
    messages: Annotated[list, add_messages]
    project_context: str   # Projektwissen, geladen durch load_context Subgraph
    task: str              # Aktuelle Aufgabe
    result: str            # Finales Ergebnis
    # Weitere Felder aus ADR – nur was wirklich gebraucht wird
```

### `workflows/[name]_flow.py`

Struktur nach erkanntem Pattern:

**Sequential Chain:**
```python
graph.add_node("load_context", load_context_subgraph)
graph.add_node("[rolle_1]", rolle_1_node)
graph.add_node("[rolle_2]", rolle_2_node)
graph.add_edge(START, "load_context")
graph.add_edge("load_context", "[rolle_1]")
graph.add_edge("[rolle_1]", "[rolle_2]")
graph.add_edge("[rolle_2]", END)
```

**Review Loop (mit HITL):**
```python
from langgraph.checkpoint.memory import MemorySaver
from langgraph.types import interrupt

def review_node(state: State) -> State:
    human_input = interrupt({
        "question": "Bitte Ergebnis prüfen:",
        "result": state["result"]
    })
    return {"approved": human_input["approved"]}

memory = MemorySaver()
app = graph.compile(checkpointer=memory, interrupt_before=["review_node"])
```

**Conditional Routing:**
```python
def route(state: State) -> str:
    # Routing-Logik auf State-Basis, kein LLM-Call hier
    if state["needs_review"]:
        return "reviewer"
    return END

graph.add_conditional_edges("coder", route)
```

### `workflows/__init__.py`

Der neue Workflow wird eingetragen.

### `workflows/subgraphs/` (falls ADR Subgraphs vorsieht)

Wiederkehrende Teilabläufe als eigene kompilierte Subgraphen.

---

## Regeln

- **State ist immer `TypedDict`** – nie plain `dict`
- **Nur benötigte State-Felder** – kein God-State
- **`load_project_context` ist immer der erste Schritt** – Projektwissen vor allen Agenten laden
- **Checkpointer nur bei HITL** – `MemorySaver` lokal, `PostgresSaver` Produktion
- **Routing-Funktionen werten State aus** – kein LLM-Call in `add_conditional_edges`
- **Kommentare auf Deutsch**
- **Bei Pattern-Unsicherheit:** `mcpdoc` aufrufen

---

## Nicht machen

```python
# FALSCH: State direkt mutieren
def bad_node(state: State):
    state["result"] = "x"  # ❌
    return state

# RICHTIG:
def good_node(state: State):
    return {"result": "x"}  # ✓

# FALSCH: interrupt() ohne Checkpointer
app = graph.compile()  # ❌
interrupt({"question": "..."})  # crasht

# RICHTIG:
app = graph.compile(checkpointer=MemorySaver())  # ✓

# FALSCH: LLM direkt in Routing
graph.add_conditional_edges("agent", llm.invoke)  # ❌

# RICHTIG:
def route(state): return "tools" if state["messages"][-1].tool_calls else END
graph.add_conditional_edges("agent", route)  # ✓
```
