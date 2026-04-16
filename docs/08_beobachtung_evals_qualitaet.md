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

---

## Minimales Eval-Template

Jeder Workflow bekommt eine eigene Eval-Datei. Das Ziel ist nicht Perfektion, sondern Nachvollziehbarkeit: Was wurde erwartet, was kam raus, ist es gut genug?

### Struktur eines Eval-Eintrags

```
ID:               eval-001
Workflow:         bugfix
Beschreibung:     Planner identifiziert korrekte Datei anhand Stacktrace

Eingabe:
  ticket:         "NullPointerException in PaymentService.process() Zeile 42"
  stacktrace:     "at PaymentService.process(PaymentService.java:42)"
  projektwissen:  project-knowledge/payment_service.md

Erwartete Ausgabe:
  - Planner benennt PaymentService.java als primäre Datei
  - Planner schlägt mindestens einen konkreten Fix-Ansatz vor
  - Coder erzeugt Änderung nur in payment_service/

Bewertungskriterien:
  - Datei korrekt identifiziert?         → ja / nein
  - Fix-Ansatz vorhanden?                → ja / nein
  - Keine unerwünschten Seiteneffekte?   → ja / nein
  - Token-Kosten im erwarteten Rahmen?   → ja / nein  (Schwelle: < 4000 Token)

Ergebnis (Beispiel):
  bestanden: ja
  Anmerkung: Coder hat korrekt nur payment_service/ angefasst, PR-Text vollständig
```

### Wo Evals abgelegt werden

```
workflows/
  bugfix/
    evals/
      eval-001_planner_datei_identifikation.md
      eval-002_coder_keine_seiteneffekte.md
      eval-003_reviewer_ablehnung_unvollstaendiger_fix.md
```

### Hinweise zur Praxis

- **Ein Eval pro Grenzfall**, nicht pro Happy Path – Happy Paths testen sich selbst
- **Goldbeispiel festhalten:** Eine bekannt-gute Ausgabe als Referenz speichern; bei Modellwechsel darauf zurückvergleichen
- **Schwellen explizit machen:** „Gut genug" bedeutet konkret: Datei richtig, kein unerwarteter Scope, Token unter Schwellwert
- **Failure-Cases als eigene Einträge:** Wenn ein Workflow einmal falsch lag, wird daraus direkt ein Eval-Eintrag – kein einmaliges Vergessen

---

## Ziel
Nicht nur funktionierende Demos, sondern belastbare, überprüfbare Abläufe.
