# Architekturüberblick

## Zielbild
Das Konzept trennt sauber zwischen Bauen, Orchestrieren und Beobachten.

## Empfohlene Rollen der Bausteine

### LangChain
LangChain ist die Bibliothek für Modelle, Tools, einfache Agenten und allgemeine Anwendungsbausteine.

### LangGraph
LangGraph ist die Orchestrierungs- und Workflow-Ebene. Hier werden State, Nodes, Edges, Subgraphs, Interrupts und Routing modelliert.

### LangSmith
LangSmith dient zur Beobachtung, Auswertung, Fehlersuche und Verbesserung von Agenten und Workflows.

### Eigene Management-Schicht
Nicht alles kommt fertig aus dem Ökosystem. Projekte, Agentenregister, Policies, Projektprofile und Kundenregeln sollten bewusst als eigene Fachschicht modelliert werden.

## Kernmodell

- `agents/` enthält Rollen und ihre Tools
- `workflows/` enthält die Abläufe
- `project_profiles/` enthält projektspezifisches Wissen
- Repositories und externe Systeme bleiben eigenständige Ressourcen
- Policies und Monitoring spannen sich über alle Ebenen

## Grundregel
Agenten liefern Fähigkeiten. Workflows steuern den Ablauf. Projektwissen wird im Workflow geladen und in den State geschrieben.