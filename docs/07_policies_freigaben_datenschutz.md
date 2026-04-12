# Policies, Freigaben und Datenschutz

## Warum dieser Bereich wichtig ist
Sobald Agenten schreiben, senden oder produktive Systeme verändern, reichen gute Prompts nicht aus. Es braucht explizite Regeln.

## Freigabestufen

### Stufe 0
Nur lesen und analysieren.

### Stufe 1
Entwürfe und Vorschläge erzeugen.

### Stufe 2
Änderungen in Branches, Drafts oder vorbereiteten Artefakten.

### Stufe 3
Ausführung erst nach menschlicher Freigabe.

### Stufe 4
Vollautomatik nur für klar begrenzte Low-Risk-Fälle.

## Datenschutz-Grundregel
PII und andere sensible Daten dürfen nicht blind an externe Modellanbieter weitergegeben werden.

## Empfohlenes Muster
1. Intake lokal oder in Kundeninfrastruktur
2. Klassifikation und PII-Erkennung
3. Redaction oder Tokenisierung
4. Entscheidung über Modellrouting
5. Externe Modelle nur mit bereinigten Daten

## Wichtige Folge
Datenschutz wird nicht allein durch LangGraph gelöst. Die Sicherheit entsteht durch Workflow-Design, Policies und Datentrennung.

## Logging und Tracing
Auch Observability darf keine Rohdaten mit PII ungeprüft speichern.

## Beispiele für Freigabepunkte
- E-Mail-Versand
- produktive Codeänderungen
- Löschoperationen
- Rechnungsfreigaben
- Angebotsversand an Kunden