# /design-agents

Phase 1 des Agenten-Designs: Use Case herausarbeiten, Architektur ableiten, ADR anlegen.

## Verwendung

```bash
/design-agents [name]
```

Beispiel:
```bash
/design-agents bugfix-review-flow
/design-agents angebot-assistent
```

`[name]` wird der Dateiname der ADR: `adr/[name]_[YYYY-MM-DD].md`

Diese Datei ist die Grundlage für Phase 2: `/build-agents [name]`

---

## Dialogregel

> Claude stellt so viele Rückfragen wie nötig – aber jede Frage muss eine Entscheidung im Design verändern.
> Fragen die keinen Einfluss auf Rollen, Pattern, Datenfluss oder HITL-Punkte hätten, werden weggelassen.
> Fragen die Claude selbst aus dem Kontext ableiten kann, werden nicht gestellt.
> Ziel ist eine belastbare ADR – nicht ein kurzer Dialog.

Nie mehrere Fragen auf einmal stellen. Eine Frage, Antwort abwarten, nächste Frage.

---

## Verfügbare MCP-Plugins

| Plugin | Einsatz im Design-Prozess |
|---|---|
| **mcpdoc** | LangGraph/LangChain Patterns live verifizieren – bevor Empfehlung gemacht wird |
| **context7** | Python-Library-Docs on-demand – wenn Tools und Libraries diskutiert werden |
| **github** | Bestehende Repo-Struktur lesen – bei bestehendem Projekt vor jeder Empfehlung |

**Wann Plugins nutzen:**
- Bestehendes Projekt genannt → erst `github` lesen, dann empfehlen
- Pattern oder API unsicher → `mcpdoc` aufrufen
- Library-Fragen zu Tools → `context7`

Nicht blind empfehlen was vielleicht veraltet ist. Lieber kurz nachschlagen.

---

## Arbeitsweise

### Phase 1A – Use Case formulieren

Claude beginnt **immer mit einer offenen Frage**, nie mit einer Checkliste.

Erste Frage:
> Was möchtest du automatisieren oder unterstützen – und woran merkst du heute, dass der Ablauf noch nicht gut gelöst ist?

**Ziel:** Problem verstehen, nicht Lösung entgegennehmen.

Wenn der Nutzer zu schnell in Lösungssprache springt ("ich brauche 3 Agenten"), führt Claude zurück:
- Welches Problem löst das konkret?
- Was passiert heute manuell oder fehleranfällig?
- Was ist der gewünschte Endzustand?

Claude fragt so lange bis Trigger, Ziel, Grenzen und Datenfluss des Use Cases klar sind.
Jede Frage muss eine dieser Dimensionen klären – sonst wird sie nicht gestellt.

---

### Phase 1B – Scope erkennen

Nach Phase 1A entscheidet Claude zwischen zwei Fällen:

#### Fall A – Neuer Agent in bestehendem Projekt

**Erkennungsmerkmale:**
- Bestehendes Projekt / Graph vorhanden
- Eine Rolle fehlt oder ist zu groß
- Integration in bestehende Struktur nötig

**Dann:** `github`-Plugin nutzen um Ist-Struktur zu lesen (agents/, workflows/, state.py), bevor Empfehlungen gemacht werden.

Claude klärt so lange bis klar ist:
- Was ist Eingabe und Output der neuen Rolle?
- Welche bestehenden Nodes grenzen daran an?
- Was ist ausdrücklich nicht Verantwortung dieser Rolle?

#### Fall B – Kompletter Workflow

**Erkennungsmerkmale:**
- Mehrere Rollen / Schritte nötig
- Noch kein Graph vorhanden
- Routing, Review oder Human-in-the-Loop wahrscheinlich relevant

**Dann:** `mcpdoc` nutzen um das passende LangGraph-Pattern zu verifizieren.

Claude klärt so lange bis klar ist:
- Was ist der Trigger, was ist das finale Ergebnis?
- Welche Schritte hängen voneinander ab, welche sind parallel?
- Wo braucht es Freigabe, Review oder Qualitätsprüfung?
- Welche Daten müssen zwischen Rollen übergeben werden?

---

### Phase 1C – Architektur ableiten

Erst wenn Use Case und Scope vollständig klar sind, leitet Claude die Architektur ab.

**Grundregeln:**
- Workflow first – Agenten sind Konsequenz des Workflows, nicht umgekehrt
- Ein Agent = eine Rolle, keine Allrounder
- Tools nur dort wo wirklich gebraucht, mit Begründung
- Human-in-the-Loop nur bei echtem Risiko oder Qualitätserfordernis
- Bestehende Strukturen bevorzugen

**Claude leitet ab:**
- Fall A oder Fall B?
- Welche Agenten-Rollen mit welchen Verantwortlichkeiten?
- Welches Pattern: Sequential Chain / Parallel Fan-Out / Review Loop / Conditional Routing?
- Braucht es einen Supervisor-Agenten?
- Welche Tools je Rolle, mit Begründung?
- Human-in-the-Loop: ja/nein – wenn ja: wo und mit welchem `interrupt()`-Punkt?
- Welche State-Felder werden wirklich gebraucht?
- Welche Dateien legt `/build-agents` an, in welcher Reihenfolge?

**Bei Pattern-Unsicherheit:** `mcpdoc` aufrufen bevor Empfehlung gemacht wird.

---

## Schluss-Check vor ADR

Claude fasst kurz zusammen:

```
Verstandener Use Case: ...
Einordnung: Neuer Agent | Kompletter Workflow
Empfohlene Rollen: ...
Pattern: ...
Größte offene Risiken: ...
```

Dann fragt Claude:
> Soll ich die ADR `adr/[name]_[datum].md` jetzt anlegen?

Nur nach Zustimmung wird die Datei angelegt.

---

## ADR-Format

```md
# ADR: [Titel]
Datum: [YYYY-MM-DD]

## Kontext
- Fachliches Problem
- Was passiert heute
- Was soll besser werden

## Einordnung
- Typ: Neuer Agent | Kompletter Workflow
- Scope: eng | mittel | breit

## Zielbild
- Was ist das gewünschte Ergebnis?
- Was ist ausdrücklich NICHT Teil dieses Designs?

## Architektur
- Rollen und Verantwortlichkeiten
- Workflow-Pattern (mit Begründung)
- State-Felder (nur benötigte)
- Tools je Rolle (mit Begründung)
- Human-in-the-Loop: nein | ja → wo + interrupt()-Punkt
- Supervisor nötig: nein | ja

## Datenfluss
- Trigger: ...
- Schritte und Übergaben
- Finales Ergebnis

## Offene Fragen
- Was ist noch unklar?
- Was muss vor /build-agents entschieden werden?

## Build-Plan
- [ ] agents/[rolle].py
- [ ] workflows/state.py
- [ ] workflows/[name]_flow.py
- [ ] workflows/subgraphs/... (falls nötig)
Reihenfolge: State → Agents → Workflow → Subgraphs
```

---

## Qualitätsregeln

- Niemals Allround-Agenten empfehlen
- Niemals Tools ohne klare Begründung
- Niemals Pattern empfehlen ohne Verifikation ob es passt
- Bei Unsicherheit über APIs/Patterns: `mcpdoc` aufrufen
- Bei bestehendem Projekt: erst `github` lesen, dann empfehlen
- Unklarheiten in ADR als offene Fragen dokumentieren, nicht erfinden
- Eine Frage zur Zeit – nie mehrere auf einmal

---

## Übergabe an Phase 2

```
ADR gespeichert unter: adr/[name]_[datum].md

Prüfe den Plan.
Wenn alles passt:
/build-agents [name]
```
