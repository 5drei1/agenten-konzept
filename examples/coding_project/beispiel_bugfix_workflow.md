# Beispiel: Bugfix-Workflow für ein Coding-Projekt

## Ausgangslage
Ein bestehendes Projekt hat einen Fehler. Das Ziel ist ein wiederverwendbarer Workflow, der projektbezogen arbeitet, ohne den Agenten selbst umzubauen.

## Beispielstruktur

- Projektwissen: direkt im Projekt unter `project-knowledge/`
- Repo: `workspace/repos/payment_service/` oder direktes Kundenrepo
- Workflow: `workflows/bugfix/`
- Agenten: Planner, Coder, Reviewer

## Ablauf
1. `load_project_context`
2. Ticket und Stacktrace laden
3. relevante Dateien ermitteln
4. Planner erstellt Vorgehensplan
5. Coder setzt Änderung um
6. Tests ausführen
7. Reviewer bewertet Ergebnis
8. PR-Text oder Zusammenfassung erzeugen

## Was der Planner bekommt
- Ticketbeschreibung
- Stacktrace
- Projektprofil oder `project-knowledge/`
- relevante Dateien
- Coding-Regeln

## Was der Coder bekommt
- Plan des Planners
- relevante Codeausschnitte
- Teststrategie
- Projektregeln

## Ergebnis
Der Workflow ist wiederverwendbar. Das Projektwissen wird aus dem Projektkontext und dem Repo geladen.