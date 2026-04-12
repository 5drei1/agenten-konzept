# Overview

## Zweck dieses Ordners
`project-knowledge/` ist die kompakte Arbeitsgrundlage für Agenten, die am Repository `agenten-konzept` arbeiten. Der Ordner soll schneller konsumierbar sein als die ausführliche Doku unter `docs/`.

## Wofür Agenten diesen Ordner nutzen sollen
Agenten sollen hier schnell verstehen:

- was das Ziel des Repositories ist
- welches Kernmodell gilt
- welche Regeln verbindlich sind
- wo vertiefende Informationen liegen
- wie neue Inhalte konsistent ergänzt werden sollen

## Wofür dieser Ordner nicht gedacht ist
Dieser Ordner ist nicht die komplette Hauptdokumentation und nicht die Ablage für beliebige Notizen. Er soll nur das arbeitsrelevante Projektwissen bündeln, das für Planung, Schreiben, Review und Strukturarbeit notwendig ist.

## Ziel des Repositories
Das Repository dokumentiert ein wiederverwendbares Konzept für Agenten, Workflows, Projektwissen, Tools und Policies. Es soll als Referenz- und Arbeitsbasis dienen, um KI- und Agentensysteme ressourceneffizient, nachvollziehbar und kundenfähig aufzubauen.

## Leitregeln

### 1. Workflow first
Abläufe werden zuerst als Workflows gedacht. Agenten sind Rollen innerhalb dieser Abläufe.

### 2. Spezialisierte Agenten
Kleine, klar definierte Agenten sind besser als ein unklarer Universalagent.

### 3. Projektwissen im Workflow
Projektspezifisches Wissen wird im Workflow geladen und über State oder Kontext weitergegeben. Es wird nicht fest in Agenten eingebaut.

### 4. Local by default, central by exception
Projektspezifisches Wissen liegt bevorzugt direkt im Projekt oder Kundenrepo. Das zentrale Konzept-Repo hält vor allem Standards, Templates und Referenzwissen.

### 5. Datenschutz als Architekturthema
PII-Schutz und Freigaben sind Teil des Workflow-Designs, nicht bloß ein nachträglicher Zusatz.

## Empfohlene Lesereihenfolge für Agenten

### Planner-Agent
1. `00_overview.md`
2. `01_core_model.md`
3. `02_repository_map.md`
4. passende Datei unter `docs/`

### Writer-Agent
1. `00_overview.md`
2. `01_core_model.md`
3. `02_repository_map.md`
4. Ziel-Datei oder passender Ordner in `docs/`, `examples/`, `faq/` oder `templates/`

### Reviewer-Agent
1. `00_overview.md`
2. `01_core_model.md`
3. `02_repository_map.md`
4. zu prüfende Datei

## Arbeitsziel
Agenten sollen mit diesem Ordner schneller konsistent arbeiten können, ohne jedes Mal die gesamte Dokumentation erneut erschließen zu müssen.