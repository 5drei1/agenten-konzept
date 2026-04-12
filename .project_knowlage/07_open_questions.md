# Open Questions

## Zweck dieser Datei
Diese Datei sammelt Themen, die im Repository noch nicht vollständig entschieden oder ausgearbeitet sind. Sie dient als Arbeitsliste für Planner-, Research- und Reviewer-Agenten.

## Bereits geklärte Punkte

### 1. Beispielcode und Referenzarchitekturen
Diese sind vorerst **nicht Teil des Scopes**. Das Repository dient aktuell der reinen Konzeptdokumentation und dem Verständnis von Agenten, Workflows und Projektwissen.

### 2. Technologischer Fokus
Der konzeptionelle Fokus ist bewusst auf **LangChain** und **LangGraph** festgelegt.

### 3. Datenschutz
Datenschutz ist grundsätzlich relevant, spielt in der aktuellen Ausarbeitungsphase aber **nicht die Hauptrolle**.

### 4. Business-Modell-Aspekt
**Entschieden: Out of scope.** Das Repository fokussiert auf Konzeptdokumentation für Agenten-Systeme, nicht auf Vermarktung oder wirtschaftliche Positionierung.

## Noch offene Themen

### 1. Welche Meta-Dokumente fehlen noch?
Noch auszuarbeiten sind insbesondere:
- Entscheidungsdokumente oder ADRs
- Review-Checklisten
- Navigationshilfen und Indizes

### 2. Wie stark sollen Governance und Policies später vertieft werden?
Aktuell ist das kein Schwerpunkt. Policies sind konzeptionell beschrieben (docs/07), aber Versionierung, Aktivierungslogik und Change-Management fehlen noch.

### 3. Wie tief soll die Management-Schicht beschrieben werden?
Das Konzept verweist auf eine eigene Management-Schicht für Projekte, Agentenregister, Policies und Profiles. Offen ist, wie detailliert diese Ebene dokumentiert werden soll und wer sie betreibt.

### 4. Wie funktioniert Modell-Routing und Entscheidungslogik konkret?
Wann wird welches Modell eingesetzt? Welche Policy greift bei welchem Datenzugang? Das ist bisher nur als Anforderung benannt, kein dokumentiertes Muster.

### 5. Wie skaliert das Konzept bei vielen Projekten oder Kunden?
docs/09 beschreibt drei Deployment-Modelle, aber keine konkreten Regeln für Skalierung: Was passiert bei 20, 50 oder 100 Projekten? Wo wird Wissen geteilt, ohne zu zentralisieren?

### 6. Wie wird das Konzept-Repository selbst gepflegt?
Kein Beitragsprozess, kein Changelog, keine Unterscheidung zwischen stabilem Kern und explorativem Inhalt. Wer entscheidet, welche Änderungen zulässig sind?

### 7. Wie sieht ein Eval-Set konkret aus?
docs/08 beschreibt Qualitätsachsen und empfiehlt Eval-Sets, aber es gibt kein Beispiel-Format, kein Template und keine konkreten Testfälle.

## Priorisierung

### Hohe Priorität
- Management-Schicht ausarbeiten (docs/12 anlegen)
- Modell-Routing und Entscheidungslogik dokumentieren
- Repo-Governance klären

### Mittlere Priorität
- Eval-Set-Format und Beispiel erstellen
- Skalierungsregeln ergänzen
- Policies versionieren und aktivieren

### Niedrige Priorität
- Datenschutz vertiefen
- spätere Implementierungsnähe entscheiden

## Arbeitsregel
Offene Fragen sollen sichtbar bleiben, bis sie bewusst entschieden, verschoben oder verworfen wurden.
