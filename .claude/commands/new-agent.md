# /new-agent

Erstellt einen neuen spezialisierten Agenten nach dem Konzeptmuster dieses Repositories.

## Verwendung

```
/new-agent [rolle] [tools]
```

Beispiel: `/new-agent reviewer "read_file,run_tests,post_review"`

## Was dieser Befehl tut

1. Liest `.project_knowledge/03_agent_roles.md`
2. Erstellt `agents/[rolle].py` mit:
   - System-Prompt der Rolle klar definiert
   - Nur die angegebenen Tools gebunden (`llm.bind_tools(tools)`)
   - Klares Ausgabeformat dokumentiert
   - Node-Funktion für LangGraph (`def [rolle]_node(state: State) -> State`)
3. Ergänzt `agents/__init__.py`

## Regeln

- Ein Agent = eine Rolle, keine Allrounder
- System-Prompt explizit: was der Agent tun soll, was nicht, welches Format erwartet wird
- Keine Projektwissen-Logik im Agenten – das kommt aus dem Workflow-State
- Docstrings für alle Tools (LangChain nutzt sie als Beschreibung für das LLM)
