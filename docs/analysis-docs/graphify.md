# Graphify

Graphify turns a project into a knowledge graph so you can inspect structure, communities, central files, inferred relationships, and cross-module paths.

## Why Use It

- Understand a large repo faster than reading files one by one.
- Find central abstractions and highly connected files.
- Ask relationship questions such as "how does auth relate to settings?"
- Keep an architecture map current as the code changes.
- Create a stronger starting point for LLM-assisted codebase work.

## Common Commands

```bash
graphify .
graphify update .
graphify query "How does X relate to Y?"
graphify path "Concept A" "Concept B"
graphify explain "Concept or file name"
```

## Recommended Project Setup

Add this instruction to a project's `AGENTS.md`:

```markdown
## graphify

This project has a graphify knowledge graph at graphify-out/.

Rules:
- Before answering architecture or codebase questions, read graphify-out/GRAPH_REPORT.md for god nodes and community structure.
- If graphify-out/wiki/index.md exists, navigate it instead of reading raw files.
- For cross-module "how does X relate to Y" questions, prefer `graphify query "<question>"`, `graphify path "<A>" "<B>"`, or `graphify explain "<concept>"` over grep.
- After modifying code files, run `graphify update .` to keep the graph current.
```

## Output Files To Review

- `graphify-out/GRAPH_REPORT.md` - high-level structure, communities, god nodes, import cycles.
- `graphify-out/wiki/index.md` - wiki-style navigation if generated.
- `graphify-out/*.json` - raw graph data for deeper inspection or automation.

## When To Use

- Before refactoring a large module.
- Before answering architecture questions.
- Before onboarding to an unfamiliar codebase.
- After major code changes, to check whether the graph still reflects the project.

## Notes And Open Questions

- Track which commands work best for each repo size.
- Record examples where graph queries found something grep would have missed.
- Keep a short list of misleading graph outputs so future usage is more precise.
