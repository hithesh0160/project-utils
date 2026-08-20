# Project Knowledge Tools

This repository collects reusable notes, workflows, prompts, and evaluation checklists for tools that help understand, document, and maintain software projects.

Use it for utilities such as Graphify, LLM-generated wikis, codebase maps, architecture explainers, repo analysis prompts, and automation recipes.

## Repository Map

- `docs/graphify.md` - Graphify setup, commands, and project workflow notes.
- `docs/llm-wiki.md` - LLM wiki concepts, structure, and usage patterns.
- `docs/tool-evaluation.md` - Checklist for deciding whether a tool is worth keeping.
- `prompts/graphify-analysis.md` - Reusable prompts for graph-based codebase analysis.
- `templates/tool-note.md` - Standard template for documenting a new utility.
- `templates/project-knowledge-map.md` - Template for recording how a project is documented.

## What Belongs Here

- Tool setup steps and troubleshooting notes.
- Reusable commands that you run across projects.
- Prompts that produce consistently useful codebase summaries.
- Comparison notes between similar tools.
- Examples of generated outputs that are worth preserving.
- Decisions about when to use a tool and when to avoid it.

## Suggested Workflow

1. Add one note per tool in `docs/`.
2. Keep reusable prompts in `prompts/`.
3. Store repeatable formats in `templates/`.
4. Add project-specific examples only when they teach a reusable pattern.
5. Review notes after each real project use and update what actually worked.

## Naming Convention

Use short lowercase filenames with hyphens:

```text
docs/graphify.md
docs/llm-wiki.md
docs/context-engineering.md
prompts/repo-review.md
templates/tool-note.md
```
