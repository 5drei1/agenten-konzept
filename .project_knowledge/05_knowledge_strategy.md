# Knowledge Strategy

## Zweck dieser Datei

Beschreibt wie Wissen im Projekt organisiert, gepflegt und von Agenten genutzt wird.

## Wissens-Hierarchie

### Ebene 1: Projekt-Wissen (`.project_knowledge/`)

Das strukturierte Kernwissen über das Projekt selbst:
- Architektur-Entscheidungen
- Konventionen und Regeln
- Aktuelle Roadmap
- Offene Fragen

**Charakteristik**: Relativ stabil, manuell gepflegt, von Menschen und Agenten lesbar

### Ebene 2: Agenten-Wissen

Spezifisches Fachwissen einzelner Agenten:
- Tool-spezifisches Wissen
- Domänen-Expertise
- Erfahrungen aus vergangenen Aufgaben

**Charakteristik**: Agent-spezifisch, kann dynamisch aktualisiert werden

### Ebene 3: Task-Wissen

Wissen das während einer Aufgabe entsteht:
- Zwischenergebnisse
- Gefundene Informationen
- Getroffene Entscheidungen

**Charakteristik**: Ephemer, aufgabenspezifisch, wird nicht dauerhaft gespeichert

## Wissens-Aktualisierung

### Wann wird `.project_knowledge/` aktualisiert?

1. Nach wichtigen Architektur-Entscheidungen
2. Wenn neue Patterns identifiziert werden
3. Wenn Konventionen sich ändern
4. Regelmäßige Reviews (z.B. wöchentlich)

### Wer aktualisiert `.project_knowledge/`?

- Primär: Menschliche Entwickler
- Sekundär: Agenten mit expliziter Berechtigung
- Review: Immer durch Menschen vor dem Commit

## Wissens-Zugriff

### Für Agenten

```python
# Empfohlenes Pattern für Agenten
def load_project_knowledge():
    knowledge_dir = Path(".project_knowledge")
    knowledge = {}
    for file in sorted(knowledge_dir.glob("*.md")):
        knowledge[file.stem] = file.read_text()
    return knowledge
```

### Priorität beim Konflikt

Wenn Wissen aus verschiedenen Quellen widersprüchlich ist:
1. Task-spezifische Anweisungen (höchste Priorität)
2. Agenten-spezifisches Wissen
3. `.project_knowledge/` Dateien
4. Allgemeines Modell-Wissen (niedrigste Priorität)

## Wissens-Qualität

### Gutes Wissen ist:
- **Präzise**: Klare, unmissverständliche Aussagen
- **Aktuell**: Regelmäßig überprüft und aktualisiert
- **Relevant**: Nur was tatsächlich benötigt wird
- **Strukturiert**: Logisch organisiert und leicht auffindbar

### Anti-Patterns

- ❌ Veraltetes Wissen behalten ohne Markierung
- ❌ Widersprüchliche Informationen in verschiedenen Dateien
- ❌ Zu viel Detail, das niemand liest
- ❌ Zu wenig Detail, das Agenten raten lässt
