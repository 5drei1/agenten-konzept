# Roadmap

## Zweck dieser Datei
Diese Datei beschreibt die sinnvolle Weiterentwicklung des Repositories in mehreren Ausbaustufen.

## Phase 1: Konzeptbasis
Ziel:
- Begriffe klären
- Kernmodell festhalten
- Struktur des Repositories aufbauen
- erste Beispiele und FAQ anlegen

Status:
- **abgeschlossen**

## Phase 2: Agentenfreundliche Steuerungsschicht
Ziel:
- `.project_knowlage/` ausbauen
- Rollen, Workflow-Muster und Wissensstrategie festhalten
- offene Fragen und Schreibregeln dokumentieren

Status:
- **weitgehend abgeschlossen**

## Phase 3: Entscheidungs- und Meta-Schicht
Ziel:
- Management-Schicht ausarbeiten
- Modell-Routing und Entscheidungslogik dokumentieren
- Repo-Governance klären
- ADRs oder Entscheidungsdokumente ergänzen

Status:
- **in Bearbeitung** (nächste Schritte, siehe unten)

## Phase 4: Strategische Ausarbeitung vertiefen
Ziel:
- Eval-Standards konkretisieren
- Skalierungsregeln dokumentieren
- Rolle von LangChain und LangGraph im Zielbild präziser ausarbeiten

## Phase 5: Spätere Vertiefungen
Ziel:
- Governance und Policies bei Bedarf vertiefen
- Datenschutz später sauber ergänzen
- optional spätere Implementierungsnähe entscheiden

## Was aktuell ausdrücklich nicht Priorität hat
- Beispielcode
- Referenzimplementierungen
- produktionsnahe Referenzarchitektur
- detaillierte Datenschutzarchitektur
- Business-Modell oder Vermarktungskonzept (out of scope)

## Nächste 5 konkrete Schritte (priorisiert)

### Schritt 1: Management-Layer ausarbeiten
**Ziel**: Den bislang nur erwähnten Baustein als eigenständiges Kapitel dokumentieren.
**Warum**: Ohne diesen Layer bleibt unklar, wer was kennt, wie Rollen zugewiesen werden und wie Projektprofile, Agenten-Registry und Policies zusammenhängen.
**Wo**: Neue Datei `docs/12_management_layer.md`, Kurzreferenz in `.project_knowlage/01_core_model.md` ergänzen.

### Schritt 2: Modell-Routing und Entscheidungslogik dokumentieren
**Ziel**: Klare wenn/dann-Regeln für Modellauswahl und Policy-Anwendung.
**Warum**: Ohne dieses Muster fehlt die operative Klammer zwischen Policy und Ausführung – der häufigste Bruchpunkt in der Praxis.
**Wo**: `docs/10_best_practices_und_entscheidungen.md` erweitern oder neue Datei `docs/13_routing_und_entscheidungslogik.md`.

### Schritt 3: Repo-Governance klären
**Ziel**: Regeln für die Weiterentwicklung dieses Repositories selbst.
**Warum**: Ohne Beitragsprozess entstehen Inkonsistenzen und Dopplungen, wenn mehrere Agenten oder Personen das Repo pflegen.
**Wo**: Neuer Abschnitt in `docs/00_vision_und_ziele.md` oder eigene kurze `CONTRIBUTING.md` im Root.

### Schritt 4: Eval-Set-Format und Beispiel erstellen
**Ziel**: Konkretes Format für Eval-Sets und ein Beispiel anhand des Bugfix-Workflows.
**Warum**: docs/08 beschreibt Qualitätsachsen, aber ohne Beispiel-Format ist das nicht umsetzbar.
**Wo**: `examples/coding_project/beispiel_eval_set.md` (neu), parallel Format-Abschnitt in `docs/08_beobachtung_evals_qualitaet.md` ergänzen.

### Schritt 5: Skalierungsregeln ergänzen
**Ziel**: Klare Regeln für den Betrieb bei vielen Projekten und Kunden.
**Warum**: docs/09 beschreibt Deployment-Modelle, aber keine Entscheidungslogik für Skalierung.
**Wo**: `docs/09_deployment_und_kundenauslagerung.md` erweitern.

## Zielbild
Das Repository soll sich zu einer belastbaren Konzept- und Arbeitsbasis für wiederverwendbare Agenten- und Workflow-Systeme entwickeln, nicht zu einem Code- oder Demo-Repository.
