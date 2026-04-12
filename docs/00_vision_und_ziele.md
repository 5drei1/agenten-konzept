# Vision und Ziele

## Das Problem

KI-gestützte Agenten-Systeme scheitern selten an der Technologie. Sie scheitern am Aufbau.

Ein Agent, der zu viel weiß, verliert den Fokus. Einer, der zu wenig weiß, trifft schlechte Entscheidungen. Wenn ein Agent alles erledigen soll, wird er schwer zu verstehen, schwer zu warten und schwer zu verbessern.

Das klassische Muster: Ein großer Prompt, ein mächtiger Agent, viel Kontext auf einmal – und am Ende weiß niemand mehr genau, warum er was getan hat.

## Die Leitidee

Nicht ein einzelner Super-Agent soll alles erledigen.

Stattdessen wird das System in klar getrennte Bausteine aufgeteilt:

- **Workflow** – beschreibt den Ablauf: was passiert in welcher Reihenfolge
- **Agent** – übernimmt eine klar definierte Rolle innerhalb des Workflows
- **Projektwissen** – liefert den projektspezifischen Kontext: gezielt, nicht pauschal
- **Tool** – führt eine technische Aufgabe aus
- **Policy** – legt Regeln und Grenzen fest
- **State** – trägt den aktuellen Stand durch den Ablauf

Der Workflow ist der Kern. Agenten sind Rollen darin. Wissen wird geladen, wenn es gebraucht wird.

## Die fünf Leitregeln

### 1. Workflow first
Bevor ein Agent definiert wird, wird der Ablauf beschrieben. Was muss passieren? In welcher Reihenfolge? Welche Entscheidungen gibt es? Agenten entstehen danach als Rollen innerhalb dieses Ablaufs – nicht umgekehrt.

### 2. Spezialisierte Agenten statt Universalagenten
Ein Agent mit einer klar beschriebenen Aufgabe ist besser zu verstehen, einfacher zu testen und leichter durch eine verbesserte Version zu ersetzen als ein Agent, der alles kann. Kleine, klare Rollen schlagen einen großen Alleskönner.

### 3. Projektwissen im Workflow
Projektspezifisches Wissen – Architekturregeln, Coding-Standards, Kundenkontexte – wird nicht pauschal in jeden Agenten kopiert. Der Workflow lädt es gezielt und gibt nur das weiter, was für den aktuellen Schritt relevant ist. Das spart Kontext und macht Entscheidungen nachvollziehbarer.

### 4. Lokal by default, zentral by exception
Wissen, das zu einem Projekt gehört, liegt möglichst nah am Projekt – im selben Repository, direkt pflegbar, versioniert mit dem Artefakt. Das zentrale Konzept-Repository hält übergreifende Standards, Templates und Referenzwissen – nicht alles.

### 5. Datenschutz als Architekturthema
Personenbezogene Daten dürfen nicht unkontrolliert an externe Modelle weitergegeben werden. Das ist kein nachträgliches Feature, sondern Teil des Workflow-Designs: Klassifikation, Erkennung, Anonymisierung und Routing sind feste Bestandteile des Ablaufs.

## Was dieses Repository ist

Ein Konzept- und Dokumentations-Repository – kein Code, keine Referenzimplementierung.

Es enthält:
- Definitionen der Kernbegriffe
- Architekturprinzipien und Entscheidungslogik
- Beispiele auf Konzeptebene
- Templates für Agenten, Workflows und Projektprofile
- eine agentenfreundliche Kurzsicht für den Einsatz von KI-Agenten im Repository selbst

Beispielcode und Referenzarchitekturen sind bewusst nicht Teil des aktuellen Scopes.

## Typische Einsatzfelder

- Bugfixing und Feature-Entwicklung in Coding-Projekten
- Technische Analyse und Planung
- Kundenprozesse wie Angebotserstellung oder Dokumentation
- Kommunikations- und Dokumentationsaufgaben mit Datenschutzanforderungen

## Technologischer Rahmen

Das Konzept ist auf **LangChain** und **LangGraph** ausgerichtet. LangChain stellt Modelle, Tools und einfache Agenten bereit. LangGraph übernimmt die Orchestrierung als Workflow-Engine. Dieser Stack ist bewusst gewählt – nicht als neutraler Vergleich, sondern als konkrete Arbeitsgrundlage.

## Ergebnisbild

Ein System, das mit demselben Grundmodell kleine Coding-Aufgaben und komplexe Kundenprozesse abbilden kann – ressourceneffizient, nachvollziehbar und langfristig wartbar.
