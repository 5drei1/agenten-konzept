# Core Model

## Kurzfassung
Das Konzept dieses Repositories basiert auf wenigen klar getrennten Bausteinen:

- Projekte
- Agenten
- Workflows
- Tools
- Subgraphs
- Projektwissen
- Policies und Freigaben
- State
- Beobachtung und Evals

Diese Trennung ist die fachliche Wahrheit des Repositories.

## Aktuelle Einordnung des Repositories
Dieses Repository ist aktuell ein **Konzept- und Dokumentations-Repository**.

Es dient dazu:
- das Modell verständlich zu machen
- Begriffe und Zusammenhänge zu schärfen
- eine strukturierte Grundlage für spätere Entscheidungen zu schaffen

Es ist aktuell **kein Referenzcode- oder Architektur-Repo**.

## Die Bausteine

### Projekt
Ein Projekt ist der fachliche und technische Rahmen eines Vorhabens. Es kann ein Coding-Projekt, ein Kundenprozess oder eine dauerhafte Aufgabe sein.

### Agent
Ein Agent ist eine Rolle mit klaren Instruktionen, erlaubten Tools, Grenzen und einem erwarteten Ausgabeformat. In LangGraph ist ein Agent typischerweise eine Node-Funktion, die ein LLM mit gebundenen Tools aufruft (`llm.bind_tools(tools)`).

### Workflow
Ein Workflow ist ein geordneter Ablauf. Er verbindet Agenten, Tools, Projektwissen, Regeln und Freigaben. In LangGraph wird ein Workflow als `StateGraph` definiert mit Nodes, Edges und einem kompilierten Graphen.

### Tool
Ein Tool ist eine technische Fähigkeit, zum Beispiel Dateizugriff, Git-Zugriff, Tests, API-Aufrufe oder E-Mail-Entwürfe. In LangChain werden Tools per `@tool`-Decorator oder `StructuredTool` definiert. Der `ToolNode` aus `langgraph.prebuilt` führt Tool-Calls eines LLMs automatisch aus. Jeder Agent bekommt nur die Tools, die er wirklich braucht.

### Subgraph
Ein Subgraph ist ein wiederverwendbarer, vollständig kompilierter Teilworkflow. Er hat dieselbe Struktur wie ein vollständiger Workflow (`graph.py`, `state.py`, `nodes.py`) und wird als Node in einen übergeordneten Graphen eingebunden. Subgraphs eignen sich für wiederkehrende Teilaufgaben wie Projektwissen laden, Freigaben einholen oder Tests ausführen.

**Entscheidungsregel Tool vs. Subgraph**: Ein Tool ist atomar – eine Aktion, ein Ergebnis, das LLM entscheidet wann. Ein Subgraph hat mehrere Schritte, eigenen State und eigenes Routing.

### Projektwissen
Projektwissen ist langlebiger, projektspezifischer Kontext. Es umfasst zum Beispiel Architektur, Regeln, Repo-Metadaten, Domänenwissen oder Kundenbesonderheiten. Es liegt bevorzugt direkt im Projekt oder Kundenrepo.

### Policy
Policies definieren Grenzen, Rechte, Datenschutzregeln, Freigaben und verbotene Aktionen.

### State
State ist der laufende Kontext eines Workflow-Runs. In LangGraph wird der State als `TypedDict` definiert – er enthält Eingaben, Zwischenergebnisse, Entscheidungen und Übergaben zwischen Nodes. Jeder Workflow hat seinen eigenen State-Typ. Subgraphs haben ihren eigenen State, der am Übergabepunkt mit dem übergeordneten State synchronisiert wird.

## Zentrale Architekturregeln

### Agent = Fähigkeit
Ein Agent ist keine komplette Anwendung, sondern eine klar abgegrenzte Rolle.

### Workflow = Ablaufsteuerung
Der Workflow entscheidet, welche Schritte ausgeführt werden, welches Wissen geladen wird und welche Agenten oder Tools beteiligt sind.

### Projektwissen = Kontextschicht
Projektwissen wird nicht blind überall repliziert. Es wird gezielt geladen und dem jeweiligen Workflow-Lauf passend injiziert.

### Subgraph = Wiederverwendung
Wiederkehrende Teilaufgaben werden als Subgraphs ausgelagert, nicht in jeden Workflow kopiert.

## Ausbaustufen
Das Konzept beschreibt drei Ausbaustufen – von einem minimalen System (1 Workflow, 3 Agenten) bis zu einem skalierten, gouvernierten System. Details in `docs/12_minimales_system.md`.

## Wissensregel
Für dieses Repository gilt:

**Local by default, central by exception.**

Das heißt:
- projektspezifisches Wissen liegt bevorzugt direkt im Projekt oder Kundenrepo
- das zentrale Konzept-Repo enthält vor allem Standards, Templates, Best Practices und Referenzmodelle

## Technologischer Fokus
Für dieses Konzept ist der Fokus aktuell bewusst auf:
- **LangChain** – Modelle, Tools, einfache Agenten
- **LangGraph** – Workflow-Orchestrierung als StateGraph

gelegt. Das Repository wertet keine alternativen Frameworks aus, sondern arbeitet ein belastbares Modell auf Basis dieses Stacks aus.

Die **Beobachtungsschicht** ist eine austauschbare Komponente. Empfohlen wird **Langfuse** (self-hostbar, MIT-Lizenz). LangSmith ist ebenfalls nutzbar, aber teilweise kostenpflichtig. Details in `docs/02_architekturueberblick.md` und `faq/tools_und_langgraph_faq.md`.

## Was Agenten bei Änderungen beachten müssen
- Begriffe konsistent halten
- Workflow-first nicht unterlaufen
- Projektwissen nicht als festen Agentenbesitz darstellen
- zentrale und lokale Wissensschichten sauber unterscheiden
- Tool vs. Subgraph klar unterscheiden
- das Repository als Konzept- und Strategierepo behandeln, nicht als Code-Repo

## Merksätze
- Agenten liefern Fähigkeiten.
- Workflows steuern Abläufe.
- Tools führen Aktionen aus.
- Subgraphs kapseln Teilabläufe.
- State trägt den Kontext durch den Graphen.
- Projektwissen liefert Kontext.
- Policies begrenzen Macht und Risiko.
