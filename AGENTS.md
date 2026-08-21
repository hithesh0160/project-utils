# Agent Instructions — project-utils

This repo is a curated collection of notes, prompts, templates, and workflow docs for tools used across software projects.

## Repo Layout

- `QUICK_START.md` — fast entry point for AI agents with common queries and workflows.
- `MANIFEST.json` — structured index with tags, use cases, and tool relationships for programmatic access.
- `docs/` — organized by category. Each file covers: purpose, install, commands, workflow, when to avoid, checklist.
  - `agents/` — AI agent tools organized by function
    - `frameworks/` — Agent orchestration, execution control, workspaces
    - `memory/` — Memory engines, knowledge graphs, context management
    - `skills/` — Skill systems, registries, procedural knowledge
    - `capabilities/` — Agent tools, capabilities, specialized functions
  - `llm-inference/` — LLM serving optimization, routing, caching techniques
  - `testing/` — Test automation frameworks across languages
  - `ui-components/` — Frontend UI libraries and animation
  - `devops-infra/` — CI/CD, notifications, infrastructure
  - `dev-stacks/` — Complete development stacks
  - `analysis-docs/` — Tools for understanding/documenting codebases
  - `resources/` — Meta-resources, catalogs, evaluation checklists
- `prompts/` — reusable LLM prompts for codebase analysis, planning, and execution.
- `templates/` — repeatable document formats for tool notes, plans, and project maps.
- `README.md` — index of all files and naming conventions.

## Navigation Rules

- **For quick lookup**: check `MANIFEST.json` for tool index by use case or tag.
- **For workflow examples**: read `QUICK_START.md` for common scenarios.
- **To find a tool doc**: look in `docs/<category>/<subcategory>/<tool-name>.md`.
- **To find a prompt**: look in `prompts/<purpose>.md`.
- **To find a template**: look in `templates/<format>.md`.
- **To understand the repo structure**: read `README.md` first.
- **To find agent-specific setup**: read `docs/agents/capabilities/agent-reach.md`.

## How To Answer Questions About This Repo

1. **For quick queries**: Check `MANIFEST.json` for tool lookup by use case or tag.
2. **For common workflows**: Check `QUICK_START.md` for example scenarios.
3. **For specific tools**: Read the relevant `docs/` file directly — each is short and self-contained.
4. **For structure**: Check `README.md` for the full file index.
5. **Do not guess** tool commands — read the doc.
6. **Use checklists**: If a tool has a checklist section, use it to verify setup.

## Adding New Content

- One doc per tool in `docs/<category>/<subcategory>/`.
- Use the template at `templates/tool-note.md` for new tool docs.
- Use the template at `templates/gsd-plan.md` for execution plans.
- Keep filenames lowercase with hyphens.
- Update `README.md` repository map when adding a new file.
- Choose the appropriate category:
  - `agents/frameworks/` for agent orchestration & execution
  - `agents/memory/` for memory engines & knowledge graphs
  - `agents/skills/` for skill systems & registries
  - `agents/capabilities/` for agent tools & functions
  - `llm-inference/` for LLM optimization
  - `testing/` for test frameworks
  - `ui-components/` for frontend libraries
  - `devops-infra/` for CI/CD and infrastructure
  - `dev-stacks/` for full-stack setups
  - `analysis-docs/` for codebase analysis tools
  - `resources/` for catalogs and meta-tools

## What This Repo Is Not

- Not a code project. There is nothing to build, test, or lint.
- Not a living wiki for a specific project. Docs here are reusable across projects.
- Not an exhaustive reference. Docs capture what actually worked, not everything possible.
