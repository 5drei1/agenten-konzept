# Wissensablage: lokal vs. zentral

## Kurzfassung
Projektwissen sollte standardmäßig möglichst nah am Projekt liegen. Zentrale Ablagen sind sinnvoll für gemeinsame Standards, Policies und wiederverwendbare Bausteine.

## Empfohlenes Modell
Das Repository sollte mit einem hybriden Ansatz arbeiten:

- projektlokal für projektspezifisches Wissen
- zentral für übergreifende Standards und Framework-Bausteine

## Local by default, central by exception
Diese Regel ist für das Konzept besonders passend.

### Projektlokal
Projektspezifisches Wissen gehört idealerweise in oder nahe an das jeweilige Projekt oder Repository.

Beispiele:
- Architekturhinweise
- Coding-Regeln
- Repo-Metadaten
- bekannte Probleme
- Kundenbesonderheiten
- Build- und Test-Kommandos

Eine mögliche Struktur innerhalb eines Projekt-Repositories ist:

```text
project-knowledge/
  architecture.md
  coding_rules.md
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

### Vorteile der lokalen Ablage
- näher am echten Projekt
- leichter aktuell zu halten
- zusammen mit dem Projekt versioniert
- weniger doppelte Pflege

### Vorteile der zentralen Ablage
- konsistente Standards
- bessere Wiederverwendung
- gemeinsame Governance
- weniger Streuung allgemeiner Regeln

## Konsequenz für dieses Konzept
`project_profiles/` sollte nicht als einziger Ort für sämtliches Projektwissen verstanden werden. Es kann als zentrale Projektsicht dienen, aber das eigentliche Detailwissen darf und sollte häufig projektlokal liegen.

## Praktisches Zielbild

### Projekt oder Kundenrepo
- `project-knowledge/`
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
Für neue Implementierungen sollte projektbezogenes Wissen bevorzugt im Projekt oder Kundenrepo liegen. Das zentrale Repository bleibt der Ort für das gemeinsame Rahmenwerk.