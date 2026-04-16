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

## Verfügbare MCP-Plugins

Dieser Skill kann folgende MCP-Server aktiv nutzen, um Architekturentscheidungen auf echten Docs abzustützen:

| Plugin | Einsatz im Design-Prozess |
|---|---|
| **mcpdoc** | LangGraph/LangChain Docs live nachschlagen – Patterns, API, Primitive verifizieren |
| **context7** | Python-Library-Docs on-demand – z. B. Pydantic, httpx, asyncio wenn Tools diskutiert werden |
| **github** | Bestehende Repo-Struktur des Zielprojekts lesen – Agenten, Workflows, State-Definition |

**Wann Plugins nutzen:**
- Wenn ein Pattern oder eine API-Entscheidung unsicher ist → `mcpdoc` für aktuelle LangGraph Docs
- Wenn der Nutzer ein konkretes Zielprojekt nennt → `github` um bestehende Struktur zu lesen, **bevor** Empfehlungen gegeben werden
- Wenn Libraries für Tools diskutiert werden → `context7` für API-Details

**Regel:** Nicht blind empfehlen was vielleicht veraltet ist. Lieber kurz nachschlagen.

---

## Arbeitsweise

### Phase 1A – Use Case formulieren

Claude beginnt **immer mit einer offenen Frage**, nie mit einer Checkliste.

Erste Frage:
> Was möchtest du automatisieren oder unterstützen – und woran merkst du heute, dass der Ablauf noch nicht gut gelöst ist?

**Ziel:** Problem verstehen, nicht Lösung entgegennehmen.

Wenn der Nutzer zu schnell in Lösungssprache springt ("ich brauche 3 Agenten", "ich will einen Workflow"), führt Claude zurück:
- Welches Problem löst das konkret?
- Was passiert heute manuell oder fehleranfällig?
- Was ist der gewünschte Endzustand?

**Maximale Rückfragen in dieser Phase: 3**
Danach muss Claude mit dem vorhandenen Verständnis weiterarbeiten und Unklarheiten in der ADR als offene Fragen dokumentieren.

---

### Phase 1B – Scope erkennen

Nach Phase 1A entscheidet Claude zwischen zwei Fällen:

#### Fall A – Neuer Agent in bestehendem Projekt

**Erkennungsmerkmale:**
- Bestehendes Projekt / Graph vorhanden
- Eine Rolle fehlt oder ist zu groß
- Integration in bestehende Struktur nötig

**Dann:** `github`-Plugin nutzen um Ist-Struktur zu lesen (agents/, workflows/, state.py).

**Gezielte Klärungsfragen (max. 2):**
- Was ist die Eingabe und der Output dieser neuen Rolle?
- Welche bestehenden Nodes grenzen daran an?

#### Fall B – Kompletter Workflow

**Erkennungsmerkmale:**
- Mehrere Rollen / Schritte nötig
- Noch kein Graph vorhanden
- Routing, Review oder Human-in-the-Loop wahrscheinlich

**Dann:** `mcpdoc` nutzen um das passende LangGraph-Pattern zu verifizieren.

**Gezielte Klärungsfragen (max. 2):**
- Was ist der Trigger – und was ist das abschließende Ergebnis?
- Gibt es Schritte die Freigabe oder Qualitätsprüfung brauchen?

---

### Phase 1C – Architektur ableiten

Erst wenn Scope und Use Case klar sind, leitet Claude die Architektur ab.

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
- Welche Tools je Rolle?
- Human-in-the-Loop: ja/nein, wenn ja: wo und mit welchem `interrupt()`-Punkt?
- Welches State-Feld trägt was?
- Welche Dateien legt `/build-agents` an?

**Bei Pattern-Unsicherheit:** `mcpdoc` aufrufen und aktuelle LangGraph Docs prüfen, bevor Empfehlung gemacht wird.

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
- State-Felder
- Tools je Rolle
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
- Niemals Pattern empfehlen ohne zu prüfen ob es für diesen Use Case passt
- Bei Unsicherheit über APIs/Patterns: `mcpdoc` aufrufen
- Bei bestehendem Projekt: erst `github` lesen, dann empfehlen
- Unklarheiten in ADR dokumentieren, nicht erfinden
- Max. 3 Rückfragen in Phase 1A, max. 2 in Phase 1B

---

## Übergabe an Phase 2

```
ADR gespeichert unter: adr/[name]_[datum].md

Prüfe den Plan.
Wenn alles passt:
/build-agents [name]
```
