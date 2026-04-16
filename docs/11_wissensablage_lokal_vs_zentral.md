# Wissensablage: lokal vs. zentral

## Kurzfassung
Projektwissen sollte standardmäßig möglichst nah am Projekt liegen – bevorzugt als Knowledge Graph. Zentrale Ablagen sind sinnvoll für gemeinsame Standards, Policies und wiederverwendbare Bausteine.

## Empfohlenes Modell
Das Repository sollte mit einem hybriden Ansatz arbeiten:

- **projektlokal** für projektspezifisches Wissen – bevorzugt als Knowledge Graph
- **zentral** für übergreifende Standards und Framework-Bausteine

## Local by default, central by exception
Diese Regel ist für das Konzept besonders passend.

### Projektlokal
Projektspezifisches Wissen gehört idealerweise in oder nahe an das jeweilige Projekt oder Repository.

**Bevorzugte Form: Knowledge Graph (z.B. Graphiti)**
Ein lokaler Knowledge Graph ist der empfohlene Weg für projektspezifisches Wissen, sobald das Projekt mehr als eine Handvoll Entitäten hat. Der Graph liegt direkt beim Projekt, wird mit ihm versioniert oder als lokale Datenbank betrieben und vom `load_project_context`-Schritt im Workflow abgefragt.

Vorteile gegenüber Flat Files:
- Beziehungen zwischen Modulen, Regeln, Bugs und Personen sind explizit und traversierbar
- Gezielte Subgraph-Abfrage statt ganzer Dokumente – spart Token
- Wissen kann zur Laufzeit dynamisch ergänzt werden
- Multi-Hop-Abfragen möglich: `ServiceA → nutzt → LibB → hat_bug → #123`

**Fallback: Markdown-Dateien**
Für einfache oder sehr stabile Projekte reichen Markdown-Dateien unter `project-knowledge/`. Der Workflow behandelt beide Quellen gleich – er fragt zuerst nach einem Graph, fällt auf Markdown zurück wenn keiner vorhanden ist.

Beispiele für projektlokales Wissen:
- Architekturhinweise und Modulabhängigkeiten
- Coding-Regeln
- Repo-Metadaten
- bekannte Probleme und Bugs
- Kundenbesonderheiten
- Build- und Test-Kommandos

Typische Struktur im Projekt-Repo:
```text
project-knowledge/
  graph/                   <- Graphiti-Datenbank (bevorzugt)
  architecture.md          <- Fallback Flat File
  coding_rules.md          <- Fallback Flat File
  repo_config.yaml
  known_issues.md
```

### Zentral
Zentral gehören vor allem Inhalte hinein, die projektübergreifend gelten oder von vielen Projekten gemeinsam genutzt werden.

Beispiele:
- Agentenstandards
- Workflow-Templates
- globale Policies
- Datenschutz- und PII-Regeln
- Tool-Policies
- Best Practices
- Evaluations-Standards

## Warum der hybride Ansatz sinnvoll ist

### Vorteile der lokalen Ablage (Graph)
- näher am echten Projekt
- leichter aktuell zu halten
- zusammen mit dem Projekt versioniert
- Relationen zwischen Entitäten explizit modelliert
- gezieltere Kontext-Injektion, niedrigere Token-Kosten

### Vorteile der zentralen Ablage
- konsistente Standards
- bessere Wiederverwendung
- gemeinsame Governance
- weniger Streuung allgemeiner Regeln

## Tool-Empfehlung für Knowledge Graphs

| Tool | Lizenz | Self-hosted | Lokale Modelle | Besonderheit |
|---|---|---|---|---|
| **Graphiti** (getzep) | MIT | ✓ | ✓ (Ollama) | Temporal, LangChain-Integration |
| **Neo4j** | Community: free | ✓ | - | Etabliert, großes Ökosystem |
| **FalkorDB** | MIT | ✓ | - | Schnell, Graphiti-kompatibel |

Graphiti ist die **bevorzugte Wahl** weil es MIT-lizenziert, self-hostbar, mit Ollama nutzbar und direkt in LangChain/LangGraph integrierbar ist.

## Konsequenz für dieses Konzept
Zentrale Profilstrukturen sollten nicht als einziger Ort für sämtliches Projektwissen verstanden werden. Das eigentliche Detailwissen liegt projektlokal – bevorzugt als Knowledge Graph.

## Praktisches Zielbild

### Projekt oder Kundenrepo
- `project-knowledge/graph/` ← Knowledge Graph (Graphiti, bevorzugt)
- `project-knowledge/*.md` ← Fallback Flat Files
- `repo_config.yaml`
- projektspezifische Regeln
- domänenspezifische Dokumentation

### Zentrales Konzept-Repo
- Templates
- FAQ
- Best Practices
- übergreifende Policies
- Referenz-Workflows

## Empfehlung
Für neue Implementierungen: projektbezogenes Wissen bevorzugt als lokalen Knowledge Graph anlegen. Das zentrale Repository bleibt der Ort für das gemeinsame Rahmenwerk.
