# Graphify Analysis Prompts

## Architecture Orientation

```text
Read graphify-out/GRAPH_REPORT.md first. Summarize the project's architecture using the community structure, god nodes, and surprising connections. Then identify the top files I should inspect before making code changes.
```

## Relationship Question

```text
Use Graphify to answer: "How does <concept A> relate to <concept B>?" Prefer graphify query/path/explain output over raw grep. Include the path of reasoning and the files that support it.
```

## Refactor Planning

```text
Using the Graphify report, identify the modules most likely to be affected by refactoring <module or feature>. List central nodes, neighboring communities, likely integration points, and tests or manual checks to run.
```

## Staleness Check

```text
Compare the graph report commit with the current HEAD. If stale, update the graph. Then explain whether the current graph is safe to use for architecture decisions.
```
