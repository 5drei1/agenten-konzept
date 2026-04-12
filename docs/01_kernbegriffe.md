# Kernbegriffe

## Projekt
Ein Projekt ist der fachliche und technische Container eines Vorhabens. Es kann ein oder mehrere Repositories, einen Prozess oder eine dauerhafte Aufgabe in einem Unternehmen umfassen.

## Agent
Ein Agent ist eine Rolle mit klaren Instruktionen, erlaubten Tools, Grenzen und einem erwarteten Ausgabeformat. Ein Agent ist logisch keine Datei, kann praktisch aber als Python-Modul umgesetzt werden.

## Workflow
Ein Workflow ist ein geordneter Ablauf. Er ruft Agenten, Tools, Freigaben und Projektwissen in einer definierten Reihenfolge auf.

## Tool
Ein Tool ist eine technische Fähigkeit wie Dateilesen, Git-Zugriff, Tests ausführen, API-Aufrufe oder E-Mails entwerfen.

## Skill
Ein Skill ist eine wiederverwendbare Fähigkeit oder Teilaufgabe, zum Beispiel `load_project_context` oder `generate_regression_test`.

## Projektwissen
Projektwissen ist langlebiger, projektspezifischer Kontext wie Architektur, Coding-Regeln, relevante Pfade, Policies oder Kundenlogik.

## State
State ist der laufende Kontext eines Workflow-Runs. Darin liegen aktuelle Eingaben, Zwischenergebnisse, Pläne und Entscheidungen.

## Policy
Eine Policy beschreibt Regeln und Grenzen, zum Beispiel Tool-Rechte, Datenschutz, Genehmigungen oder verbotene Aktionen.

## Approval
Approval ist eine bewusste Freigabe durch Mensch oder Regelwerk, bevor riskante Aktionen stattfinden.

## Subgraph
Ein Subgraph ist ein wiederverwendbarer Teilworkflow innerhalb eines größeren Graphen.