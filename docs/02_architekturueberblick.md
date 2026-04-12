# Architekturüberblick

## Zielbild
Das Konzept trennt sauber zwischen Bauen, Orchestrieren und Beobachten.

## Technologische Leitplanke
Für dieses Repository ist die technologische Ausrichtung aktuell bewusst auf **LangChain** und **LangGraph** festgelegt.

Das Repository ist damit nicht als neutraler Marktvergleich gedacht, sondern als fokussierte Konzeptbasis für einen Stack, der als modern, anpassungsfähig und für agentische Workflows sehr geeignet betrachtet wird.

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
- projektspezifisches Wissen liegt vorzugsweise direkt im Projekt oder Kundenrepo
- zentrale Repositories enthalten Standards, Templates und Referenzwissen
- Repositories und externe Systeme bleiben eigenständige Ressourcen
- Policies und Monitoring spannen sich über alle Ebenen

## Aktueller Fokus des Repositories
Der Fokus liegt aktuell auf:
- Konzeptdokumentation
- Strukturverständnis
- Rollen- und Workflow-Modellen
- strategischer und fachlicher Klarheit

Nicht im Fokus liegt aktuell:
- Beispielcode
- Referenzimplementierung
- detaillierte produktionsnahe Datenschutzarchitektur

## Grundregel
Agenten liefern Fähigkeiten. Workflows steuern den Ablauf. Projektwissen wird im Workflow geladen und in den State geschrieben.