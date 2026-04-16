# Projektwissen und Kontext

## Grundregel
Projektspezifisches Wissen gehört primär in den Workflow und möglichst nah an das jeweilige Projekt. Es gehört nicht fest in die Agenten.

## Warum?
Wenn Projektwissen in Agenten verdrahtet wird, verlieren Agenten ihre Wiederverwendbarkeit. Wenn es im Workflow geladen wird, bleibt der Agent allgemein und der Ablauf wird projektspezifisch.

## Bevorzugte Wissensform: Knowledge Graph

Für projektspezifisches Wissen wird bevorzugt ein **Knowledge Graph** eingesetzt statt flacher Markdown-Dateien.

**Warum ein Graph besser ist als Flat Files:**
- Entitäten und ihre Beziehungen sind explizit modelliert – kein implizites Wissen in Fließtext
- Der Workflow fragt nur den relevanten Subgraph ab – keine ganzen Dateien in den Prompt laden
- Beziehungen zwischen Modulen, Bugs, Regeln und Personen sind direkt traversierbar (Multi-Hop)
- Wissen kann zur Laufzeit dynamisch ergänzt werden, ohne Dateien manuell zu editieren
- Token-Kosten sinken: statt 2000 Token für ein ganzes Dokument nur ~150 Token für den relevanten Subgraph

**Empfohlenes Tool: Graphiti**
Graphiti (getzep/graphiti) ist self-hosted, MIT-lizenziert und unterstützt lokale Modelle via Ollama. Es passt damit direkt zur Architekturleitlinie "kein Vendor-Lock-in".

```mermaid
flowchart LR
    WF["Workflow\n(load_project_context)"]
    GR["Knowledge Graph\n(Graphiti, lokal)"]
    MD["Fallback:\nMarkdown-Dateien"]
    ST["Workflow State"]

    WF -->|"Subgraph-Query: 'PaymentService'"| GR
    GR -->|"relevante Nodes + Edges"| ST
    WF -->|"wenn kein Graph vorhanden"| MD
    MD --> ST
```

**Beispiel einer Graph-Abfrage im Workflow:**
```
Query: Subgraph um 'PaymentService'
Ergebnis:
  PaymentService → hängt_ab_von → StripeClient
  PaymentService → hat_bekannten_bug → NullPointer #42
  PaymentService → wird_genutzt_von → CheckoutFlow
  PaymentService → hat_regel → "nur in payment_service/ ändern"
```
Nur dieser kompakte Subgraph landet im Agenten-Prompt.

## Wann Flat Files als Fallback sinnvoll sind

Für einfache, stabile Projekte mit wenigen Entitäten reichen Markdown-Dateien vollständig aus. Der Graph lohnt sich ab:
- vielen verknüpften Komponenten (Microservices, Abhängigkeiten)
- dynamisch wachsendem Wissen (bekannte Bugs, Entscheidungen, Kunden)
- Projekten, bei denen der Kontext über mehrere Workflow-Runs hinweg wächst

## Arten von Wissen

### Stabiles Projektwissen
- Architektur und Modulbeziehungen
- Coding-Regeln
- relevante Verzeichnisse
- Test- und Build-Kommandos
- Sicherheits- und Compliance-Regeln

### Extrahiertes Repo-Wissen
- Dateibaum
- Modulstruktur und Abhängigkeiten
- relevante Dateien
- Testlandschaft
- wichtige Entry Points

### Laufzeitwissen
- Ticket
- Stacktrace
- Logs
- betroffene Dateien
- aktueller Diff

## Wo dieses Wissen liegen sollte

### Bevorzugt projektlokal
Direkt im Projekt oder Kundenrepo, als lokaler Graphiti-Graph oder unter `project-knowledge/`.

### Zentral nur ergänzend
Für gemeinsame Sichtweisen, Templates oder Referenzstrukturen kann eine zentrale Ablage zusätzlich sinnvoll sein.

## Repo-Einbindung
Das eigentliche Git-Repository bleibt die operative Quelle. Das Projektwissen liegt entweder im Projekt selbst (als Graph oder Markdown) oder in einer begleitenden projektnahen Struktur.

## Typischer Ablauf
1. Workflow erhält `project_id` oder `repo_path`
2. Workflow prüft: Knowledge Graph vorhanden?
   - Ja → Subgraph-Query für den relevanten Kontext
   - Nein → Markdown-Fallback aus `project-knowledge/`
3. Workflow ermittelt relevante Dateien und Regeln
4. State wird mit Projektkontext gefüllt
5. Agenten erhalten nur den nötigen Ausschnitt

## Merksatz
Projektwissen wird im Workflow injiziert. Agenten erhalten nur den relevanten Teilkontext – bevorzugt als kompakter Subgraph, nicht als ganzes Dokument.
