# Beispiel: Git-Repo als Projektwissen einbinden

## Grundregel
Das Git-Repository ist nicht Teil des Agentenordners. Es bleibt die operative Ressource.

## Empfohlenes Muster

- Agentenlogik unter `agents/`
- Workflow-Logik unter `workflows/`
- projektspezifisches Wissen direkt im Projekt unter `project-knowledge/`
- zentrale Standards in einem separaten Framework- oder Konzept-Repo

## Beispiel im Projekt-Repo

```text
project-knowledge/
  architecture.md
  coding_rules.md
  repo_config.yaml
  known_issues.md
```

`repo_config.yaml` kann enthalten:

```yaml
project_id: payment_service
repo_path: /workspace/repos/payment_service
important_paths:
  - app/
  - tests/
```

## Ablauf im Workflow
1. Workflow lädt Wissen aus `project-knowledge/`
2. Workflow liest relevante Dateien oder lässt sie von Tools suchen
3. Nur der passende Ausschnitt wird an Agenten übergeben

## Vorteil
Der Agent bleibt allgemein. Das Projekt wird über Kontext und State spezifisch gemacht.