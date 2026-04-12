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
- `examples/` = konkrete Anwendungsbeispiele auf Konzeptebene
- `templates/` = wiederverwendbare Vorlagen

## Rolle dieses Repositories
Dieses Repository dient aktuell:
- der Konzeptdokumentation
- der strategischen Ausarbeitung
- dem Verständnis von Agenten, Workflows und Projektwissen
- der Schärfung eines möglichen Business-Modells

Es dient aktuell **nicht** als Ort für Beispielcode oder Referenzimplementierungen.

## Was in `.project_knowlage/` gehört
- verdichtete Projektsicht
- Kernmodell
- Agentenrollen
- Workflow-Muster
- Wissensstrategie
- Navigationshilfe
- Schreibregeln
- offene Fragen
- Roadmap

## Was nicht in `.project_knowlage/` gehört
- komplette Langtexte aus `docs/`
- beliebige Rohnotizen
- doppelte Kopien ganzer Dokumente
- unstrukturierte Zwischenstände
- Codebeispiele oder Implementierungsskizzen ohne klaren Konzeptnutzen

## Was in `docs/` gehört
- ausführliche Erklärungen
- Architektur- und Strukturtexte
- Best Practices
- Policies
- Deployment-Modelle
- strategische Einordnungen

## Was in `faq/` gehört
- wiederkehrende Klärungsfragen
- kurze, direkte Antworten
- Einordnungen zu Missverständnissen

## Was in `examples/` gehört
- illustrative Beispiele
- konzeptionelle Musterfälle
- praxisnahe Denkmodelle

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

## Zusätzliche Leitplanke
Das Repository ist bewusst auf LangChain und LangGraph ausgerichtet. Inhalte sollten diesen Fokus nicht beiläufig relativieren oder in eine allgemeine Framework-Sammlung verwandeln.

## Ziel
Wissen soll im Repository nicht nur vorhanden, sondern auffindbar, konsistent und für verschiedene Agentenrollen leicht nutzbar sein.