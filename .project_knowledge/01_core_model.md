# Core Model

## Kurzfassung

Dieses Repository entwickelt ein Konzept für ein Multi-Agenten-System, das auf Claude (Anthropic) basiert. Das System besteht aus spezialisierten Agenten, die über strukturierte Protokolle miteinander kommunizieren.

## Kernkomponenten

### 1. Agenten-Architektur

- **Spezialisierte Agenten**: Jeder Agent hat einen klar definierten Verantwortungsbereich
- **Hierarchische Struktur**: Koordinations-Agent orchestriert Spezial-Agenten
- **Klare Schnittstellen**: Standardisierte Input/Output-Formate zwischen Agenten

### 2. Kommunikationsprotokoll

- **Structured Handoffs**: Explizite Übergabe-Dokumente zwischen Agenten
- **Context Preservation**: Kontext wird über Agenten-Grenzen hinweg erhalten
- **Task Decomposition**: Komplexe Aufgaben werden in Teilaufgaben zerlegt

### 3. Wissensverwaltung

- **Shared Knowledge Base**: Gemeinsame Wissensbasis in `.project_knowledge/`
- **Agent-spezifisches Wissen**: Jeder Agent hat eigene Spezialkenntnisse
- **Dynamische Updates**: Wissen wird während der Ausführung aktualisiert

## Design-Prinzipien

### Prinzip 1: Explizitheit über Implizitheit

Alle Annahmen, Entscheidungen und Übergaben sind explizit dokumentiert. Kein Agent macht implizite Annahmen über den Zustand anderer Agenten.

### Prinzip 2: Modulare Komposierbarkeit

Agenten können unabhängig voneinander entwickelt, getestet und ausgetauscht werden. Das System ist durch Komposition aufgebaut, nicht durch enge Kopplung.

### Prinzip 3: Nachvollziehbarkeit

Jede Entscheidung und jeder Arbeitsschritt ist dokumentiert und nachvollziehbar. Das ermöglicht Debugging, Optimierung und Auditierung.

### Prinzip 4: Graceful Degradation

Das System funktioniert auch wenn einzelne Agenten nicht verfügbar sind oder Fehler auftreten. Fallback-Mechanismen sind Teil des Designs.

## Technische Grundlagen

### Claude als Basis

- Verwendung der Claude API (Anthropic)
- Ausnutzung von langen Kontextfenstern für komplexe Aufgaben
- Tool-Use für externe Interaktionen
- Strukturierte Outputs für zuverlässige Agenten-Kommunikation

### Implementierungssprache

Das Konzept ist sprachunabhängig, aber Referenzimplementierungen werden in Python entwickelt:
- `anthropic` Python SDK
- Async-Patterns für parallele Agenten-Ausführung
- Pydantic für Datenvalidierung

### Persistenz

- Dateibasierte Persistenz für Einfachheit
- JSON/YAML für strukturierte Daten
- Markdown für menschenlesbare Dokumente
