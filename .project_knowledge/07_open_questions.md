# Open Questions

## Zweck dieser Datei

Dokumentiert offene Fragen und Diskussionspunkte, die noch nicht final entschieden wurden.

## Format

Jede Frage hat:
- **Status**: `[OFFEN]`, `[IN DISKUSSION]`, `[ENTSCHIEDEN]`
- **Priorität**: `[HOCH]`, `[MITTEL]`, `[NIEDRIG]`
- **Kontext**: Warum stellt sich diese Frage?
- **Optionen**: Mögliche Antworten/Lösungen
- **Entscheidung**: (nur wenn Status = ENTSCHIEDEN)

---

## Architektur

### [OFFEN][HOCH] Wie werden Agenten-Fehler behandelt?

**Kontext**: Was passiert wenn ein Agent einen Fehler wirft oder ein unerwartetes Ergebnis liefert?

**Optionen**:
1. Retry-Mechanismus mit maximalem Retry-Count
2. Fallback auf anderen Agenten
3. Fehler an Orchestrator propagieren
4. Kombination aus 1+3

### [OFFEN][MITTEL] Wie wird Agenten-Zustand persistiert?

**Kontext**: Sollen Agenten zwischen Aufgaben Zustand behalten können?

**Optionen**:
1. Kein persistierter Zustand (stateless)
2. Dateibasierte Persistenz
3. Datenbank
4. Nur während einer Session

### [IN DISKUSSION][HOCH] Synchrone vs. Asynchrone Kommunikation

**Kontext**: Sollen Agenten synchron (warten auf Antwort) oder asynchron (Fire-and-Forget mit Callbacks) kommunizieren?

**Optionen**:
1. Vollständig synchron (einfacher, aber blockiert)
2. Vollständig asynchron (komplexer, aber effizienter)
3. Hybrid: synchron für kurze, asynchron für lange Aufgaben

**Aktuelle Tendenz**: Option 3 (Hybrid)

## Implementation

### [OFFEN][MITTEL] Welches Framework für die Implementierung?

**Kontext**: Soll ein bestehendes Framework (LangChain, AutoGen, etc.) genutzt werden oder eine eigene Lösung?

**Optionen**:
1. LangChain/LangGraph
2. Microsoft AutoGen
3. Eigene Implementierung
4. Anthropic's eigene Tools

### [OFFEN][NIEDRIG] Testing-Strategie für Agenten

**Kontext**: Wie testet man nicht-deterministisches Agenten-Verhalten?

**Optionen**:
1. Snapshot-Tests der Outputs
2. Property-based Testing
3. Mock-LLM für deterministische Tests
4. Evaluierungs-Framework (RAGAS, etc.)
