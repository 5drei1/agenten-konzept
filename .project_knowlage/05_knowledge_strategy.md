# Knowledge Strategy

## Zweck dieser Datei
Diese Datei beschreibt, wo Wissen in diesem Repository liegen soll und wie Agenten damit umgehen sollen.

## Leitregel
**Local by default, central by exception. Graph before flat file.**

## Wissensform: Graph bevorzugt
Für projektspezifisches Wissen wird ein **Knowledge Graph** bevorzugt. Flat Files (Markdown) sind der Fallback für einfache oder sehr stabile Projekte.

**Warum Graph:**
- Beziehungen zwischen Entitäten sind explizit und traversierbar
- Subgraph-Abfragen liefern nur relevanten Kontext – niedrigere Token-Kosten
- Wissen wächst dynamisch mit dem Projekt
- Multi-Hop möglich: `Modul → nutzt → Lib → hat_bug → Ticket`

**Empfehlung: Graphiti** (self-hosted, MIT, Ollama-kompatibel, LangGraph-Integration)

**Fallback: Markdown-Dateien** wenn kein Graph vorhanden ist.

## Bedeutung für dieses Repository
Das Repository selbst ist das zentrale Konzept- und Framework-Repo. Trotzdem soll Wissen auch hier klar getrennt werden:

- Root-Level (`PITCH.md`, `README.md`) = Einstieg für menschliche Leser, Präsentationsartefakte
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
- konzeptionelle Muterfälle
- praxisnahe Denkmodelle

## Was in `templates/` gehört
- standardisierte Ausgangspunkte
- Strukturen, die mehrfach genutzt werden sollen

## Was auf Root-Ebene gehört
- `PITCH.md` – Executive Summary für Bewerbungsgespräche und Kunden-Pitches (Mermaid-Diagramme, kompakt)
- `README.md` – Einstieg ins Repository, Orientierung für menschliche Leser

Agenten schreiben **nicht** auf Root-Ebene, außer sie ergänzen explizit diese beiden Dateien.

## Arbeitsregel für Agenten
Wenn ein Agent Projektwissen laden will:
1. Gibt es einen lokalen Knowledge Graph (Graphiti)? → Subgraph-Query
2. Kein Graph vorhanden? → Markdown-Fallback aus `project-knowledge/`

Wenn ein Agent etwas ergänzen will:
1. Ist es ein Pitch oder Einstiegsdokument? Dann Root-Ebene (`PITCH.md`, `README.md`)
2. Ist es kompakte Steuerungsinformation? Dann `.project_knowlage/`
3. Ist es ausführliche Hauptdoku? Dann `docs/`
4. Ist es eine häufige Frage? Dann `faq/`
5. Ist es ein Musterfall? Dann `examples/`
6. Ist es eine Vorlage? Dann `templates/`

## Ausbaustufen als Orientierung
Das Repository beschreibt drei Ausbaustufen für Agenten-Systeme (Minimal, Mittel, Komplex). Details und Übergangskriterien in `docs/12_minimales_system.md`. Agenten sollten diese Stufen kennen, um Inhalte passend einzuordnen.

## Zusätzliche Leitplanke
Das Repository ist bewusst auf LangChain und LangGraph ausgerichtet. Graphiti ist als Knowledge-Graph-Lösung in diesen Stack integrierbar. Inhalte sollten diesen Fokus nicht beiläufig relativieren.

## Ziel
Wissen soll im Repository nicht nur vorhanden, sondern auffindbar, konsistent und für verschiedene Agentenrollen leicht nutzbar sein – bevorzugt über gezielten Graphzugriff, nicht über pauschales Dokument-Dumping.
