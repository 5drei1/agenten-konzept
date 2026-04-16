# /build-agents

Phase 2: Liest eine ADR und koordiniert den Aufbau der Agenten- und Workflow-Dateien.

> Dieser Befehl ist der **Koordinator** zwischen Plan und Ausführung.
> Er liest die ADR, leitet daraus die Build-Reihenfolge ab und ruft
> `/new-agent` und `/new-workflow` als Primitive auf.

## Verwendung

```bash
/build-agents [name]
```

Beispiel:
```bash
/build-agents bugfix-review-flow
/build-agents angebot-assistent
```

**Voraussetzung:** Eine ADR-Datei unter `adr/[name]_[YYYY-MM-DD].md`,
erstellt durch `/design-agents [name]`.

---

## Dialogregel

> Claude stellt so viele Rückfragen wie nötig – aber jede Frage muss eine Entscheidung im Build verändern.
> Fragen die keinen Einfluss auf Reihenfolge, Dateistruktur oder Implementierung hätten, werden weggelassen.
> Fragen die Claude selbst aus der ADR ableiten kann, werden nicht gestellt.
> Ziel ist ein sauber gebautes System – nicht ein kurzer Dialog.

Nie mehrere Fragen auf einmal stellen. Eine Frage, Antwort abwarten, nächste Frage.

---

## Phase 2A – ADR laden und prüfen

Claude sucht zuerst die passende ADR:

- **Genau eine Datei gefunden** → laden und weiter
- **Mehrere Dateien gefunden** → Treffer auflisten, Nutzer wählen lassen, nie raten
- **Keine Datei gefunden** → abbrechen, auf `/design-agents [name]` verweisen

Aus der ADR extrahiert Claude:
- Typ: Neuer Agent | Kompletter Workflow
- Empfohlene Rollen mit Verantwortlichkeiten und Tools
- Workflow-Pattern
- State-Felder
- Human-in-the-Loop Punkte
- Supervisor nötig: ja/nein
- Build-Plan (Checkbox-Liste aus ADR Abschnitt "Build-Plan")
- Offene Fragen

Wenn die ADR **offene Kernfragen** enthält die den Build blockieren:
- Punkte benennen
- Gezielt klären (Dialogregel gilt)
- Erst danach bauen

Wenn die ADR vollständig ist: direkt zu Phase 2B.

---

## Phase 2B – Build-Plan präsentieren

Vor dem Build präsentiert Claude eine kompakte Übersicht:

```
ADR: adr/[name]_[datum].md
Typ: Neuer Agent | Kompletter Workflow

Build-Reihenfolge:
  1. workflows/state.py          (neu | bereits vorhanden)
  2. agents/[rolle_1].py         (neu)
  3. agents/[rolle_2].py         (neu)
  4. workflows/[name]_flow.py    (neu)
  5. workflows/subgraphs/...     (falls nötig)
  6. agents/__init__.py          (ergänzen)
  7. workflows/__init__.py       (ergänzen)

Bestehende Dateien die angepasst werden:
  - ...

Offene Risiken:
  - ...
```

Dann fragt Claude:
> Soll ich jetzt mit dem Build beginnen?

Erst nach Zustimmung startet Phase 2C.

---

## Phase 2C – Build ausführen

Claude baut in der festgelegten Reihenfolge:

**Immer: State zuerst, dann Agents, dann Workflow, dann Subgraphs.**

### Fall A – Neuer Agent in bestehendem Projekt

```
1. Bestehende agents/, workflows/state.py lesen (github-Plugin falls Repo bekannt)
2. /new-agent [rolle] [tools]  → agents/[rolle].py
3. Anbindung an bestehenden Workflow prüfen und minimal anpassen
4. agents/__init__.py ergänzen
```

Keine neuen Workflows anlegen wenn nicht in ADR vorgesehen.
Keine Anpassungen an bestehenden Dateien ohne explizite Benennung.

### Fall B – Kompletter Workflow

```
1. workflows/state.py            → /new-workflow [name] (State-Teil)
2. agents/[rolle_1].py           → /new-agent [rolle_1] [tools]
3. agents/[rolle_2].py           → /new-agent [rolle_2] [tools]
   ... (alle Rollen aus ADR)
4. workflows/[name]_flow.py      → /new-workflow [name] (Graph-Teil)
5. workflows/subgraphs/*.py      → falls ADR Subgraphs vorsieht
6. agents/__init__.py            → alle neuen Rollen eintragen
7. workflows/__init__.py         → neuen Workflow eintragen
```

---

## Koordinationsregeln

- **Nur bauen was in der ADR steht** – keine Extrapolation, keine Agenten "zur Sicherheit"
- **Reihenfolge einhalten:** State → Agents → Workflow → Subgraphs
- **Bei bestehendem Projekt:** `github`-Plugin nutzen um Ist-Zustand zu lesen bevor geschrieben wird
- **Änderungen an bestehenden Dateien** immer explizit benennen und begründen
- **Bei Pattern-Unsicherheit während Build:** `mcpdoc` aufrufen
- **Jede erzeugte Datei entspricht einem Eintrag im Build-Plan der ADR** – keine Abweichung ohne Rückfrage

---

## Fehlerfälle

| Situation | Verhalten |
|---|---|
| Keine ADR gefunden | Abbrechen → `/design-agents [name]` |
| Mehrere ADR gefunden | Auflisten → Nutzer wählen lassen |
| ADR hat offene Kernfragen | Klären (Dialogregel) → dann bauen |
| Bestehende Datei würde überschrieben | Explizit warnen → Zustimmung einholen |
| Pattern unklar | `mcpdoc` aufrufen → dann entscheiden |

---

## Abschluss

Nach erfolgreichem Build:

```
Angelegte Dateien:
  - agents/[rolle_1].py
  - agents/[rolle_2].py
  - workflows/state.py
  - workflows/[name]_flow.py

Angepasste Dateien:
  - agents/__init__.py
  - workflows/__init__.py

Empfohlene nächste Schritte:
  - State-Felder auf Vollständigkeit prüfen
  - System-Prompts der Agenten schärfen
  - Ersten manuellen Testlauf starten
```

Falls weitere Rollen oder Subgraphs später ergänzt werden sollen:
```bash
/new-agent [neue-rolle] [tools]
/new-workflow [subgraph-name] [beschreibung]
```
