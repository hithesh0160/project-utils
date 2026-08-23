# unlazy (Leonxlnx)

## Summary

unlazy is a completion discipline skill for AI agents built on the Depth Tree method, which splits substantial tasks N layers deep and allocates the full time budget to each leaf to prevent premature completion. It provides a gate-based verification system with runnable checks, approval workflows, evidence collection, and optional Claude Code Stop hooks to ensure agents complete work thoroughly rather than abandoning tasks early or producing partial solutions.

## Best For

- Substantial multi-part AI agent tasks requiring thorough completion
- Projects where AI agents historically abandon work or deliver partial solutions
- Verification-driven workflows with executable acceptance criteria
- Parallel agent work requiring ownership coordination and state management
- Research-backed approaches to combating LLM laziness and underthinking
- Teams needing auditable evidence trails for agent-completed work
- Long-horizon iterative tasks prone to degradation over time

## Avoid When

- Simple single-step tasks that don't benefit from deep decomposition
- Exploratory or brainstorming work without concrete deliverables
- Projects without executable verification criteria
- Work requiring continuous human oversight (unlazy enables autonomy)
- Agents that already demonstrate consistent task completion
- Quick prototyping where gate overhead exceeds value

## Setup

### Installation via Skills CLI

```bash
# Claude Code, Codex, Cursor, etc.
npx skills add Leonxlnx/unlazy

# User-level install
npx skills add -g Leonxlnx/unlazy

# All detected agents
npx skills add --all Leonxlnx/unlazy
```

### Manual Installation

```bash
# Claude Code
~/.claude/skills/unlazy

# Codex CLI
~/.codex/skills/unlazy

# Clone into the appropriate directory
git clone https://github.com/Leonxlnx/unlazy.git ~/.claude/skills/unlazy
```

### Requirements

- Node.js 16 or newer
- No third-party runtime dependencies
- Supports Unix (sh) and Windows (cmd/PowerShell) shells

## Core Concepts

### The Depth Tree Method

Instead of linear task execution, decompose work N layers deep:

```
Task (100% budget)
├── Subtask 1 (100% budget)
│   ├── Leaf 1.1 (100% budget)
│   └── Leaf 1.2 (100% budget)
└── Subtask 2 (100% budget)
    ├── Leaf 2.1 (100% budget)
    └── Leaf 2.2 (100% budget)
```

Each leaf receives the full attention budget, multiplying effort with depth.

### Gates: Executable Acceptance Criteria

Gates are verification checkpoints with:
- **CHECK**: Shell command to execute
- **EXPECT**: Required output pattern
- **EVIDENCE**: Recorded execution results

```markdown
# Gates: pricing behavior

- [ ] G1: pricing fixtures render expected tiers
  CHECK: node scripts/verify-pricing.mjs
  EXPECT: pricing verification passed
  EVIDENCE: pending

- [ ] G2: checkout integration succeeds
  CHECK: node scripts/verify-checkout.mjs
  EXPECT: checkout verification passed
  CWD: packages/checkout
  EVIDENCE: pending
```

## Basic Usage

### Quick Start with Trigger

```bash
# In your agent prompt
/unlazy tree 5 refactor the payment module and verify every migration path
```

### Create a Gate Ledger

```bash
# Copy template
cp ~/.claude/skills/unlazy/templates/gates-leaf.md GATES.md

# Edit GATES.md with your checks
```

### Check Gates (Inspect Only)

```bash
# Status mode never executes
node ~/.claude/skills/unlazy/scripts/gate-check.mjs --status GATES.md
```

### Dry Run (New Oracles)

```bash
# Shows resolved command without executing
node ~/.claude/skills/unlazy/scripts/gate-check.mjs GATES.md
```

### Approve and Execute

```bash
# Review commands, then approve
node ~/.claude/skills/unlazy/scripts/gate-check.mjs --approve GATES.md
```

### Re-verify Completed Work

```bash
# Run all gates including completed ones
node ~/.claude/skills/unlazy/scripts/gate-check.mjs --reverify GATES.md
```

## Gate Contract Format

### Complete Runnable Gate

```markdown
- [ ] G1: API returns valid responses
  CHECK: node test/api-smoke.mjs
  EXPECT: all endpoints passed
  EVIDENCE: pending
```

### With Working Directory

```markdown
- [ ] G2: frontend builds successfully
  CHECK: npm run build
  EXPECT: build completed
  CWD: packages/frontend
  EVIDENCE: pending
```

### Evidence Records

After execution:

```markdown
- [x] G1: API returns valid responses
  CHECK: node test/api-smoke.mjs
  EXPECT: all endpoints passed
  EVIDENCE: exit 0, /bin/sh, /path/to/project, PATH:node:npm:..., matched "all endpoints passed"
```

Evidence includes:
- Resolved shell
- Resolved working directory
- Exit status
- PATH fingerprint
- Matching output

## Advanced Features

### Parallel Work with Scoped Pipelines

```bash
# Project structure
.unlazy/api/
├── PLAN.md
├── GATES.md
└── gates/
    ├── leaf-1.1.md
    ├── leaf-1.2.md
    └── node-1.md

# Claim ownership before execution
node scripts/gate-check.mjs --scope api --leaf leaf-1.1 --claim

# Execute with scope
node scripts/gate-check.mjs --scope api --leaf leaf-1.1 --approve
```

**Ownership Declaration:**

```markdown
OWNS: src/api/routes
OWNS: src/api/middleware
OWNS: test/api/*.test.js
```

### Parallel Gate Execution

```bash
# Sequential (default)
node scripts/gate-check.mjs --approve GATES.md

# Parallel with job limit
node scripts/gate-check.mjs --jobs 4 --approve GATES.md
```

Job limits: 1-64 (sequential execution remains default)

### Shell Selection

```bash
# Explicit shell
node scripts/gate-check.mjs --shell /bin/bash GATES.md

# Via environment
export UNLAZY_SHELL=/bin/bash
node scripts/gate-check.mjs GATES.md
```

Platform defaults:
- Unix: `/bin/sh`
- Windows: `%ComSpec%` (cmd.exe or PowerShell)

### Claude Code Stop Hook

Blocks Claude Code execution until gates pass:

```bash
# Install hook (with consent)
node scripts/install-hooks.mjs

# Scoped hook
node scripts/install-hooks.mjs --scope api

# Uninstall
node scripts/install-hooks.mjs --uninstall

# Shared project settings (not portable)
node scripts/install-hooks.mjs --shared

# Global user settings
node scripts/install-hooks.mjs --global
```

Hook behavior:
- Scans resolved ledger for unmet gates
- Returns `decision: "block"` to Claude Code
- Releases after 6 consecutive blocks without progress
- Does not execute checks itself

## Orchestration States

### Leaf States

- `WAITING` - Dependencies not met
- `READY` - Can execute
- `IN-FLIGHT` - Currently executing
- `VERIFIED` - All gates passed
- `ABANDONED` - Explicitly skipped (requires reason)

### Branch States

- `OPEN` - Work in progress
- `VERIFIED` - All child leaves verified
- `ABANDONED` - Explicitly skipped (requires reason)

### Rolling Dispatch

Start ready leaves immediately without waiting for unrelated work:

```bash
# Leaf 1.1 completes → unblocks Leaf 1.2
# Start Leaf 1.2 even if Leaf 2.1 is still running
```

## Security Model

### Approval System

Approval records stored under:
- Default: `~/.unlazy/approved`
- Custom: `$UNLAZY_APPROVAL_DIR`

Each record binds:
- Absolute ledger and gate ID
- Exact `CHECK:` and `EXPECT:` text
- Resolved `CWD:` and shell
- Timeout and output limits
- Platform and full `PATH`

Editing any bound input requires re-approval.

### Execution Boundaries

**What approval protects:**
- Prevents execution without explicit consent
- Detects command/environment changes
- Requires re-approval for modified gates

**What approval does NOT restrict:**
- Filesystem access
- Network access
- Environment variables
- Credentials

Checks run with full ambient permissions. Review commands before approval.

### Scope and Lease Coordination

**Coordination guards:**
- Serialized lease claims
- Disjoint path validation
- Conservative conflict detection

**Not provided:**
- Write isolation (use separate worktrees)
- Cache separation (configure distinct cache dirs)
- Process sandboxing

## Templates

### Leaf Template (Solo Task)

`templates/gates-leaf.md`:

```markdown
# Gates: <feature-name>

- [ ] G1: <first-check>
  CHECK: <command>
  EXPECT: <pattern>
  EVIDENCE: pending

- [ ] G2: <second-check>
  CHECK: <command>
  EXPECT: <pattern>
  EVIDENCE: pending
```

### Branch Template (Parent Verification)

For hierarchical verification:

```markdown
# Gates: <feature-branch>

- [ ] B1: all API leaves verified
  BRANCH: leaf-1.1, leaf-1.2, leaf-1.3
  EVIDENCE: pending
```

### Plan Template

Defines work structure:

```markdown
# Plan: <scope-name>

## Dependencies
- Leaf 1.1 → Leaf 1.2
- Leaf 2.1 → Branch 1

## Ownership
- Leaf 1.1: src/auth
- Leaf 1.2: src/api
```

## Common Patterns

### Good Gate Design

```markdown
# ✅ Tests actual artifact
- [ ] G1: built binary runs correctly
  CHECK: ./dist/app --version
  EXPECT: v1.2.3

# ✅ Success-only marker
- [ ] G2: all tests pass
  CHECK: npm test
  EXPECT: All tests passed

# ✅ Measures supplied figures
- [ ] G3: bundle size under limit
  CHECK: node check-size.mjs
  EXPECT: bundle: 245KB (limit: 250KB)

# ✅ Absence check with control
- [ ] G4: no TODO comments in src
  CHECK: node check-todos.mjs
  EXPECT: 0 TODOs in src/, 1 in test/fixtures/
```

### Poor Gate Design

```markdown
# ❌ No executable check
- [ ] G1: code looks good
  EVIDENCE: I reviewed it

# ❌ Copies output into EXPECT
- [ ] G2: tests pass
  CHECK: npm test
  EXPECT: Test Suites: 4 passed, 4 total
         Tests:       42 passed, 42 total

# ❌ Generic success message
- [ ] G3: build works
  CHECK: make
  EXPECT: Build succeeded
```

## CLI Reference

```bash
# Status (never executes)
node scripts/gate-check.mjs --status GATES.md

# Dry run for new oracles (shows resolved command)
node scripts/gate-check.mjs GATES.md

# Approve and execute
node scripts/gate-check.mjs --approve GATES.md

# Re-verify all gates
node scripts/gate-check.mjs --reverify GATES.md

# Parallel execution
node scripts/gate-check.mjs --jobs 4 --approve GATES.md

# Custom shell
node scripts/gate-check.mjs --shell /bin/bash GATES.md

# Scoped pipeline
node scripts/gate-check.mjs --scope api --leaf leaf-1.1 --claim
node scripts/gate-check.mjs --scope api --leaf leaf-1.1 --approve

# Help
node scripts/gate-check.mjs --help
```

## Research Basis

unlazy addresses documented AI agent limitations:

**Laziness & Premature Completion:**
- [Quantifying Laziness in LLMs](https://arxiv.org/abs/2512.20662) - Partial compliance and truncation
- [SlopCodeBench](https://arxiv.org/abs/2603.24755) - 14.8% checkpoint success, agents rarely complete tasks end-to-end

**Underthinking & Overthinking:**
- [Thoughts Are All Over the Place](https://arxiv.org/abs/2501.18585) - Premature exploration stopping
- [When More Thinking Hurts](https://arxiv.org/abs/2604.10739) - Excessive compute without benefit
- [OptimalThinkingBench](https://arxiv.org/abs/2508.13141) - Balance between over/underthinking

**Long-Horizon Tasks:**
- [Measuring AI Ability to Complete Long Software Tasks](https://arxiv.org/abs/2503.14499) - Task completion challenges
- [METR Time Horizon 1.1](https://metr.org/blog/2026-1-29-time-horizon-1-1/) - 130.8-196.5 day doubling time

**Test-Time Compute:**
- [s1: Simple test-time scaling](https://arxiv.org/abs/2501.19393) - Budget forcing techniques
- [Test-Time Scaling in Reasoning Models](https://arxiv.org/abs/2509.06861) - Limitations for knowledge tasks

## Troubleshooting

### Gates Not Executing

```bash
# Check approval status
node scripts/gate-check.mjs --status GATES.md

# Review what would execute
node scripts/gate-check.mjs GATES.md

# Approve if commands are correct
node scripts/gate-check.mjs --approve GATES.md
```

### PATH or Shell Mismatch

```bash
# Show resolved PATH before execution
node scripts/gate-check.mjs GATES.md

# Specify shell explicitly
node scripts/gate-check.mjs --shell /bin/bash --approve GATES.md

# Set via environment
export UNLAZY_SHELL=/bin/bash
```

### Parallel Work Conflicts

```bash
# Check lease claims
# Lease system is conservative - may reject safe pairs

# Use separate worktrees for conflicting output
git worktree add ../project-branch2 branch2

# Configure separate cache directories
export CACHE_DIR=.cache/leaf-1.1
```

### Stop Hook Not Blocking

```bash
# Check hook installation
cat .claude/settings.local.json

# Reinstall hook
node scripts/install-hooks.mjs

# Hook releases after 6 consecutive blocks without progress
# Make ledger progress or temporarily uninstall
```

## Repository Structure

```
SKILL.md                         # Core instructions
SECURITY.md                      # Threat model
references/
  ├── gates.md                   # Format specification
  ├── method.md                  # Depth Tree method
  ├── orchestration.md           # States and dispatch
  ├── parallel.md                # Coordination limits
  └── token-economy.md           # Cost discipline
scripts/
  ├── gate-check.mjs             # Main checker
  ├── install-hooks.mjs          # Hook installer
  └── stop-hook.mjs              # Claude Code hook
templates/
  ├── gates-leaf.md              # Solo task template
  ├── gates-branch.md            # Parent verification
  └── plan.md                    # Pipeline structure
tests/                           # Regression tests
```

## Integration Examples

### With Claude Code

```bash
# Install skill
npx skills add Leonxlnx/unlazy

# In Claude Code prompt
/unlazy tree 4 implement user authentication with OAuth and verify all flows

# Claude will:
# 1. Decompose into 4-level depth tree
# 2. Create GATES.md with acceptance criteria
# 3. Execute with full budget per leaf
# 4. Verify each gate before marking complete
```

### With CI/CD

```yaml
# .github/workflows/verify-gates.yml
name: Verify Gates
on: [push, pull_request]

jobs:
  verify:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
      - name: Verify all gates
        run: |
          node scripts/gate-check.mjs --reverify GATES.md
```

### With Pre-commit Hooks

```bash
# .git/hooks/pre-commit
#!/bin/sh
if [ -f GATES.md ]; then
  node scripts/gate-check.mjs --status GATES.md || {
    echo "Gates not satisfied"
    exit 1
  }
fi
```

## Best Practices

### Gate Authoring

1. **Test artifacts, not processes**: Verify built output, not build logs
2. **Use success-only markers**: Print unique strings only when all checks pass
3. **Measure, don't copy**: Calculate metrics rather than hardcoding them
4. **Provide positive controls**: Test absence checks against known positives
5. **Match evidence to risk**: Consequential manual steps need proportional evidence

### Decomposition

1. **Clear leaf boundaries**: Each leaf should be independently verifiable
2. **Disjoint ownership**: No overlapping file claims between parallel leaves
3. **Explicit dependencies**: Document what blocks what in PLAN.md
4. **Budget allocation**: Give each leaf full time, don't subdivide

### Approval Workflow

1. **Review all commands**: Read scripts before approving
2. **Check PATH**: Ensure required tools are available
3. **Verify CWD**: Confirm working directories are correct
4. **Test manually first**: Run commands by hand before approval
5. **Keep approval dir secure**: Store outside repository

## Known Limitations

- Lease coordination is conservative, may reject safe concurrency
- Stop hook state is session-keyed, not work-content keyed
- Approval is consent, not a sandbox (full ambient access)
- Parser rejects zero-gate ledgers (design choice)
- Windows PATH/shell mismatch between Git Bash and PowerShell

## Related Skills

- **Ponytail** (`docs/agents/skills/dietrichgebert-ponytail.md`) - Anti-over-engineering decision ladder
- **Skills.sh** (`docs/agents/skills/skills-sh.md`) - Package manager for agent skills
- **Get Shit Done** (`docs/agents/frameworks/get-shit-done.md`) - Spec-driven framework

## Reference

- [GitHub Repository](https://github.com/Leonxlnx/unlazy)
- [SKILL.md](https://github.com/Leonxlnx/unlazy/blob/main/SKILL.md) - Core instructions
- [SECURITY.md](https://github.com/Leonxlnx/unlazy/blob/main/SECURITY.md) - Threat model
- [CONTRIBUTING.md](https://github.com/Leonxlnx/unlazy/blob/main/CONTRIBUTING.md) - Contribution guide
- [CHANGELOG.md](https://github.com/Leonxlnx/unlazy/blob/main/CHANGELOG.md) - Version history

## Lessons Learned

- Explicit gates prevent agents from "checking the box" without verification
- Evidence trails reveal whether work was actually tested or just marked complete
- Depth Tree method forces thorough exploration rather than surface-level completion
- Approval workflow catches dangerous commands before execution
- Stop hook prevents premature "done" signals from agents
- Parallel coordination requires explicit ownership to avoid silent conflicts
- Research-backed design addresses documented LLM failure modes
- Shell and PATH disclosure prevents verification mismatches

## Status

Status: active development
Version: 2.1.0 (unreleased, pin commits for stability)
Stars: 1.2k on GitHub
License: MIT
Requires: Node.js 16+, zero runtime dependencies
