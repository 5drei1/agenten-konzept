# Empfohlene Ordnerstruktur

## Grundidee
Die Struktur trennt Fähigkeiten, Abläufe und Wissen. Das hält Agenten wiederverwendbar und Workflows nachvollziehbar.

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

project_profiles/
  payment_service/
    project_profile.yaml
    repo_config.yaml
    knowledge/
      architecture.md
      coding_rules.md
      known_issues.md

workspace/
  repos/
    payment_service/

run_workflow.py
```

## Bedeutung der Bereiche

### agents/
Hier liegen Agentenrollen mit allgemeinen Instruktionen, erlaubten Tools und Konfigurationen.

### workflows/
Hier liegen die eigentlichen LangGraph-Workflows. Sie verbinden Agenten, Tools, Projektwissen und Freigaben.

### project_profiles/
Hier liegt stabiles, projektspezifisches Wissen. Dazu gehören Regeln, Architekturhinweise, Repo-Metadaten und Besonderheiten.

### workspace/repos/
Hier liegen echte Git-Repositories oder gemountete Arbeitskopien. Das Projektwissen zeigt auf diese Repositories, es ersetzt sie nicht.

### run_workflow.py
Einfacher Einstiegspunkt zum Starten eines Workflows mit einem konkreten Input.