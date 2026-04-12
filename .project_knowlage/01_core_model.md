# Core Model

## Kurzfassung
Das Konzept dieses Repositories basiert auf wenigen klar getrennten Bausteinen:

- Projekte
- Agenten
- Workflows
- Tools
- Projektwissen
- Policies und Freigaben
- Beobachtung und Evals

Diese Trennung ist die fachliche Wahrheit des Repositories.

## Aktuelle Einordnung des Repositories
Dieses Repository ist aktuell ein **Konzept- und Dokumentations-Repository**.

Es dient dazu:
- das Modell verständlich zu machen
- Begriffe und Zusammenhänge zu schärfen
- eine strukturierte Grundlage für spätere Entscheidungen zu schaffen
- das Konzept auch als Grundlage eines Business-Modells zu beschreiben

Es ist aktuell **kein Referenzcode- oder Architektur-Repo**.

## Die Bausteine

### Projekt
Ein Projekt ist der fachliche und technische Rahmen eines Vorhabens. Es kann ein Coding-Projekt, ein Kundenprozess oder eine dauerhafte Aufgabe sein.

### Agent
Ein Agent ist eine Rolle mit klaren Instruktionen, erlaubten Tools, Grenzen und einem erwarteten Ausgabeformat.

### Workflow
Ein Workflow ist ein geordneter Ablauf. Er verbindet Agenten, Tools, Projektwissen, Regeln und Freigaben.

### Tool
Ein Tool ist eine technische Fähigkeit, zum Beispiel Dateizugriff, Git-Zugriff, Tests, API-Aufrufe oder E-Mail-Entwürfe.

### Projektwissen
Projektwissen ist langlebiger, projektspezifischer Kontext. Es umfasst zum Beispiel Architektur, Regeln, Repo-Metadaten, Domänenwissen oder Kundenbesonderheiten.

### Policy
Policies definieren Grenzen, Rechte, Datenschutzregeln, Freigaben und verbotene Aktionen.

### State
State ist der laufende Kontext eines Workflow-Runs. Dort liegen Eingaben, Zwischenergebnisse, Entscheidungen und Übergaben.

## Zentrale Architekturregeln

### Agent = Fähigkeit
Ein Agent ist keine komplette Anwendung, sondern eine klar abgegrenzte Rolle.

### Workflow = Ablaufsteuerung
Der Workflow entscheidet, welche Schritte ausgeführt werden, welches Wissen geladen wird und welche Agenten oder Tools beteiligt sind.

### Projektwissen = Kontextschicht
Projektwissen wird nicht blind überall repliziert. Es wird gezielt geladen und dem jeweiligen Workflow-Lauf passend injiziert.

## Wissensregel
Für dieses Repository gilt:

**Local by default, central by exception.**

Das heißt:
- projektspezifisches Wissen liegt bevorzugt direkt im Projekt oder Kundenrepo
- das zentrale Konzept-Repo enthält vor allem Standards, Templates, Best Practices und Referenzmodelle

## Technologischer Fokus
Für dieses Konzept ist der Fokus aktuell bewusst auf:
- LangChain
- LangGraph

gelegt.

Diese Festlegung ist strategisch und konzeptionell gewollt. Das Repository soll also nicht primär alternative Frameworks gegeneinander auswerten, sondern ein belastbares Modell auf Basis dieses Stacks ausarbeiten.

## Was Agenten bei Änderungen beachten müssen
- Begriffe konsistent halten
- Workflow-first nicht unterlaufen
- Projektwissen nicht als festen Agentenbesitz darstellen
- zentrale und lokale Wissensschichten sauber unterscheiden
- Datenschutz aktuell nicht künstlich zum Hauptfokus machen
- das Repository als Konzept- und Strategierepo behandeln, nicht als Code-Repo

## Merksätze
- Agenten liefern Fähigkeiten.
- Workflows steuern Abläufe.
- Tools führen Aktionen aus.
- Projektwissen liefert Kontext.
- Policies begrenzen Macht und Risiko.