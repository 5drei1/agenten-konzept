# Beobachtung, Evals und Qualität

## Warum Beobachtung notwendig ist
Ein produktiver Agenten- oder Workflow-Stack muss nachvollziehbar sein. Ohne Tracing, Evals und Qualitätskriterien ist weder Debugging noch sichere Verbesserung möglich.

## Was beobachtet werden sollte

- welcher Workflow lief
- welche Agenten beteiligt waren
- welche Tools aufgerufen wurden
- welche Datenklassen genutzt wurden
- Kosten, Dauer und Fehler
- Freigaben und Abbrüche

## Qualitätsachsen

- Korrektheit
- Reproduzierbarkeit
- Sicherheit
- Kosten
- Laufzeit

## Evals
Evals sollten nicht erst am Ende dazukommen. Schon frühe Workflows sollten mit einfachen Testfällen, Goldbeispielen und Fehlerfällen geprüft werden.

## Praktische Empfehlung
- Traces für alle relevanten Läufe
- kleine Eval-Sets pro Workflow
- Failure-Cases dokumentieren
- Prompts, Policies und Tools versionieren

## Ziel
Nicht nur funktionierende Demos, sondern belastbare, überprüfbare Abläufe.