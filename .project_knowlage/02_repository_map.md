# Repository Map

## Zweck
Diese Datei hilft Agenten dabei, schnell zu verstehen, welche Ordner im Repository welche Rolle haben.

## Hauptordner

### `.project_knowlage/`
Kompakte Arbeitsgrundlage für Agenten. Dieser Ordner enthält die verdichtete Projektsicht und soll schneller lesbar sein als die ausführliche Dokumentation.

### `docs/`
Ausführliche Konzeptdokumentation. Hier liegen Vision, Architektur, Ordnerstruktur, Projektwissen, Datenschutz, Deployment und Best Practices.

### `examples/`
Praxisnahe Beispielbeschreibungen für Coding-Projekte und Kundenprozesse.

### `faq/`
Sammlung wichtiger Fragen und Antworten, die während der gemeinsamen Ausarbeitung entstanden sind.

### `templates/`
Vorlagen für Project Profiles, Workflows und Agenten.

## Wichtige Dateien in `docs/`

### `docs/00_vision_und_ziele.md`
Warum das Repository existiert und welches Zielbild verfolgt wird.

### `docs/02_architekturueberblick.md`
Erklärt die Rollen von LangChain, LangGraph, LangSmith und der eigenen Fachschicht.

### `docs/03_ordnerstruktur.md`
Beschreibt die empfohlene Trennung zwischen Agenten, Workflows, Framework-Elementen und projektlokalem Wissen.

### `docs/05_projektwissen_und_kontext.md`
Erklärt, wie Projektwissen geladen wird und warum es primär in den Workflow gehört.

### `docs/11_wissensablage_lokal_vs_zentral.md`
Definiert die aktuelle Wissensstrategie des Repositories.

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