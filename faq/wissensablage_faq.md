# FAQ: Wissensablage lokal und zentral

## Sollte Projektwissen zentral in `project_profiles/` liegen?
Nicht ausschließlich. `project_profiles/` ist nützlich als zentrale Sicht, aber detailreiches Projektwissen sollte oft direkt am Projekt liegen.

## Ist eine Ablage wie `project-knowledge/` im Projekt-Repo sinnvoll?
Ja. Das ist für viele Coding- und Kundenprojekte sogar sehr sinnvoll, weil das Wissen näher am tatsächlichen Artefakt bleibt.

## Was bleibt trotzdem zentral?
Vor allem Standards, Templates, globale Policies, Best Practices und wiederverwendbare Workflow-Bausteine.

## Was ist die empfohlene Grundregel?
Local by default, central by exception.

## Wie greift ein Workflow auf projektlokales Wissen zu?
Über Repo-Pfade, Konfigurationen und dedizierte Kontext-Lader. Der Workflow entscheidet, was geladen wird, und gibt nur den relevanten Ausschnitt an Agenten weiter.