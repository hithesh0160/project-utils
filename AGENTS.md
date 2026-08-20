# Agent Instructions — project-utils

This repo is a curated collection of notes, prompts, templates, and workflow docs for tools used across software projects.

## Repo Layout

- `docs/` — one file per tool or concept. Each file covers: purpose, install, commands, workflow, when to avoid, checklist.
- `prompts/` — reusable LLM prompts for codebase analysis, planning, and execution.
- `templates/` — repeatable document formats for tool notes, plans, and project maps.
- `README.md` — index of all files and naming conventions.

## Navigation Rules

- **To find a tool doc**: look in `docs/<tool-name>.md`.
- **To find a prompt**: look in `prompts/<purpose>.md`.
- **To find a template**: look in `templates/<format>.md`.
- **To understand the repo structure**: read `README.md` first.
- **To find agent-specific setup**: read `docs/agent-reach.md`.

## How To Answer Questions About This Repo

1. Check `README.md` for the full file index.
2. Read the relevant `docs/` file directly — each is short and self-contained.
3. Do not guess tool commands — read the doc.
4. If a tool has a checklist section, use it to verify setup.

## Adding New Content

- One doc per tool in `docs/`.
- Use the template at `templates/tool-note.md` for new tool docs.
- Use the template at `templates/gsd-plan.md` for execution plans.
- Keep filenames lowercase with hyphens.
- Update `README.md` repository map when adding a new file.

## What This Repo Is Not

- Not a code project. There is nothing to build, test, or lint.
- Not a living wiki for a specific project. Docs here are reusable across projects.
- Not an exhaustive reference. Docs capture what actually worked, not everything possible.
