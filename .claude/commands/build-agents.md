# /build-agents

Setzt einen zuvor entworfenen Plan aus einer ADR-Datei in konkrete Agenten- und Workflow-Dateien um.

## Zweck

Dieser Befehl ist **Phase 2: Build**.
Er baut nicht aus einer vagen Idee, sondern auf Basis einer bereits erstellten ADR-Datei aus `/design-agents [name]`.

Claude soll die ADR lesen, den geplanten Scope prüfen und dann gezielt die notwendigen Dateien erzeugen oder vorbereiten.

## Verwendung

```bash
/build-agents [name]
```

Beispiel:
```bash
/build-agents bugfix-review-flow
/build-agents angebot-assistent
/build-agents code-review-agent
```

## Erwartete Grundlage

Dieser Befehl erwartet eine bestehende ADR-Datei im Ordner `adr/`, benannt nach dem Schema:

```bash
adr/[name]_[YYYY-MM-DD].md
```

Wenn mehrere passende ADR-Dateien existieren, soll Claude:
- die möglichen Treffer auflisten,
- den Nutzer die richtige Datei auswählen lassen,
- nicht raten.

Wenn keine passende ADR existiert, soll Claude abbrechen und auf `/design-agents [name]` verweisen.

---

## Arbeitsweise

### Phase 2A – ADR lesen und Build-Scope prüfen

Claude liest die ADR und extrahiert daraus:
- Typ: neuer Agent oder kompletter Workflow
- empfohlene Rollen
- empfohlene Workflow-Patterns
- Build-Reihenfolge
- offene Fragen oder Risiken

Wenn die ADR noch Widersprüche oder offene Kernfragen enthält, soll Claude **nicht blind bauen**, sondern zuerst rückfragen.

### Phase 2B – Build-Strategie ableiten

Claude unterscheidet wieder zwischen zwei Fällen:

#### Fall A – Neuer Agent in bestehendem Projekt

Dann soll Claude typischerweise:
- einen neuen Agenten anlegen,
- den Agenten sauber an bestehende Struktur anbinden,
- keine unnötigen neuen Workflows erzeugen,
- nur die minimal nötigen Anpassungen empfehlen oder durchführen.

Typische Artefakte:
- `agents/[rolle].py`
- `agents/__init__.py`
- ggf. Anpassung an bestehendem Workflow oder Routing

#### Fall B – Kompletter Workflow

Dann soll Claude typischerweise:
- benötigte Rollen nacheinander erzeugen,
- State-Struktur anlegen,
- Workflow-Datei anlegen,
- Subgraphs oder Review-Schritte anlegen,
- notwendige Tool-Bindings vorbereiten.

Typische Artefakte:
- `agents/*.py`
- `workflows/state.py`
- `workflows/[name]_flow.py`
- `workflows/subgraphs/*.py`
- ergänzende Dateien wie `agents/__init__.py`

---

## Build-Regeln

- Claude baut nur das, was aus der ADR begründet ableitbar ist
- Keine zusätzlichen Agenten „zur Sicherheit“
- Keine Allround-Agenten
- Tools nur minimal und rollenbezogen
- Bestehende Struktur bevorzugen vor neuer Komplexität
- Bei bestehendem Projekt erst lesen, dann schreiben
- Änderungen an bestehenden Dateien klar benennen

---

## Verwendung bestehender Befehle

Wenn die ADR klar genug ist, soll Claude auf den vorhandenen Befehlen aufbauen.

Insbesondere:
- `/new-agent [rolle] [tools]`
- `/new-workflow [name] [beschreibung]`

Wenn Claude diese Befehle nicht direkt automatisiert ausführen kann, soll es:
- die exakten Befehle vorschlagen,
- die Reihenfolge angeben,
- anschließend die betroffenen Dateien im Projekt direkt anlegen oder anpassen, soweit der Kontext es erlaubt.

Beispiel:

```bash
/new-agent reviewer "read_file,run_tests,post_review"
/new-agent coder "read_file,write_file,run_tests"
/new-workflow bugfix-review-flow "Analysiert Bug, erstellt Fix, reviewed Ergebnis"
```

---

## Dialog am Anfang

Claude startet diesen Befehl nicht mit einer offenen Frage, sondern mit einem kurzen Prüfmodus:

1. Welche ADR passt zu `[name]`?
2. Ist sie vollständig genug für den Build?
3. Handelt es sich um Agent-Erweiterung oder Workflow-Neubau?
4. Welche Dateien werden jetzt angelegt oder angepasst?

Dann präsentiert Claude kurz den Plan und fragt:
> Ich habe den Build-Scope aus der ADR abgeleitet. Soll ich jetzt mit dem Anlegen der Dateien beginnen?

Erst nach Zustimmung beginnt der eigentliche Build.

---

## Minimaler Output vor dem Build

Vor jeder Ausführung soll Claude kurz zusammenfassen:
- verwendete ADR-Datei
- Typ des Vorhabens
- zu erzeugende Agenten
- zu erzeugende Workflows
- betroffene bestehende Dateien
- mögliche Risiken

---

## Fehlerfälle

### Keine ADR gefunden

Dann:
- nicht bauen
- auf `/design-agents [name]` verweisen

### Mehrere ADR-Dateien gefunden

Dann:
- Treffer auflisten
- Nutzer wählen lassen
- nicht automatisch die neueste nehmen

### ADR zu unklar

Dann:
- offene Punkte benennen
- gezielte Rückfragen stellen
- erst danach weiterbauen

---

## Abschluss

Nach erfolgreichem Build soll Claude:
- alle angelegten oder geänderten Dateien benennen,
- die nächste sinnvolle manuelle Prüfung empfehlen,
- falls nötig auf weitere gezielte Aufrufe verweisen, z. B. neue Rollen oder Review-Schleifen ergänzen.
