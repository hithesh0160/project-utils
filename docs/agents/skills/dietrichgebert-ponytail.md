# Ponytail (`DietrichGebert/ponytail`)

[Ponytail](https://github.com/DietrichGebert/ponytail) (by [DietrichGebert](https://github.com/DietrichGebert)) is an open-source anti-over-engineering skill and decision-framework plugin for AI coding agents (Claude Code, Cursor, Windsurf, Copilot, Codex, Gemini).

It trains AI agents to "think like the laziest senior dev in the room," enforcing YAGNI (You Aren't Gonna Need It) and DRY principles to cut token costs, bloated dependencies, and extraneous boilerplate.

---

## The 7-Step "Decision Ladder"

Before writing a single line of new code, Ponytail forces the agent to evaluate the task through a strict decision hierarchy:

```text
 1. NEED TO EXIST? ──────────> (No? Delete / Skip task completely)
        │
        ▼
 2. ALREADY IN CODEBASE? ────> (Yes? Reuse existing helper / module)
        │
        ▼
 3. STANDARD LIBRARY HAS IT? ─> (Yes? Use stdlib, do not install packages)
        │
        ▼
 4. NATIVE PLATFORM HAS IT? ──> (Yes? Use HTML/CSS/browser/OS native API)
        │
        ▼
 5. ALREADY-INSTALLED PKG? ──> (Yes? Use existing package in package.json/pyproject)
        │
        ▼
 6. CAN BE ONE LINE? ────────> (Yes? Write concise inline solution)
        │
        ▼
 7. MINIMUM CODE THAT WORKS ─> (Write exact, essential code only)
```

---

## Intensity Levels

Ponytail allows configuring how aggressively the agent prunes unnecessary code:

- `lite`: Soft suggestions to reduce dependency bloat and avoid over-abstraction.
- `full` *(Default)*: Enforces strict adherence to the 7-step Decision Ladder.
- `ultra`: Extreme minimalist mode—refuses to add abstractions, custom classes, or new dependencies unless proven mandatory.

---

## Installation & Setup

```bash
# Add to agent skills via skills CLI or ClawHub
npx skills add DietrichGebert/ponytail
```

---

## Resources

- **GitHub Repository:** [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail)
- **License:** MIT License / Open Source
