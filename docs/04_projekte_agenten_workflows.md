# Projekte, Agenten und Workflows

## Das Basismodell
Das Konzept arbeitet mit drei zentralen Ebenen:

- Projekte
- Agenten
- Workflows

Diese drei Ebenen reichen jedoch nur dann aus, wenn Projektwissen, Tools und Policies sauber ergänzt werden.

## Projekte
Ein Projekt ist der Rahmen eines Vorhabens. Es kann ein Codeprojekt, ein Kundenprozess oder eine wiederkehrende Aufgabe sein. Ein Projekt bündelt Ziele, Regeln, Kontext und Zuständigkeiten.

## Agenten
Agenten sind spezialisierte Rollen. Beispiele:

- Planner
- Coder
- Reviewer
- Analyst
- Writer

Agenten sollten nicht als unkontrollierte Alleskönner entworfen werden. Besser sind kleine, gut definierte Rollen mit klaren Grenzen.

## Workflows
Workflows verbinden die Schritte eines Ablaufs. Sie laden Wissen, rufen Agenten und Tools auf und prüfen, ob Freigaben notwendig sind.

## Die wichtige Korrektur
Workflows rufen nicht nur Agenten auf. Sie rufen auch:

- Tools
- Projektwissen
- Policies
- Freigaben
- Validierungen

## Merksatz

- Agent = Fähigkeit
- Workflow = Ablaufsteuerung
- Projekt = fachlicher und technischer Kontext

## Beispiel Bugfix
Ein Bugfix-Workflow kann so aussehen:

1. Projektkontext laden
2. Ticket und Stacktrace laden
3. Planner erstellt Vorgehen
4. Coder erzeugt Änderung
5. Tests werden ausgeführt
6. Reviewer prüft Ergebnis
7. PR-Text wird erstellt

Dieses Muster kann für viele Projekte wiederverwendet werden, solange das Projektwissen austauschbar bleibt.