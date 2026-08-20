# Agent Reach

Agent reach is the practice of structuring a repo so AI agents can navigate it reliably without needing a human to explain the layout each time.

A repo with good agent reach gives agents a clear entry point, consistent file structure, short self-contained docs, and explicit instructions in a root `AGENTS.md`.

## Why It Matters

Without agent reach:
- Agents read random files and miss the important ones.
- Agents hallucinate commands that do not exist in this project.
- Context windows fill with noise before the agent finds the relevant content.
- The same orientation work repeats every session.

With agent reach:
- An agent reads `AGENTS.md` and knows where everything is.
- Each `docs/` file answers questions about one tool without needing other files.
- Prompts and templates are findable in one step.
- Sessions start productive immediately.

## What Good Agent Reach Looks Like

- `AGENTS.md` at the repo root with layout, navigation rules, and content conventions.
- Short, self-contained files. Each doc covers one tool or concept completely.
- Consistent file naming so an agent can predict where a file lives.
- Checklists at the end of docs so agents can verify setup without guessing.
- Clear separation between stable docs and temporary investigation notes.

## This Repo's Agent Reach Setup

```text
AGENTS.md              — agent entry point, layout, navigation rules
docs/                  — one file per tool or concept
prompts/               — reusable LLM prompts
templates/             — repeatable doc formats
README.md              — human-readable index of all files
```

Navigation path for an agent:
1. Read `AGENTS.md`.
2. Read `README.md` for the full file index.
3. Open the relevant `docs/` file.
4. Use the checklist at the bottom of the doc to verify setup.

## Adding Agent Reach To A New Repo

1. Create `AGENTS.md` at the repo root.
2. Write a short layout description covering all top-level directories.
3. Add navigation rules: what to read first, how to find things, what not to assume.
4. Add content conventions: how files are named, what belongs in each folder.
5. Keep each doc short enough that an agent can read it in one pass.
6. Test by asking an agent a question about the repo without giving any extra context.

## Signs Agent Reach Is Working

- Agent finds the right doc without being told the filename.
- Agent does not invent commands that are not in the docs.
- Agent uses the checklist to verify its own work.
- Agent updates `README.md` when adding a new file.

## Signs Agent Reach Needs Work

- Agent asks "where is the doc for X?" when there is a clear naming convention.
- Agent reads five files before finding the relevant one.
- Agent suggests commands that only work in a different tool or version.
- Agent duplicates content that already exists in a doc.

## Avoid When

- The repo is a single-file script with no structure to navigate.
- The team does not use AI agents and has no plans to.
- The overhead of maintaining `AGENTS.md` is higher than the benefit.

## Checklist

- `AGENTS.md` exists at the repo root.
- Layout section covers all top-level directories.
- Navigation rules tell the agent what to read first.
- Content conventions match the actual file naming in the repo.
- Each doc in `docs/` is self-contained.
- `README.md` file index is current.
