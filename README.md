# agenten-konzept

Dieses Repository beschreibt, wie KI-gestützte Agenten-Systeme strukturiert, organisiert und betrieben werden können – ressourceneffizient, nachvollziehbar und wiederverwendbar.

## Das Problem

Viele Agenten-Systeme scheitern nicht an der Technologie, sondern am Aufbau: Ein Agent soll alles können, kennt zu viel oder zu wenig Kontext, und wenn er falsch entscheidet, weiß niemand warum.

## Die Grundidee

**Erst der Ablauf, dann die Agenten.**

Ein Workflow beschreibt, was passiert. Agenten übernehmen klar abgegrenzte Rollen darin. Projektwissen wird gezielt geladen – nicht pauschal in jeden Agenten kopiert.

## Fünf Leitregeln

1. **Workflow first** – Abläufe werden zuerst als Prozesse gedacht, nicht als Agenten-Aufgaben
2. **Spezialisierte Agenten** – Klein, klar definiert, austauschbar
3. **Projektwissen im Workflow** – Kontext wird gezielt geladen, nicht global verteilt
4. **Lokal by default** – Projektwissen liegt nah am Projekt, nicht zentral für alles
5. **Datenschutz als Architektur** – PII-Schutz ist Teil des Workflow-Designs, kein Nachgedanke

## Inhalt

| Ordner | Inhalt |
|---|---|
| `docs/` | Vollständiges Konzept: Architektur, Bausteine, Patterns, Best Practices |
| `examples/` | Konkrete Beispiele: Bugfix-Workflow, Angebotsprozess, PII-Gate |
| `faq/` | Häufige Fragen zu Agenten, Projektwissen, Tools und Wissensablage |
| `templates/` | Vorlagen für Agenten, Workflows und Projektprofile |
| `.project_knowlage/` | Komprimierte Arbeitssicht für KI-Agenten |

## Wo anfangen?

**Konzept verstehen** → [`docs/00_vision_und_ziele.md`](docs/00_vision_und_ziele.md)

**Architektur und Technik** → [`docs/02_architekturueberblick.md`](docs/02_architekturueberblick.md)

**Direkt ein Beispiel** → [`examples/coding_project/beispiel_bugfix_workflow.md`](examples/coding_project/beispiel_bugfix_workflow.md)
