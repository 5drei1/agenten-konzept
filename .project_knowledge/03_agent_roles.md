# Agent Roles

## Zweck dieser Datei

Beschreibt die verschiedenen Agenten-Rollen im System, ihre Verantwortlichkeiten und wie sie zusammenarbeiten.

## Definierte Rollen

### Koordinations-Agent (Orchestrator)

**Verantwortung**: Übergeordnete Aufgabenplanung und Delegation

**Aufgaben**:
- Empfängt komplexe Aufgaben vom Nutzer
- Zerlegt Aufgaben in Teilaufgaben
- Delegiert an Spezial-Agenten
- Aggregiert Ergebnisse
- Kommuniziert finales Ergebnis an Nutzer

**Eigenschaften**:
- Kennt alle verfügbaren Spezial-Agenten
- Versteht Abhängigkeiten zwischen Teilaufgaben
- Kann parallele Ausführung koordinieren

### Spezial-Agenten

Jeder Spezial-Agent hat einen eng definierten Verantwortungsbereich:

**Research Agent**:
- Web-Recherche und Informationssammlung
- Faktenprüfung
- Zusammenfassung von Quellen

**Code Agent**:
- Code schreiben, reviewen und debuggen
- Technische Dokumentation
- Test-Erstellung

**Writing Agent**:
- Texte verfassen und editieren
- Dokumentation erstellen
- Kommunikation formulieren

## Agenten-Interaktion

Agenten kommunizieren über strukturierte Übergabe-Dokumente. Jede Übergabe enthält:
1. Aufgabenbeschreibung
2. Kontext und Vorarbeit
3. Erwartetes Output-Format
4. Deadline/Priorität
