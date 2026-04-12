# Tools, Integrationen und Skills

## Tools sind nicht Agenten
Agenten entscheiden und formulieren. Tools führen technische Aktionen aus.

## Typische Tool-Klassen

- Dateizugriff
- Git und Repository-Zugriff
- Testausführung
- E-Mail-Entwürfe
- Datenbank- und API-Zugriffe
- Suche, Crawling und Extraktion

## Skills
Ein Skill ist eine wiederverwendbare Fähigkeit. Beispiele:

- `load_project_context`
- `find_relevant_files`
- `generate_regression_test`
- `draft_pr_description`

Skills können als einzelne Funktionen, Hilfsbausteine oder Subgraphs umgesetzt werden.

## Integrationen
Kundenprozesse benötigen oft Anbindungen an:

- CRM
- ERP
- Ticketsysteme
- Mailboxen
- interne APIs

Diese Integrationen sollten über klar definierte Tools oder Adapter angesprochen werden.

## Best Practice
Agenten bekommen nur die Tools, die sie wirklich brauchen. Tool-Rechte sollten durch Policies begrenzt werden.