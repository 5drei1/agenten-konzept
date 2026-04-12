# FAQ: Tools, LangChain, LangGraph und Beobachtung

## Was ist der Unterschied zwischen LangChain, LangGraph und der Beobachtungsschicht?
- **LangChain**: Bibliothek für Modelle, Tools und allgemeine Agentenbausteine. Tools werden hier definiert und an Agenten gebunden.
- **LangGraph**: Workflow- und Orchestrierungsebene. StateGraph, Nodes, Edges, Subgraphs, ToolNode.
- **Beobachtungsschicht**: Tracing, Debugging und Evaluation. Diese Schicht ist **austauschbar** und kein fester Bestandteil des Kernstacks.

## Welche Beobachtungslösung wird empfohlen?
**Langfuse** – self-hostable, MIT-lizenziert, deckt Tracing, Evals und Prompt-Management ab. Die Integration in LangChain/LangGraph erfolgt über einen Callback-Handler.

Weitere Open-Source-Optionen:
- **Phoenix (Arize)** – starkes Tracing, LangChain-Integration
- **OpenLLMetry** – OpenTelemetry-basiert, vendor-neutral, funktioniert mit jedem OTel-Backend (Jaeger, Grafana Tempo)

Das Konzept setzt auf self-hostbare Lösungen, um keine Abhängigkeit von kommerziellen Plattformen einzugehen.

## Kann ich auch LangSmith verwenden?
Ja. LangSmith ist die native Lösung von LangChain Labs und hat die tiefste Integration. Sie ist aber teilweise kostenpflichtig und nicht vollständig self-hostbar. Für Open-Source-Betrieb ist Langfuse die empfohlene Alternative.

## Was kann ich kostenlos und self-hosted nutzen?
- LangChain und LangGraph sind vollständig open source und self-hostbar.
- Langfuse ist MIT-lizenziert und kann per Docker Compose selbst betrieben werden.
- OpenLLMetry ist OpenTelemetry-basiert und vendor-neutral.

## Gibt es ein fertiges Agent-Building-Tool?
Nicht als perfektes All-in-one-Werkzeug. Der offizielle Weg ist code-first mit LangGraph für die Workflow-Orchestrierung und Langfuse oder Phoenix für Beobachtung und Iteration.

## Sollte ich alles selbst bauen?
Nein. Die Agenten- und Workflow-Infrastruktur sollte auf bestehende Bausteine setzen. Selbst gebaut werden sollte vor allem die Fachschicht mit Projekten, projektnahem Wissen, Policies und konkreten Workflows.

## Wann ist ein Tool das Richtige, wann ein Subgraph?
Ein **Tool** ist atomar – eine Aktion, ein Ergebnis, das LLM entscheidet wann es aufgerufen wird.
Ein **Subgraph** hat mehrere Schritte, eigenen State und eigenes Routing – und wird als fertiger Graph in einen übergeordneten Workflow eingebunden.
