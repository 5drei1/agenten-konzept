# Agent Roles

## Zweck dieser Datei
Diese Datei beschreibt, welche Agentenrollen für die Ausarbeitung des Repositories sinnvoll sind und wie sie zusammenarbeiten.

## Grundregel
Die Rollen sollen klein, klar und überprüfbar sein. Jede Rolle hat ein enges Aufgabenprofil.

## Empfohlene Rollen

### Planner-Agent
Zuständig für:
- neue Themen strukturieren
- Inhalte in passende Dateien einsortieren
- Redundanzen erkennen
- Arbeitsreihenfolgen vorschlagen

Typische Inputs:
- neue Idee oder Frage
- bestehende Repository-Struktur
- relevante Dateien

Typische Outputs:
- Strukturvorschlag
- Dateivorschläge
- To-do-Liste
- Scope-Abgrenzung

### Writer-Agent
Zuständig für:
- neue Dokumentation schreiben
- bestehende Texte sauber erweitern
- Beispiele ausformulieren
- klare, konsistente Beschreibungen erzeugen

Typische Inputs:
- Strukturvorgabe
- Kernmodell
- Ziel-Datei

Typische Outputs:
- neue Markdown-Inhalte
- umgeschriebene oder ergänzte Dokumentation

### Reviewer-Agent
Zuständig für:
- Widersprüche finden
- Begriffe prüfen
- KISS-Verstöße erkennen
- unnötige Dopplungen markieren

Typische Outputs:
- Review-Hinweise
- Korrekturvorschläge
- Konsistenzprüfung

### Research- oder Analyst-Agent
Zuständig für:
- neue Fragestellungen aufbereiten
- Varianten vergleichen
- fehlende Bereiche identifizieren
- offene Fragen präzisieren

### Maintainer-Agent
Zuständig für:
- Dateistruktur pflegen
- Indizes und Referenzen aktuell halten
- veraltete oder doppelte Inhalte erkennen

## Zusammenarbeit der Rollen
Ein sinnvoller Ablauf ist oft:
1. Planner strukturiert
2. Writer formuliert
3. Reviewer prüft
4. Maintainer konsolidiert

## Grenzen
- Kein Agent sollte stillschweigend Grundbegriffe umdefinieren.
- Kein Agent sollte neue Dateien anlegen, wenn vorhandene Dateien sinnvoll erweitert werden können.
- Kein Agent sollte Beispiele als allgemeingültige Regeln darstellen.

## Merksatz
Rollen trennen Denken, Schreiben, Prüfen und Strukturpflege.