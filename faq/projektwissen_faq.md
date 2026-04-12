# FAQ: Projektwissen und Kontext

## Gehört Git-Code in den Agentenordner?
Nein. Das Repository bleibt eine eigene Ressource. Das Projektprofil verweist darauf.

## Was gehört in `project_profiles/`?
Vor allem stabiles, projektspezifisches Wissen wie Architektur, Regeln, Repo-Metadaten und Besonderheiten.

## Gehören aktuelle Tickets oder Logs in `project_profiles/`?
In der Regel nein. Das sind meist Laufzeitdaten und gehören in Workflow-Input oder State.

## Wird Projektwissen im Agenten oder im Workflow geladen?
Primär im Workflow. Der Agent bekommt nur den relevanten Ausschnitt.

## Warum ist das wichtig?
Weil Agenten dadurch wiederverwendbar bleiben und nicht pro Projekt hart umgebaut werden müssen.