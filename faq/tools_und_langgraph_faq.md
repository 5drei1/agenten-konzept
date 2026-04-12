# FAQ: Tools, LangChain, LangGraph und LangSmith

## Was ist der Unterschied?
- LangChain: Bibliothek für Modelle, Tools und allgemeine Agentenbausteine
- LangGraph: Workflow- und Orchestrierungsebene
- LangSmith: Beobachtung, Debugging und Evaluation

## Was kann ich kostenlos nutzen?
LangChain und LangGraph sind offen nutzbar. Beobachtungs- und Management-Plattformen sind oft nur teilweise kostenlos oder in Self-hosting eingeschränkt.

## Gibt es ein fertiges Agent-Building-Tool?
Nicht als perfektes All-in-one-Werkzeug. Der offizielle Weg ist eher code-first mit zusätzlicher UI für Beobachtung und Iteration.

## Sollte ich alles selbst bauen?
Nein. Die Agenten- und Workflow-Infrastruktur sollte auf bestehende Bausteine setzen. Selbst gebaut werden sollte vor allem die Fachschicht mit Projekten, projektnahem Wissen, Policies und konkreten Workflows.