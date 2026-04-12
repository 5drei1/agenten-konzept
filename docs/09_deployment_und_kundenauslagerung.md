# Deployment und Kundenauslagerung

## Grundidee
Dasselbe Konzept soll intern, hybrid und beim Kunden selbst betrieben werden können.

## Drei Betriebsmodelle

### Zentral betrieben
Schnell für Prototypen und kleinere Kunden.

### Hybrid
Ein Teil der Steuerung bleibt zentral, sensible Schritte oder Datenhaltung laufen beim Kunden.

### Self-hosted beim Kunden
Geeignet für regulierte oder besonders sensible Umgebungen.

## Empfohlenes Muster
Nicht einen riesigen Graphen pro Kunde bauen, sondern:

- wiederverwendbare Kern-Subgraphs
- kundenspezifische Policies und Profile
- kundenspezifische Integrationsadapter
- einen Host-Workflow pro Kunde

## Was typischerweise beim Kunden laufen sollte
- Zugriff auf interne Daten
- PII-Erkennung und Redaction
- Freigabeschritte
- produktive Integrationen

## Was zentral bleiben kann
- generische Analysebausteine
- Referenz-Workflows
- Dokumentation und Templates
- nicht-sensitive Hilfsservices

## Zielbild
Ein gemeinsamer Kern, mehrere projektspezifische oder kundenspezifische Ausprägungen.