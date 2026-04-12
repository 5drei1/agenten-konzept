# Repository Map

## Zweck
Diese Datei hilft Agenten dabei, schnell zu verstehen, welche Ordner im Repository welche Rolle haben.

## Einstieg für menschliche Leser
- `PITCH.md` – Kompakte Präsentationsübersicht mit Diagrammen (Bewerbung / Kunden-Pitch)
- `README.md` – Überblick und Orientierung (Startpunkt)
- `docs/00_vision_und_ziele.md` – Vollständige Konzepterklärung

## Hauptordner

### `.project_knowlage/`
Kompakte Arbeitsgrundlage für Agenten. Enthält die verdichtete Projektsicht – schneller lesbar als die ausführliche Dokumentation. Kein Einstieg für menschliche Leser.

### `docs/`
Ausführliche Konzeptdokumentation. Hier liegen Vision, Architektur, Ordnerstruktur, Projektwissen, Datenschutz, Deployment und Best Practices.

### `examples/`
Praxisnahe Beispielbeschreibungen für Coding-Projekte und Kundenprozesse.

### `faq/`
Sammlung wichtiger Fragen und Antworten, die während der gemeinsamen Ausarbeitung entstanden sind.

### `templates/`
Vorlagen für Project Profiles, Workflows und Agenten.

## Dateien in `docs/`

### `docs/00_vision_und_ziele.md`
Warum das Repository existiert, die Leitidee und das Zielbild. Einstieg für menschliche Leser.

### `docs/01_kernbegriffe.md`
Definitionen aller zentralen Begriffe: Projekt, Agent, Workflow, Tool, Skill, Projektwissen, State, Policy, Approval, Subgraph.

### `docs/02_architekturueberblick.md`
Rollen von LangChain, LangGraph, LangSmith und der eigenen Management-Schicht.

### `docs/03_ordnerstruktur.md`
Empfohlene Ordnerstruktur für Framework- und Projektrepos. Inkl. Tool-Ordner, Subgraph-Struktur und Entscheidungsregel Tool vs. Subgraph.

### `docs/04_projekte_agenten_workflows.md`
Das Drei-Ebenen-Modell: Projekt, Agent, Workflow – und was Workflows neben Agenten noch aufrufen.

### `docs/05_projektwissen_und_kontext.md`
Wie Projektwissen geladen wird und warum es primär in den Workflow gehört, nicht in den Agenten.

### `docs/06_tools_integrationen_und_skills.md`
Tool-Definition in LangChain (@tool, StructuredTool), bind_tools(), ToolNode, Entscheidungsregel Tool vs. Subgraph, Skill-Begriff.

### `docs/07_policies_freigaben_datenschutz.md`
Freigabestufen, PII-Schutz-Muster und Governance-Regeln.

### `docs/08_beobachtung_evals_qualitaet.md`
Was beobachtet wird, Qualitätsachsen und Eval-Sets.

### `docs/09_deployment_und_kundenauslagerung.md`
Drei Deployment-Modelle (zentral, hybrid, self-hosted) und Muster für Kundenprojekte.

### `docs/10_best_practices_und_entscheidungen.md`
Architekturprinzipien und Entscheidungsregeln. Verweist auf docs/12 für Ausbaustufen.

### `docs/11_wissensablage_lokal_vs_zentral.md`
Wissensstrategie: was lokal bleibt, was zentral liegt.

### `docs/12_minimales_system.md`
Drei Ausbaustufen (Minimal, Mittel, Komplex) mit konkreten Inhalten pro Stufe und Übergangskriterien. Einstiegspunkt für: Womit fange ich an?

## Wie Agenten navigieren sollten

### Wenn neue Konzeptinhalte entstehen
- zuerst `.project_knowlage/` lesen
- dann passende Datei in `docs/` suchen
- nur dann neue Datei anlegen, wenn bestehende Dateien nicht sinnvoll erweitert werden können

### Wenn ein Beispiel ergänzt werden soll
- zuerst `examples/` prüfen
- ähnliches Beispiel erweitern oder neues Beispiel sauber abgrenzen

### Wenn eine Frage auftaucht
- zuerst `faq/` prüfen
- vorhandene Antwort ergänzen, statt sofort neue Doku zu schreiben

### Wenn eine Vorlage gebraucht wird
- zuerst `templates/` prüfen
- dort gemeinsame Muster pflegen, nicht in zufälligen Doku-Dateien

## Aktuelle Navigationsregel
Für dieses Repository gilt:

- `.project_knowlage/` = agentenfreundliche Kurzsicht
- `docs/` = ausführliche Hauptdokumentation
- `examples/` = Anwendungsbeispiele
- `faq/` = wiederkehrende Klärungen
- `templates/` = strukturierte Vorlagen

## Ziel für Agenten
Ein Agent soll anhand dieser Datei schnell entscheiden können:
- wo er lesen muss
- wo er schreiben darf
- ob er bestehende Inhalte erweitert oder neue Dateien anlegt