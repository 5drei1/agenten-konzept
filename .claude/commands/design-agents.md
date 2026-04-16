# /design-agents

Hilft dabei, einen Use Case zuerst sauber zu formulieren und daraus einen umsetzbaren Agenten- oder Workflow-Plan abzuleiten.

## Zweck

Dieser Befehl ist **Phase 1: Design**.
Er dient nicht dazu, sofort Code zu erzeugen, sondern zuerst den eigentlichen Use Case sauber herauszuarbeiten.

Claude soll den Nutzer dabei unterstützen,
- den fachlichen Kern eines Vorhabens zu formulieren,
- zwischen **neuem Agenten in bestehendem Projekt** und **komplettem Workflow** zu unterscheiden,
- passende Agenten-Rollen und Workflow-Patterns abzuleiten,
- daraus einen belastbaren Plan als ADR zu erzeugen.

Der Befehl soll bewusst dialogisch arbeiten. Der erste Input des Nutzers ist oft noch unscharf, lösungsorientiert oder unvollständig. Claude hilft deshalb zuerst beim **Formulieren des Use Cases**, bevor es Architektur empfiehlt.

## Verwendung

```bash
/design-agents [name]
```

Beispiel:
```bash
/design-agents bugfix-review-flow
/design-agents angebot-assistent
/design-agents code-review-agent
```

## Ergebnis

Claude erstellt eine ADR-Datei unter:

```bash
adr/[name]_[YYYY-MM-DD].md
```

Beispiel:
```bash
adr/bugfix-review-flow_2026-04-16.md
```

Diese Datei dient als Grundlage für Phase 2 mit `/build-agents [name]`.

---

## Arbeitsweise

### Phase 1A – Use Case formulieren

Claude beginnt **immer offen** und nicht mit einer Checkliste.

Erste Frage sinngemäß:
> Was möchtest du automatisieren oder unterstützen, und woran merkst du heute, dass der Ablauf noch nicht gut gelöst ist?

Ziel dieser ersten Phase:
- Problem vor Lösung verstehen
- gewünschten Ablauf erfassen
- Auslöser, Ziel, Grenzen und Ergebnis klären
- implizite Annahmen sichtbar machen

Wenn der Nutzer zu schnell in Lösungssprache springt (z. B. „ich brauche 3 Agenten“), führt Claude zurück auf den fachlichen Kern:
- Welches Problem lösen diese Agenten?
- Was passiert heute manuell?
- Was ist der gewünschte Endzustand?
- Wo beginnt und endet der Ablauf?

Claude soll in dieser Phase **wenige, aber gute Rückfragen** stellen.
Nicht interviewartig alles auf einmal, sondern iterativ, bis der Use Case verständlich ist.

### Phase 1B – Scope unterscheiden

Sobald der Use Case verständlich ist, muss Claude entscheiden, welcher von zwei Fällen vorliegt:

#### Fall A – Neuer Agent in bestehendem Projekt

Merkmale:
- Es gibt bereits einen bestehenden Ablauf oder Graphen
- Es fehlt eine klar abgegrenzte Rolle
- Der neue Agent soll in vorhandene Struktur eingebettet werden

Dann klärt Claude gezielt:
- Welche Aufgabe übernimmt der neue Agent genau?
- Was ist seine Eingabe?
- Was ist sein Output?
- Welche bestehenden Agenten oder Workflows grenzen daran an?
- Welche Tools braucht er wirklich?
- Welche Verantwortung soll **nicht** in diesen Agenten?

#### Fall B – Kompletter Workflow

Merkmale:
- Es geht nicht nur um eine Rolle, sondern um einen ganzen Ablauf
- Mehrere Schritte oder Rollen sind nötig
- Routing, Review oder Human-in-the-Loop sind wahrscheinlich relevant

Dann klärt Claude gezielt:
- Was ist der Trigger des Workflows?
- Welche Hauptschritte gibt es?
- Welche Schritte hängen voneinander ab?
- Welche Schritte sind unabhängig und parallelisierbar?
- Wo braucht es Review, Freigabe oder Routing?
- Welches Ergebnis muss am Ende vorliegen?

### Phase 1C – Architektur ableiten

Erst wenn der Use Case klar genug ist, leitet Claude aus dem Konzept die Architektur ab.

Dabei gelten diese Grundregeln:
- Erst Workflow, dann Agenten
- Ein Agent = eine klar definierte Rolle
- Tools nur dort binden, wo sie wirklich gebraucht werden
- Review und Human-in-the-Loop nur dort, wo Risiko oder Qualität es verlangen
- Bestehende Struktur bevorzugen statt unnötig neue Komplexität einzuführen

Claude soll folgende Punkte ableiten:
- Handelt es sich um **Agent-Erweiterung** oder **Workflow-Neubau**?
- Welche Agenten-Rollen sind nötig?
- Welches Workflow-Pattern passt? (Sequential Chain, Parallel Fan-Out, Review Loop, Conditional Routing)
- Welche Tools/MCP-Fähigkeiten sind voraussichtlich nötig?
- Welche Artefakte sollten in Phase 2 erzeugt werden?

---

## ADR-Struktur

Claude erstellt eine kompakte, aber belastbare ADR-Datei mit folgendem Aufbau:

```md
# ADR: [Titel]

## Kontext
- Worum geht es fachlich?
- Was ist heute das Problem?
- Was soll verbessert oder automatisiert werden?

## Einordnung
- Typ: Neuer Agent in bestehendem Projekt | Kompletter Workflow
- Scope: eng | mittel | breit

## Zielbild
- Was soll der Nutzer am Ende können?
- Was ist ausdrücklich nicht Teil dieses Designs?

## Empfohlene Architektur
- Empfohlene Agenten-Rollen
- Verantwortlichkeit je Rolle
- Empfohlenes Workflow-Pattern
- Benötigte Tools/Fähigkeiten
- Human-in-the-Loop ja/nein, falls ja wo

## Eingaben und Ausgaben
- Zentrale Inputs
- Erwartete Outputs
- Übergaben zwischen Rollen

## Offene Fragen
- Was ist noch unklar?
- Was muss vor Phase 2 entschieden werden?

## Build-Plan
- Welche Dateien soll `/build-agents [name]` anlegen?
- In welcher Reihenfolge?
```

---

## Ausgabe-Regeln im Dialog

Claude soll am Ende nicht nur eine Empfehlung geben, sondern diese **begründen**.

Die Schlussausgabe vor dem Schreiben der ADR sollte deshalb enthalten:
- Kurzfassung des verstandenen Use Cases
- Einordnung: neuer Agent oder kompletter Workflow
- empfohlene Rollen
- empfohlenes Pattern
- größte offene Risiken oder Unklarheiten

Dann fragt Claude:
> Ich habe daraus einen belastbaren Plan abgeleitet. Soll ich die ADR-Datei `adr/[name]_[datum].md` jetzt anlegen?

Nur nach Zustimmung legt Claude die Datei an.

---

## Qualitätsregeln

- Nicht zu früh Lösung vorschlagen
- Erst Problemverständnis, dann Architektur
- Keine Allround-Agenten empfehlen
- Keine Tools ohne klare Begründung
- Bei Unsicherheit lieber offene Fragen dokumentieren als falsche Sicherheit erzeugen
- Bestehende Workflows respektieren, wenn es um Agent-Erweiterung geht
- Immer zwischen **fachlichem Problem**, **Agentenrolle** und **technischer Implementierung** unterscheiden

---

## Übergabe an Phase 2

Wenn die ADR geschrieben wurde, endet dieser Befehl mit einem klaren nächsten Schritt:

```bash
Prüfe die ADR.
Wenn der Plan passt, starte:
/build-agents [name]
```
