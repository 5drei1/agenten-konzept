# Tools, Integrationen und Skills

## Tools sind nicht Agenten
Agenten entscheiden und formulieren. Tools führen technische Aktionen aus. Ein Agent ruft ein Tool auf – das Tool führt die Aktion aus und gibt ein Ergebnis zurück.

## Typische Tool-Klassen

- Dateizugriff (lesen, schreiben, suchen)
- Git und Repository-Zugriff (Diff, Branch, Commit)
- Testausführung und Ergebnisauswertung
- E-Mail-Entwürfe und Kommunikation
- Datenbank- und API-Zugriffe
- Suche, Crawling und Extraktion

## Tools in LangChain definieren

LangChain bietet drei Wege, ein Tool zu definieren:

**`@tool`-Decorator** – der einfachste Weg für eigenständige Funktionen:
```python
from langchain_core.tools import tool

@tool
def read_file(path: str) -> str:
    """Liest eine Datei und gibt ihren Inhalt zurück."""
    with open(path) as f:
        return f.read()
```

**`StructuredTool`** – wenn das Tool mehrere strukturierte Eingaben braucht:
```python
from langchain_core.tools import StructuredTool
from pydantic import BaseModel

class GitDiffInput(BaseModel):
    repo_path: str
    branch: str = "main"

def get_git_diff(repo_path: str, branch: str) -> str:
    ...

git_diff_tool = StructuredTool.from_function(
    func=get_git_diff,
    args_schema=GitDiffInput,
)
```

**`BaseTool`** – für komplexere Tools mit eigener Klasse und Fehlerbehandlung.

Die Docstring-Beschreibung ist wichtig: Das LLM nutzt sie, um zu entscheiden, ob und wann es das Tool aufruft.

## Tools an einen Agenten binden

In LangGraph wird ein LLM mit seinen erlaubten Tools per `bind_tools()` verknüpft:

```python
from langchain_anthropic import ChatAnthropic

tools = [read_file, git_diff_tool]
llm = ChatAnthropic(model="claude-opus-4-6")
llm_with_tools = llm.bind_tools(tools)
```

Der Agent bekommt nur die Tools, die er für seine Rolle wirklich braucht – kein Tool mehr.

## ToolNode in LangGraph

LangGraph hat einen fertigen `ToolNode`, der die Tool-Calls eines LLMs automatisch ausführt:

```python
from langgraph.prebuilt import ToolNode

tool_node = ToolNode(tools)
```

Der `ToolNode` erkennt, welche Tools das LLM aufgerufen hat, führt sie aus und schreibt die Ergebnisse zurück in den State. Er gehört als Node in `nodes.py` des Workflows, nicht in den Agenten-Ordner.

Das typische Muster in einem LangGraph-Workflow:

```
Agent-Node → prüft ob Tool-Call → ToolNode führt aus → zurück zum Agent
```

## Vorgefertigte Tools nicht vergessen

LangChain bietet eine umfangreiche Sammlung fertiger Tools, die direkt eingebunden werden können:
- Tavily oder DuckDuckGo für Websuche
- Python REPL für Code-Ausführung
- Filesystem-Tools für Dateioperationen
- GitHub-Tools für Repository-Zugriff

Bevor ein Tool selbst gebaut wird, lohnt ein Blick in die LangChain-Toolbibliothek.

## Entscheidungsregel: Tool oder Subgraph?

Ein **Tool** ist die richtige Wahl, wenn:
- die Aufgabe atomar ist (eine Aktion, ein klares Ergebnis)
- das LLM selbst entscheidet, ob und wann es die Aktion ausführt
- kein eigener State oder Routing notwendig ist

Ein **Subgraph** ist die richtige Wahl, wenn:
- mehrere Schritte in einer definierten Reihenfolge ablaufen
- ein eigener State geführt werden muss
- ein Mensch oder eine Regel zwischendrin freigeben muss
- die Teilaufgabe in mehreren Workflows wiederverwendet wird

## Skills

Ein Skill ist eine wiederverwendbare Fähigkeit auf höherem Abstraktionsniveau als ein einzelnes Tool. In LangChain/LangGraph gibt es den Begriff technisch nicht – Skills werden entweder als Tools oder als Subgraphs umgesetzt.

Beispiele und ihre Umsetzung:

| Skill | Umsetzung |
|---|---|
| `load_project_context` | Subgraph (mehrere Schritte, eigener State) |
| `find_relevant_files` | Tool (atomare Suche, ein Ergebnis) |
| `generate_regression_test` | Agent-Aufruf im Workflow |
| `draft_pr_description` | Agent-Aufruf im Workflow |

## Integrationen

Kundenprozesse benötigen oft Anbindungen an externe Systeme:

- CRM, ERP
- Ticketsysteme (Jira, GitHub Issues)
- Mailboxen
- interne APIs

Diese Integrationen sollten über klar definierte Tools oder Adapter angesprochen werden – nie direkt aus einem Agenten-Prompt heraus.

## Best Practice

- Jeder Agent bekommt nur die Tools, die er in seiner Rolle wirklich braucht
- Tool-Rechte werden durch Policies begrenzt und dokumentiert
- Tool-Beschreibungen (Docstrings) sorgfältig formulieren – das LLM entscheidet danach
- Geteilte Tools in `tools/` ablegen, agenten-spezifische im Agenten-Ordner
- Bestehende LangChain-Tools prüfen, bevor etwas neu gebaut wird
