# Projektwissen und Kontext

## Grundregel
Projektspezifisches Wissen gehört primär in den Workflow und möglichst nah an das jeweilige Projekt. Es gehört nicht fest in die Agenten.

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

## Wo dieses Wissen liegen sollte

### Bevorzugt projektlokal
Direkt im Projekt oder Kundenrepo, zum Beispiel unter `project-knowledge/`.

### Zentral nur ergänzend
Für gemeinsame Sichtweisen, Templates oder Referenzstrukturen kann eine zentrale Ablage zusätzlich sinnvoll sein.

## Repo-Einbindung
Das eigentliche Git-Repository bleibt die operative Quelle. Das Projektwissen liegt entweder im Projekt selbst oder in einer begleitenden projektnahen Struktur.

## Typischer Ablauf
1. Workflow erhält `project_id` oder `repo_path`
2. Workflow lädt Wissen aus `project-knowledge/` oder einem projektnahen Profil
3. Workflow ermittelt relevante Dateien und Regeln
4. State wird mit Projektkontext gefüllt
5. Agenten erhalten nur den nötigen Ausschnitt

## Merksatz
Projektwissen wird im Workflow injiziert. Agenten erhalten nur den relevanten Teilkontext.