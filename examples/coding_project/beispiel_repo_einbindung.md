# Beispiel: Git-Repo als Projektwissen einbinden

## Grundregel
Das Git-Repository ist nicht Teil des Agentenordners. Es bleibt eine externe Ressource.

## Empfohlenes Muster

- Agentenlogik unter `agents/`
- Workflow-Logik unter `workflows/`
- Projektwissen unter `project_profiles/`
- echte Repos unter `workspace/repos/`

## Beispiel
`project_profiles/payment_service/repo_config.yaml` kann enthalten:

```yaml
project_id: payment_service
repo_path: /workspace/repos/payment_service
important_paths:
  - app/
  - tests/
```

## Ablauf im Workflow
1. Workflow lädt `repo_config.yaml`
2. Workflow liest relevante Dateien oder lässt sie von Tools suchen
3. Nur der passende Ausschnitt wird an Agenten übergeben

## Vorteil
Der Agent bleibt allgemein. Das Projekt wird über Kontext und State spezifisch gemacht.