# Minimales System und Wachstumspfad

## Worum es geht

Die Gefahr bei einem durchdachten Konzept ist, dass man zu viel auf einmal umsetzt. Wer sieben Bausteine, fünf Leitregeln und einen Management-Layer vor sich sieht, beginnt zu planen, statt zu bauen.

Dieses Dokument zeigt, was wirklich nötig ist, was weggelassen werden darf und wann ein Upgrade Sinn ergibt. Es beschreibt drei Ausbaustufen: Minimal, Mittel und Komplex. Diese sind keine starren Phasen, sondern Orientierungspunkte. Ein System kann zwischen Stufen liegen oder für verschiedene Workflows auf unterschiedlichen Stufen stehen.


## Stufe 1 – Minimal: Ein Workflow, der funktioniert

### Was diese Stufe ist

Das kleinste System, das echten Nutzen bringt. Es gibt einen Ablauf, zwei bis drei Agenten und genug Projektwissen, damit die Ergebnisse brauchbar sind. Alles andere ist zu diesem Zeitpunkt Overhead.

### Agenten

- **Planner**: nimmt die Aufgabe entgegen, bewertet den Kontext, erstellt ein klares Vorgehen
- **Coder oder Ausführender**: setzt den Plan um – schreibt Code, erstellt Text oder führt eine klar definierte Aktion durch
- **Reviewer**: bewertet das Ergebnis anhand definierter Kriterien

Diese drei Rollen reichen für die meisten anfänglichen Workflows aus. In manchen Fällen können Planner und Reviewer zu Beginn nacheinander vom gleichen Agenten übernommen werden – solange das explizit so entschieden wurde.

### Workflows

Ein einziger Workflow für den wichtigsten Anwendungsfall. Typisch ist das ein Bugfix-Workflow, ein Analyse-Workflow oder ein Dokumentationslauf. Der Workflow lädt Projektwissen, ruft die Agenten in einer festen Reihenfolge auf und schreibt das Ergebnis in den State. Verzweigungen, Schleifen und bedingte Pfade sind in dieser Stufe bewusst nicht vorhanden.

### Projektwissen

Eine Datei oder ein Verzeichnis direkt im Projekt. Minimal enthalten:
- Beschreibung des Projekts (Zweck, Sprache, Rahmenbedingungen)
- die wichtigsten Coding- oder Inhaltsregeln
- relevante Pfade und Einstiegspunkte
- Build- und Testbefehle, sofern der Workflow sie braucht

Projektwissen wird vom Workflow geladen und in den State geschrieben. Der Agent erhält es als Teil seines Inputs – nicht als Teil seiner Konfiguration.

### Tools

- Dateizugriff (lesen und schreiben)
- Testausführung, wenn Tests zur Aufgabe gehören
- optional: Git-Zugriff für Diff oder Commit-Entwurf

Kein Tool, das nicht direkt für den Ablauf gebraucht wird. Jeder Agent bekommt nur die Tools, die er in seiner Rolle tatsächlich benötigt.

### Policies

Eine einfache Freigaberegel genügt: bevor etwas in das Repository geschrieben oder ein externer Dienst aufgerufen wird, wartet der Workflow auf eine manuelle Freigabe. Diese Regel kann zunächst implizit sein – also durch den Prozess selbst gewährleistet, nicht durch technische Enforcement.

### Tracing

Jeder Workflow-Lauf sollte protokolliert werden: welcher Workflow lief, welche Agenten aufgerufen wurden, was zurückkam. LangSmith ist dafür das naheliegende Werkzeug, aber auch ein einfaches Log reicht in Stufe 1.

### Was bewusst fehlt

- Subgraphs und wiederverwendbare Teilworkflows
- mehrere parallele Workflows für verschiedene Aufgaben
- automatisches Routing und bedingte Verzweigungen
- technisch erzwungene Policies
- Eval-Sets und systematische Qualitätsmessung
- Agentenregister oder zentrale Konfiguration


## Stufe 2 – Mittel: Mehrere Workflows, wiederverwendbare Teile

### Was diese Stufe ist

Das System hat bewiesen, dass es funktioniert. Es gibt mehrere Aufgabentypen, die regelmäßig anfallen. Muster wiederholen sich. Teile des Ablaufs tauchen in mehreren Workflows auf. Die Stufe-1-Lösung funktioniert noch, aber Ergänzungen kosten zunehmend Aufwand, weil nichts geteilt wird.

### Agenten

Die Kernrollen aus Stufe 1 bleiben bestehen. Hinzu kommen spezialisierte Agenten für neue Aufgabentypen, zum Beispiel ein Analyst für technische Einschätzungen oder ein Writer für Kundenkommunikation.

Die Agenten bleiben spezialisiert. Kein Agent übernimmt mehrere unverbundene Rollen. Wenn ein Agent zu viele Aufgaben bekommt, ist das ein Zeichen, dass er aufgeteilt werden sollte.

### Workflows

Zwei bis vier Workflows für die häufigsten Aufgabentypen. Typische Ergänzungen zu einem Bugfix-Workflow sind ein Feature-Workflow, ein Review-Workflow oder ein Dokumentations-Workflow.

Wiederkehrende Teile werden als Subgraphs ausgelagert. Kandidaten dafür sind:
- Projektwissen laden und in den State schreiben
- Ergebnis prüfen und Freigabe einholen
- Tests ausführen und Ergebnis auswerten
- PR-Text oder Zusammenfassung erzeugen

Ein Subgraph wird einmal definiert und von mehreren Workflows verwendet.

### Projektwissen

Das Projektwissen ist strukturierter und besteht aus mehreren Dateien mit klar getrennten Bereichen: Architektur, Coding-Regeln, bekannte Probleme, Test- und Build-Konfiguration. Projektprofile für wiederkehrende Projekte oder Kunden existieren als eigene Strukturen.

Der Workflow lädt nur den Teil des Wissens, der für den jeweiligen Ablauf relevant ist. Nicht alles wird immer in den State geladen.

### Tools

- Dateizugriff (lesen, schreiben, suchen)
- Git-Zugriff (Diff, Branch, Commit-Entwurf, PR-Vorbereitung)
- Testausführung und Auswertung
- optional: Zugriff auf Ticketsysteme wie Jira oder GitHub Issues

Jeder Tool-Zugriff ist an eine Rolle gebunden und dokumentiert.

### Policies

Policies sind explizit formuliert und im Projekt abgelegt. Sie regeln:
- welche Agenten auf welche Tools zugreifen dürfen
- bei welchen Aktionen eine Freigabe erforderlich ist
- wie mit Fehlern oder unklaren Situationen umgegangen wird

Technisches Enforcement ist wünschenswert, aber nicht zwingend. Eine dokumentierte Policy, die im Review beachtet wird, ist besser als gar keine Policy.

### Tracing und Evals

Tracing ist aktiv und wird ausgewertet. Erste Eval-Sets pro Workflow existieren: eine Handvoll Beispielinputs mit bekanntem guten Ergebnis. Fehler aus dem Betrieb fließen in die Eval-Sets ein.

### Was bewusst fehlt

- vollautomatischer Betrieb ohne Freigaben
- Skalierung über mehrere parallele Instanzen
- umfassende Governance-Strukturen
- tiefe PII-Erkennung und automatisches Redaction
- komplexe Multi-Agenten-Koordination mit dynamischem Routing


## Stufe 3 – Komplex: Skaliertes, gouverniertes System

### Was diese Stufe ist

Das System wird von mehreren Teams oder für mehrere Kunden betrieben. Abläufe müssen sicher, nachvollziehbar und wartbar sein, auch wenn die Person, die sie aufgebaut hat, nicht mehr direkt involviert ist. Qualität, Datenschutz und Compliance sind keine Empfehlungen mehr, sondern Anforderungen.

### Agenten

Das Agentenregister ist dokumentiert. Für jeden Agenten gibt es eine klare Rollenbeschreibung, die erlaubten Tools, die erwarteten Inputs und Outputs sowie die zugehörigen Policies. Agenten werden versioniert. Wenn ein Agent verändert wird, ist klar, welche Workflows davon betroffen sind.

Neue Agenten entstehen aus konkretem Bedarf, nicht aus Vorsorge.

### Workflows

Es gibt eine vollständige Bibliothek wiederverwendbarer Subgraphs. Workflows für neue Aufgabentypen entstehen durch Kombination bestehender Teile. Komplexere Workflows enthalten Verzweigungen, Schleifen und bedingte Freigabepunkte.

Der Einstieg in einen neuen Anwendungsfall ist ein Template, kein leeres Dokument.

### Projektwissen

Projektwissen ist nach dem hybriden Modell organisiert: projektspezifisches Wissen liegt im jeweiligen Projekt oder Kundenrepo, übergreifende Standards und Policies liegen zentral. Die Grenze ist klar definiert und wird eingehalten.

Für Kunden mit eigener Infrastruktur liegt das Projektwissen beim Kunden. Das Agentensystem holt es von dort ab, es wird nicht dupliziert.

### Tools

Tool-Zugriffe sind durch Policies technisch begrenzt, nicht nur dokumentarisch. Neue Tools durchlaufen einen definierten Freigabeprozess. Für externe Dienste oder APIs gibt es Adapter, die den Zugriff kapseln und kontrollierbar machen.

### Policies

Policies sind technisch enforced. Freigabestufen sind in den Workflows verankert: bestimmte Aktionen werden nicht ausgeführt, bis eine Freigabe vorliegt. PII-Erkennung und Redaction sind Teil des Workflow-Designs.

Für jede Policy gilt: sie ist schriftlich definiert, im Repository abgelegt und wird bei Änderungen aktiv versioniert.

### Tracing und Evals

Tracing ist lückenlos und wird systematisch ausgewertet. Eval-Sets existieren für alle Kern-Workflows und werden gepflegt. Qualitätsmetriken sind definiert: Korrektheit, Reproduzierbarkeit, Kosten, Laufzeit. Neue Workflows kommen erst in den Betrieb, wenn sie eine Mindestqualität nachgewiesen haben.

Failure-Cases werden analysiert, dokumentiert und in die Eval-Sets aufgenommen.

### Was bewusst fehlt oder variiert

Auch auf dieser Stufe gilt: kein Agent übernimmt unkontrolliert mehr Aufgaben als definiert. Komplexität ist kein Selbstzweck. Wenn ein Workflow auf Stufe 1 korrekt arbeitet und wartbar ist, gibt es keinen Grund, ihn auf Stufe 3 umzubauen.


## Übergangskriterien

### Von Stufe 1 nach Stufe 2

Der Übergang ist sinnvoll, wenn mindestens zwei der folgenden Situationen zutreffen:

- Es gibt mehr als einen Aufgabentyp, der regelmäßig anfällt, und beide brauchen einen Workflow.
- Teile des Ablaufs werden in mehreren Workflows wiederholt und verursachen spürbaren Pflege-Aufwand.
- Der Reviewer oder eine externe Person kann nicht nachvollziehen, welcher Agent was getan hat, weil Tracing fehlt oder unvollständig ist.
- Projektregeln oder Tool-Rechte werden mündlich kommuniziert statt schriftlich fixiert, und das führt zu Fehlern.
- Das System wird von mehr als einer Person gepflegt.

### Von Stufe 2 nach Stufe 3

Der Übergang ist sinnvoll, wenn mindestens zwei der folgenden Situationen zutreffen:

- Das System wird für mehrere Kunden oder Projekte betrieben, und der manuelle Koordinationsaufwand steigt deutlich.
- Compliance- oder Datenschutzanforderungen machen technisch erzwungene Policies notwendig.
- Qualitätsprobleme tauchen wiederholt auf, und es gibt keine systematische Möglichkeit, sie zu erkennen oder zu beheben.
- Agenten oder Workflows werden verändert, ohne dass klar ist, welche anderen Teile des Systems davon betroffen sind.
- Das System soll von Teams genutzt werden, die nicht an seinem Aufbau beteiligt waren.

### Was kein Übergangskriterium ist

Komplexität als solche ist kein Grund für ein Upgrade. Wenn Stufe 1 funktioniert und der Aufwand gering ist, ist Stufe 1 die richtige Wahl. Die Stufen beschreiben Zustände, die entstehen, wenn echte Anforderungen das Wachstum erzwingen – nicht Zustände, die man anstreben sollte, weil sie architektonisch befriedigender aussehen.


## Zusammenfassung

Ein System, das auf Stufe 1 sauber aufgebaut ist, lässt sich schrittweise erweitern, ohne grundlegend umgebaut zu werden. Der Schlüssel ist, dass die Grundprinzipien von Anfang an stimmen: Workflow first, spezialisierte Agenten, Projektwissen im Workflow – nicht in den Agenten.

Ein System, das auf Stufe 1 bereits gegen diese Prinzipien verstößt – weil der erste Agent zu viel weiß, weil Projektwissen hart verdrahtet ist oder weil kein Workflow, sondern nur ein Prompt existiert – wird auf Stufe 2 und 3 deutlich schwerer zu kontrollieren sein.
