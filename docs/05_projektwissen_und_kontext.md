# Projektwissen und Kontext

## Grundregel
Projektspezifisches Wissen gehört primär in den Workflow und in das Projektprofil, nicht fest in den Agenten.

## Warum?
Wenn Projektwissen in Agenten verdrahtet wird, verlieren Agenten ihre Wiederverwendbarkeit. Wenn es im Workflow geladen wird, bleibt der Agent allgemein und der Ablauf wird projektspezifisch.

## Arten von Wissen

### Stabiles Projektwissen
- Architektur
- Coding-Regeln
- relevante Verzeichnisse
- Test- und Build-Kommandos
- Sicherheits- und Compliance-Regeln

### Extrahiertes Repo-Wissen
- Dateibaum
- Modulstruktur
- relevante Dateien
- Testlandschaft
- wichtige Entry Points

### Laufzeitwissen
- Ticket
- Stacktrace
- Logs
- betroffene Dateien
- aktueller Diff

## project_profiles/
In `project_profiles/` liegt vor allem langlebiges Wissen. Es ist keine Ablage für jeden einzelnen Lauf oder jedes Ticket.

## Repo-Einbindung
Das eigentliche Git-Repository liegt separat, zum Beispiel unter `workspace/repos/`. Das Projektprofil enthält Metadaten darüber, etwa den Pfad, wichtige Verzeichnisse oder Regeln.

## Typischer Ablauf
1. Workflow erhält `project_id`
2. Workflow lädt `project_profile.yaml` und `repo_config.yaml`
3. Workflow ermittelt relevante Dateien und Regeln
4. State wird mit Projektkontext gefüllt
5. Agenten erhalten nur den nötigen Ausschnitt

## Merksatz
Projektwissen wird im Workflow injiziert. Agenten erhalten nur den relevanten Teilkontext.