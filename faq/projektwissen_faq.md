# FAQ: Projektwissen und Kontext

## Gehört Git-Code in den Agentenordner?
Nein. Das Repository bleibt eine eigene Ressource. Der Workflow greift darauf über Kontext, Pfade und Tools zu.

## Was gehört in projektnahes Wissen?
Vor allem stabiles, projektspezifisches Wissen wie Architektur, Regeln, Repo-Metadaten und Besonderheiten.

## Wo sollte dieses Wissen liegen?
Bevorzugt direkt im Projekt oder Kundenrepo, zum Beispiel unter `project-knowledge/`.

## Gehören aktuelle Tickets oder Logs in projektnahes Wissen?
In der Regel nein. Das sind meist Laufzeitdaten und gehören in Workflow-Input oder State.

## Wird Projektwissen im Agenten oder im Workflow geladen?
Primär im Workflow. Der Agent bekommt nur den relevanten Ausschnitt.

## Gibt es trotzdem zentrale Ablagen?
Ja. Für gemeinsame Standards, Templates, Policies und Best Practices sind zentrale Repositories sehr sinnvoll.