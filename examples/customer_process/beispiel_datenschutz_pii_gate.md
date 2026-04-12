# Beispiel: Datenschutz- und PII-Gate

## Ziel
Sensible Daten sollen nicht ungeprüft an externe Modelle gehen.

## Empfohlenes Muster
1. Intake lokal oder in Kundeninfrastruktur
2. Klassifikation der Daten
3. PII-Erkennung
4. Redaction oder Tokenisierung
5. Entscheidung über Modellrouting
6. externe Modelle nur mit bereinigten Daten

## Typische Rollen
- lokaler Klassifikationsschritt
- Privacy- oder Policy-Node
- optional Human Approval
- externer LLM-Schritt nur für freigegebene Inhalte

## Ergebnis
Das Workflow-Design steuert den Datenschutz. LangGraph orchestriert die Schritte, ersetzt aber nicht die notwendige Datenschutzarchitektur.