# Knowledge Strategy

## Zweck dieser Datei
Diese Datei beschreibt, wo Wissen in diesem Repository liegen soll und wie Agenten damit umgehen sollen.

## Leitregel
**Local by default, central by exception.**

## Bedeutung für dieses Repository
Das Repository selbst ist das zentrale Konzept- und Framework-Repo. Trotzdem soll Wissen auch hier klar getrennt werden:

- `.project_knowlage/` = kompakte, agentenfreundliche Arbeitsgrundlage
- `docs/` = ausführliche Hauptdokumentation
- `faq/` = wiederkehrende Fragen und Antworten
- `examples/` = konkrete Anwendungsbeispiele
- `templates/` = wiederverwendbare Vorlagen

## Was in `.project_knowlage/` gehört
- verdichtete Projektsicht
- Kernmodell
- Agentenrollen
- Workflow-Muster
- Wissensstrategie
- Navigationshilfe

## Was nicht in `.project_knowlage/` gehört
- komplette Langtexte aus `docs/`
- beliebige Rohnotizen
- doppelte Kopien ganzer Dokumente
- unstrukturierte Zwischenstände

## Was in `docs/` gehört
- ausführliche Erklärungen
- Architektur- und Strukturtexte
- Best Practices
- Policies
- Deployment-Modelle

## Was in `faq/` gehört
- wiederkehrende Klärungsfragen
- kurze, direkte Antworten
- Einordnungen zu Missverständnissen

## Was in `examples/` gehört
- konkrete Beispiele
- illustrative Abläufe
- praxisnahe Musterfälle

## Was in `templates/` gehört
- standardisierte Ausgangspunkte
- Strukturen, die mehrfach genutzt werden sollen

## Arbeitsregel für Agenten
Wenn ein Agent etwas ergänzen will, sollte er zuerst prüfen:
1. Ist es kompakte Steuerungsinformation? Dann `.project_knowlage/`
2. Ist es ausführliche Hauptdoku? Dann `docs/`
3. Ist es eine häufige Frage? Dann `faq/`
4. Ist es ein Musterfall? Dann `examples/`
5. Ist es eine Vorlage? Dann `templates/`

## Ziel
Wissen soll im Repository nicht nur vorhanden, sondern auffindbar, konsistent und für verschiedene Agentenrollen leicht nutzbar sein.