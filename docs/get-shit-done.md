# Get Shit Done (GSD Framework)

[Get Shit Done (GSD)](https://github.com/gsd-build/get-shit-done) (now maintained under [open-gsd/gsd-core](https://github.com/open-gsd/gsd-core)) is a meta-prompting, context engineering, and spec-driven development framework designed to maximize AI coding agent reliability (e.g., Claude Code, Codex, Gemini).

---

## Purpose & Core Philosophy

AI agents suffer from **context rot**—performance and reasoning degrade as context windows accumulate irrelevant output, chatter, and debug attempts.

GSD solves this through:
- **Spec-Driven Slicing:** Break work into explicit, verifiable micro-tasks before touching code.
- **Fresh Context Cycles:** Reset or isolate context windows per task phase.
- **Atomic Commits & State Machine:** Maintain state in structured markdown files (`.gsd/` or execution plans) so work can be paused, resumed, or audited.
- **Verification Gates:** Explicit test/verification requirements before declaring a step complete.

---

## Phase Workflow

GSD enforces a strict cycle for non-trivial coding tasks:

```text
  1. DISCUSS ──> 2. PLAN ──> 3. EXECUTE ──> 4. VERIFY ──> 5. COMMIT
```

1. **Discuss / Clarify:**
   - Define exact desired outcome and scope limits.
   - Establish what is *out of scope*.
2. **Plan (Spec & Tasks):**
   - Create step-by-step checklist files (e.g., `templates/gsd-plan.md` or `.gsd/PLAN.md`).
   - Identify dependencies, edge cases, and automated tests.
3. **Execute:**
   - Implement one slice at a time in isolation.
   - Keep context clean by avoiding massive unstructured tool outputs.
4. **Verify:**
   - Run empirical build/test tools. Never claim success without verification output.
5. **Commit & Closeout:**
   - Create clean, atomic Git commits for completed slices.

---

## Triage & Anti-Stall Techniques

### 10-Minute Triage
Use when work feels messy or overwhelming:
1. Write the desired outcome in one sentence.
2. List all open items without organizing.
3. Mark each item: `Do`, `Defer`, `Delegate`, `Delete`.
4. Pick the smallest item that creates visible progress.
5. Work in a single uninterrupted 25-50 minute block.

### Anti-Stall Questions
When an AI agent or developer stalls:
- Is the step too large? (Slice smaller)
- Is the verification condition missing? (Define exact pass criteria)
- Are we chasing symptoms instead of root cause? (Read full error traceback)
- Should we drop unneeded scope? (Simplify)

---

## Associated Artifacts & Utilities in Repo

- **`templates/gsd-plan.md`**: Template for focused, spec-driven execution plans.
- **`prompts/get-shit-done.md`**: Meta-prompts for planning, unblocking, and executing tasks.

---

## Links & Setup

- **Repo:** [gsd-build/get-shit-done](https://github.com/gsd-build/get-shit-done) / [open-gsd/gsd-core](https://github.com/open-gsd/gsd-core)
- **Cli / Installation:** `npx get-shit-done-cc@latest`
