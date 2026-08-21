# Skills.sh (`vercel-labs/skills`)

[Skills.sh](https://skills.sh) (maintained by [Vercel Labs](https://github.com/vercel-labs/skills)) is an open-source registry and package manager ("npm for AI Agent Skills") that allows developers to discover, share, install, and manage procedural agent skills across 38+ AI coding agents (Claude Code, Cursor, Windsurf, GitHub Copilot, etc.).

---

## Core Concept

Agent Skills provide AI agents with procedural knowledge, API integrations, and specialized domain instructions. `skills.sh` standardizes how skills are packaged, published, and installed into local agent environments (`.skills/` or `.claude/skills/`).

```text
       skills.sh Registry
               │
               ▼  (npx skills add <skill-name>)
  Local Project (.skills/ | SKILL.md)
               │
               ▼
  Platform-Agnostic Agent Loading (Claude Code, Cursor, Windsurf, Copilot)
```

---

## Key Features

- **Platform Agnostic:** Works across 38+ AI coding agents and IDE extensions.
- **Unified CLI:** Install skills directly from GitHub repositories or the central registry.
- **Standardized Spec:** Built on the `SKILL.md` format (YAML frontmatter + markdown instructions + optional helper scripts).
- **Auto-Discovery:** Agents detect installed skills dynamically when relevant tasks match skill triggers.

---

## CLI Usage

```bash
# Search for available skills
npx skills find <query>

# Install a skill into the current workspace
npx skills add <owner/repo>

# List installed skills in current workspace
npx skills list

# Remove an installed skill
npx skills remove <skill-name>
```

---

## Skill File Layout (`SKILL.md`)

```markdown
---
name: my-custom-skill
description: "Brief explanation of what this skill enables the agent to do."
---

# My Custom Skill

Detailed procedural steps, rules, and commands for the AI agent...
```

---

## Resources

- **Website:** [skills.sh](https://skills.sh)
- **GitHub Repository:** [vercel-labs/skills](https://github.com/vercel-labs/skills)
- **License:** MIT / Open Source
