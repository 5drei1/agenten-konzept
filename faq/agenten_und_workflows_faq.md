# FAQ: Agenten und Workflows

## Ist ein Agent einfach eine Python-Datei?
Logisch nein, praktisch oft ja. Ein Agent ist eine Rolle mit Instruktionen, Tools und Grenzen. Für den Start ist eine Datei pro Agent sinnvoll.

## Können Agenten Wissen teilen?
Ja. Am saubersten geschieht das über den Workflow-State oder klar definierte Übergaben.

## Ruft ein Workflow nur Agenten auf?
Nein. Ein Workflow ruft auch Tools, Projektwissen, Policies und Freigaben auf.

## Ist LangGraph granularer als Claude Code?
Ja. LangGraph ist ein Baukasten für eigene Workflows und Agentensysteme. Claude Code ist stärker auf fertige Coding-Workflows ausgerichtet.

## Brauche ich ein separates Agenten-Management?
Meist ja, aber oft als schlanke eigene Schicht. Die reine Workflow-Orchestrierung ersetzt kein vollständiges Agentenregister.