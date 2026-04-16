# Repository Map

## Zweck

Diese Datei beschreibt die Struktur des Repositories und erklärt den Zweck jedes Verzeichnisses und jeder wichtigen Datei.

## Verzeichnisstruktur

```
agenten-konzept/
├── .claude/                    # Claude-spezifische Konfiguration
│   └── commands/               # Slash-Commands für Claude
│       ├── new-agent.md        # Befehl: Neuen Agenten erstellen
│       └── new-workflow.md     # Befehl: Neuen Workflow erstellen
├── .project_knowledge/         # Strukturiertes Projektwissen
│   ├── 00_overview.md          # Überblick über diesen Ordner
│   ├── 01_core_model.md        # Kernmodell des Projekts
│   ├── 02_repository_map.md    # Diese Datei
│   ├── 03_agent_roles.md       # Agenten-Rollen
│   ├── 04_workflow_patterns.md # Workflow-Muster
│   ├── 05_knowledge_strategy.md # Wissensstrategie
│   ├── 06_writing_rules.md     # Schreibregeln
│   ├── 07_open_questions.md    # Offene Fragen
│   └── 08_roadmap.md           # Roadmap
├── agents/                     # Agenten-Definitionen (geplant)
├── workflows/                  # Workflow-Definitionen (geplant)
├── examples/                   # Beispiel-Implementierungen (geplant)
├── CLAUDE.md                   # Haupt-Kontextdatei für Claude
└── README.md                   # Projektübersicht
```

## Wichtige Dateien

### CLAUDE.md

Die primäre Kontextdatei für Claude-Agenten. Wird automatisch von Claude Code geladen. Enthält:
- Projekt-Übersicht und Kontext
- Verweise auf `.project_knowledge/`
- Wichtige Konventionen und Regeln

### README.md

Öffentliche Projektbeschreibung für GitHub. Erklärt:
- Was das Projekt ist
- Wie man es nutzt
- Wie man beiträgt

### .claude/commands/

Slash-Commands für Claude Code. Diese Dateien definieren wiederverwendbare Workflows:
- Werden mit `/` aufgerufen (z.B. `/new-agent`)
- Enthalten strukturierte Anweisungen für Claude
- Ermöglichen konsistente Ausführung von Standardaufgaben

## Namenskonventionen

### Dateien

- `snake_case` für alle Dateinamen
- Nummerierung (`00_`, `01_`, etc.) für Lesereihenfolge
- `.md` für Markdown-Dokumente
- `.json` für strukturierte Daten
- `.py` für Python-Code

### Verzeichnisse

- `snake_case` für normale Verzeichnisse
- `.` Prefix für versteckte/Konfigurations-Verzeichnisse
- Keine Leerzeichen in Verzeichnisnamen
