# /run-evals

Führt die Evaluierungen für einen Workflow oder Agenten aus.

## Verwendung

```
/run-evals [workflow|agent] [name]
```

Beispiel: `/run-evals workflow bugfix`

## Was dieser Befehl tut

1. Sucht Fixtures in `evals/fixtures/[name]/`
2. Führt `uv run python evals/eval_harness.py --target [name]` aus
3. Gibt Ergebnis-Summary aus: Pass/Fail je Fixture, Token-Verbrauch, Latenz
4. Wenn Langfuse konfiguriert: Link zum Trace

## Eval-Kriterien (aus dem Konzept)

- **Korrektheit**: Stimmt das Ergebnis mit dem erwarteten Output überein?
- **Vollständigkeit**: Wurden alle Teilschritte ausgeführt?
- **Scope-Treue**: Hat der Agent seinen Scope eingehalten (keine verbotenen Aktionen)?
- **Token-Effizienz**: Liegt der Verbrauch unter dem definierten Budget?
- **Freigabe-Compliance**: Wurden Freigabestufen korrekt eingehalten?

## Regeln

- Evals laufen immer lokal, nie gegen Produktionsdaten
- Fixtures sind anonymisierte Beispieldaten
- Jede Änderung an Agenten oder Workflows erfordert einen Eval-Run
