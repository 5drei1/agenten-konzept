# Best Practices und Architekturentscheidungen

## Workflow first
Prozesse sollten zuerst als Ablauf beschrieben werden. Agenten sind dann ausführende Rollen innerhalb dieses Ablaufs.

## Spezialisierte Agenten statt Universalagent
Kleine, klar definierte Agenten sind leichter zu kontrollieren, zu testen und wiederzuverwenden.

## Projektwissen im Workflow
Projektspezifisches Wissen wird im Workflow geladen und über den State weitergereicht. Es gehört nicht hart in die Agentenlogik.

## Tools streng begrenzen
Jeder Agent bekommt nur die Tools, die er für seine Rolle wirklich braucht.

## Subgraphs für Wiederverwendung
Wiederkehrende Aufgaben wie Kontextladen, Review, Freigaben oder Testvalidierung sollten als wiederverwendbare Teil-Workflows umgesetzt werden.

## Datenschutz als Architekturthema
PII-Schutz ist kein nachträgliches Add-on, sondern Teil des Workflow-Designs.

## Sauber starten
Ein gutes Startset besteht oft aus:

- Planner
- Coder
- Reviewer
- 2 bis 4 Kernworkflows
- Projektprofilen
- einfachen Policies
- Tracing und ersten Evals

## Bewusste Vereinfachung
Nicht jedes Problem braucht Multi-Agent-Systeme. Manche Schritte sind mit Skripten oder einfachen Tools besser gelöst.