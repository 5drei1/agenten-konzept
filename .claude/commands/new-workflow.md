# /new-workflow

Erstellt einen neuen LangGraph-Workflow nach dem Konzeptmuster dieses Repositories.

## Verwendung

```
/new-workflow [name] [beschreibung]
```

Beispiel: `/new-workflow bugfix "Analysiert einen Bug, schreibt Fix, läuft Tests"`

## Was dieser Befehl tut

1. Liest `.project_knowlage/01_core_model.md` und `04_workflow_patterns.md`
2. Erstellt `workflows/[name]_flow.py` mit:
   - `TypedDict` State-Definition
   - `StateGraph` mit Planner → Coder → Reviewer Grundmuster
   - `load_project_context` Subgraph als ersten Schritt
   - Human-in-the-Loop Freigabe vor kritischen Aktionen
   - Conditional Edges für Tool-Calls
3. Erstellt `workflows/subgraphs/[name]_subgraph.py` wenn Teilabläufe wiederkehrend sind
4. Ergänzt `workflows/__init__.py`

## Regeln

- State ist `TypedDict`, nie plain `dict`
- Checkpointer (`MemorySaver` lokal, `PostgresSaver` Produktion) immer bei HITL
- Jeder Agent bekommt nur die Tools die er braucht
- `load_project_context` ist immer der erste Schritt
- Kommentare auf Deutsch
