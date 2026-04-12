# Empfohlene Ordnerstruktur

## Grundidee
Die Struktur trennt Fähigkeiten, Abläufe und Wissen. Das hält Agenten wiederverwendbar und Workflows nachvollziehbar.

## Repo-Organisation (Gitea / Self-hosted Git)

Die Empfehlung ist eine **Organization** mit mehreren Repositories, aufgeteilt nach Verantwortlichkeit – nicht nach technischer Kategorie.

```text
gitea-org/
  agent-framework/      ← geteilte Agenten, Workflows, Tools, Subgraphs
  agenten-konzept/      ← dieses Konzept-Repo
  projekt-kunde-a/      ← Kundenprojekt mit src/ + project-knowledge/
  projekt-kunde-b/      ← weiteres Kundenprojekt
```

### Warum nicht ein Repo pro Kategorie (agents/, workflows/, tools/)?

Ein Workflow importiert gleichzeitig Agenten und Tools. Liegen diese in separaten Repos, entstehen cross-repo-Abhängigkeiten, die Packaging, Submodules oder eine eigene Abhängigkeitsverwaltung erfordern. Dieser Overhead lohnt sich erst bei sehr großer Skalierung.

### Warum nicht ein Repo pro Agent oder Workflow?

Zu granular. Änderungen, die Agent und Workflow gleichzeitig betreffen, erfordern koordinierte Commits über viele Repos hinweg. Das erschwert Entwicklung und Nachvollziehbarkeit.

### Wann macht ein eigenes Tools-Repo Sinn?

Wenn Tools wirklich projektübergreifend geteilt werden, semantisch versioniert werden sollen und von mehreren unabhängigen Teams genutzt werden. Bis dahin gehören sie in `agent-framework/tools/`.

### Prompts

Prompts gehören **nicht** in ein eigenes Repo. Zwei sinnvolle Optionen:
- Im Code neben dem Agenten (`agents/planner/prompts.py`) – versioniert mit dem Code, einfach zu ändern
- In Langfuse verwaltet – dann sind sie gar nicht im Git, sondern über die Langfuse-API abrufbar

### Beobachtung (Langfuse)

Langfuse läuft als eigenständiger Dienst (Docker Compose, self-hosted). Es hat keinen Platz im Code-Ordner. Die Integration erfolgt über einen Callback-Handler im Workflow. Konfiguration (Endpoint, API-Key) kommt aus Umgebungsvariablen.

---

## Zentrales Framework-Repo (agent-framework/)

```text
tools/
  file_ops.py
  git_tools.py
  test_runner.py
  search.py

agents/
  planner/
    agent.py        ← Node-Funktion + llm.bind_tools()
    prompts.py
    tools.py        ← agenten-spezifische Tools (falls nicht geteilt)
    config.py
  coder/
    agent.py
    prompts.py
    tools.py
    config.py
  reviewer/
    agent.py
    prompts.py
    tools.py
    config.py

workflows/
  bugfix/
    graph.py        ← StateGraph-Definition, add_node(), add_edge(), compile()
    state.py        ← TypedDict für diesen Workflow
    nodes.py        ← Node-Funktionen inkl. ToolNode
    subgraphs/      ← Subgraphs, die nur dieser Workflow nutzt
      code_review/
        graph.py
        state.py
        nodes.py
  feature/
    graph.py
    state.py
    nodes.py

framework/
  policies/
  templates/
  shared_subgraphs/         ← Subgraphs, die mehrere Workflows teilen
    load_project_context/
      graph.py              ← kompilierter StateGraph
      state.py
      nodes.py
    human_approval/
      graph.py
      state.py
      nodes.py
    run_tests/
      graph.py
      state.py
      nodes.py

run_workflow.py
```

## Projekt- oder Kundenrepo

```text
project-knowledge/
  architecture.md
  coding_rules.md
  repo_config.yaml
  known_issues.md

src/
...
```

## Bedeutung der Bereiche

### tools/
Geteilte Tools, die von mehreren Agenten verwendet werden. Ein Tool ist eine technische Fähigkeit – Dateizugriff, Git-Zugriff, Testausführung, API-Aufruf. Tools werden in LangChain als Funktionen mit `@tool`-Decorator oder als `StructuredTool` definiert und dann per `llm.bind_tools()` an einen Agenten gebunden.

Agenten-spezifische Tools, die kein anderer Agent braucht, können alternativ im jeweiligen Agenten-Unterordner liegen (`agents/planner/tools.py`).

### agents/
Agentenrollen mit Instruktionen, gebundenen Tools und Prompts. In LangGraph ist ein Agent typischerweise eine Node-Funktion, die ein LLM mit gebundenen Tools aufruft. `agent.py` enthält diese Funktion und die Tool-Bindung. `prompts.py` enthält System-Prompts und Prompt-Templates.

### workflows/
LangGraph-Workflows. Jeder Workflow hat:
- `graph.py` – definiert den `StateGraph`, registriert Nodes und Edges, kompiliert den Graphen
- `state.py` – definiert den `TypedDict` für den Workflow-State
- `nodes.py` – enthält alle Node-Funktionen des Workflows, inklusive `ToolNode` für automatische Tool-Ausführung

Subgraphs, die nur für diesen einen Workflow relevant sind, liegen in `workflows/[name]/subgraphs/`. Sie haben dieselbe interne Struktur (`graph.py`, `state.py`, `nodes.py`) wie ein vollständiger Workflow.

### framework/shared_subgraphs/
Wiederverwendbare Subgraphs, die von mehreren Workflows genutzt werden. Ein Subgraph in LangGraph ist ein vollständig kompilierter `StateGraph`, der in einem übergeordneten Graphen als Node eingebunden wird. Typische Kandidaten für gemeinsame Subgraphs sind: Projektwissen laden, menschliche Freigabe einholen, Tests ausführen und auswerten.

### framework/policies/
Projektübergreifende Regeln für Tool-Rechte, Freigaben und Datenschutz.

### project-knowledge/
Stabiles, projektspezifisches Wissen direkt am Projekt oder Kundenprozess.

## Entscheidungsregel: Tool oder Subgraph?

Ein **Tool** ist die richtige Wahl, wenn:
- die Aufgabe atomar ist (eine Aktion, ein Ergebnis)
- das LLM selbst entscheidet, ob und wann es das Tool aufruft
- kein eigener State oder Routing nötig ist

Ein **Subgraph** ist die richtige Wahl, wenn:
- mehrere Schritte in einer festen oder bedingten Reihenfolge ablaufen
- ein eigener State geführt werden muss
- die Teilaufgabe in mehreren Workflows wiederverwendet wird
- ein Mensch oder eine Regel an einem Punkt freigeben muss

## Kerngedanke
Projektspezifisches Wissen liegt nah am Projekt. Geteilte Tools und Subgraphs liegen im zentralen Framework-Repo. Agenten bleiben schlank. Die Beobachtungsschicht (Langfuse) läuft als externer Dienst und hat keinen Platz im Code-Ordner.
