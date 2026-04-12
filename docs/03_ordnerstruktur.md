# Empfohlene Ordnerstruktur

## Grundidee
Die Struktur trennt Fähigkeiten, Abläufe und Wissen. Das hält Agenten wiederverwendbar und Workflows nachvollziehbar.

## Zentrales Framework- oder Konzept-Repo

```text
agents/
  planner/
    agent.py
    prompts.py
    tools/
    config.py
  coder/
    agent.py
    prompts.py
    tools/
    config.py
  reviewer/
    agent.py
    prompts.py
    tools/
    config.py

workflows/
  bugfix/
    graph.py
    state.py
    nodes.py
  bootstrap_project/
    graph.py
    state.py
    nodes.py

framework/
  policies/
  templates/
  shared_subgraphs/

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

### agents/
Hier liegen Agentenrollen mit allgemeinen Instruktionen, erlaubten Tools und Konfigurationen.

### workflows/
Hier liegen die eigentlichen LangGraph-Workflows. Sie verbinden Agenten, Tools, Projektwissen und Freigaben.

### framework/
Hier liegen projektübergreifende Policies, Templates und wiederverwendbare Bausteine.

### project-knowledge/
Hier liegt stabiles, projektspezifisches Wissen direkt am Projekt oder Kundenprozess.

## Kerngedanke
Projektspezifisches Wissen sollte möglichst nah am Projekt liegen. Zentrale Repositories sammeln vor allem allgemeine Standards und Referenzstrukturen.