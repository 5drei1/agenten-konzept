# Workflow Patterns

## Zweck dieser Datei

Dokumentiert wiederkehrende Muster in Agenten-Workflows, die als Bausteine für komplexere Abläufe dienen.

## Pattern: Sequential Chain

```
Agent A → Output A → Agent B → Output B → Agent C
```

**Wann nutzen**: Aufgaben mit klaren Abhängigkeiten, wo jeder Schritt auf dem vorherigen aufbaut.

**Beispiel**: Research → Analyse → Bericht schreiben

## Pattern: Parallel Fan-Out

```
         ┌→ Agent A
Orchestrator → Agent B
         └→ Agent C
              ↓
         Aggregation
```

**Wann nutzen**: Unabhängige Teilaufgaben, die parallel bearbeitet werden können.

**Beispiel**: Mehrere Quellen gleichzeitig recherchieren

## Pattern: Review Loop

```
Agent A → Draft → Agent B (Review) → Feedback → Agent A → Final
```

**Wann nutzen**: Qualitätssicherung, iterative Verbesserung.

**Beispiel**: Code schreiben → Code reviewen → Überarbeiten

## Pattern: Conditional Routing

```
Input → Router Agent → [Condition]
                        ├─ If A → Agent A
                        └─ If B → Agent B
```

**Wann nutzen**: Verschiedene Eingaben erfordern verschiedene Bearbeitungswege.

**Beispiel**: Anfrage-Klassifikation vor Bearbeitung
