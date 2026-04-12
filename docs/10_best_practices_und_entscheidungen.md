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
PII-Schutz ist kein nachträgliches Add-on, sondern Teil des Workflow-Designs. Im aktuellen Repository ist dieser Bereich aber nicht Hauptfokus.

## Sauber starten
Ein gutes Startset besteht oft aus:

- Planner
- Coder
- Reviewer
- 2 bis 4 Kernworkflows
- projektnahem Wissen
- einfachen Policies
- Tracing und ersten Evals

Was genau in einem minimalen System enthalten ist, welche Teile bewusst weggelassen werden und wann ein Upgrade auf eine komplexere Stufe sinnvoll ist, beschreibt `docs/12_minimales_system.md`.

## Bewusste Vereinfachung
Nicht jedes Problem braucht Multi-Agent-Systeme. Manche Schritte sind mit Skripten oder einfachen Tools besser gelöst.