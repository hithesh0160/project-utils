# Quick Start for AI Agents

This repository contains curated tool documentation, prompts, and templates for software projects.

## How to Use This Repo

### 1. Quick Lookup (Fastest)
Read `MANIFEST.json` for:
- Complete tool index with tags and use cases
- Query by use case: "What tools help with X?"
- Query by tag: "Show me all MCP tools"
- See related tools: "What works well with Y?"

### 2. Understand Structure
Read `AGENTS.md` for:
- Repository layout and organization
- Navigation rules
- How to find specific information
- Content conventions

### 3. Browse Available Tools
Read `README.md` for:
- Full categorized index
- Human-readable descriptions
- Category overviews

## Common Queries

### "I need a tool for [use case]"
1. Check `MANIFEST.json` → `query_examples.by_use_case`
2. Or search categories in `README.md`
3. Read the specific doc in `docs/<category>/<tool>.md`

### "Show me all tools with [tag]"
1. Check `MANIFEST.json` → `query_examples.by_tag`
2. Example tags: `mcp`, `memory`, `optimization`, `testing`, `automation`

### "What tools work well together?"
1. Check `MANIFEST.json` → `related_tools`
2. See complementary tool relationships

### "How do I set up [specific tool]?"
1. Navigate to `docs/<category>/<tool>.md`
2. Each doc has: Summary, Best For, Avoid When, Setup, Commands

### "I want to document a new tool"
1. Use template at `templates/tool-note.md`
2. Follow guidelines in `AGENTS.md` → Adding New Content
3. Place in appropriate `docs/<category>/` folder

### "I need a prompt for [task]"
1. Check `prompts/` directory
2. Available: `get-shit-done.md`, `graphify-analysis.md`

## Tool Categories

- **agents/frameworks/** - Orchestration, execution (CrewAI, GSD, OpenWork)
- **agents/memory/** - Memory engines (GBrain, Cognee, LLM Wiki)
- **agents/skills/** - Skill systems (Skills.sh, Upskill, Ponytail)
- **agents/capabilities/** - Agent tools (Agent Reach, Autoresearch)
- **llm-inference/** - LLM optimization (KV Caching, PagedAttention, Omniroute)
- **testing/** - Test frameworks (Playwright, Appium, Selenium, Pytest)
- **ui-components/** - Frontend libs (Animate UI, Inspira UI, Lenis)
- **devops-infra/** - CI/CD (GitHub Actions, Telegram, FreeDomain)
- **dev-stacks/** - Full stacks (React+Vite+Capacitor, nanochat)
- **analysis-docs/** - Codebase analysis (Graphify, Image Blaster)
- **resources/** - Catalogs (Free for Dev, Tool Evaluation)

## Example Workflows

### Starting a New Web Project
```
1. Check dev-stacks/ for stack options
2. Check testing/ for test framework
3. Check devops-infra/ for CI/CD setup
4. Check ui-components/ for UI libraries
```

### Building an AI Agent
```
1. Check agents/frameworks/ for orchestration
2. Check agents/memory/ for long-term memory
3. Check agents/skills/ for skill management
4. Check agents/capabilities/ for internet access, research
5. Check llm-inference/ for optimization
```

### Optimizing LLM Inference
```
1. Start with llm-inference/kv-caching.md (fundamental technique)
2. Then llm-inference/paged-attention.md (memory optimization)
3. Consider llm-inference/omniroute.md (multi-provider routing)
```

### Setting Up Testing
```
1. Pick framework from testing/ based on language/platform
2. Add testing/allure-reports.md for reporting
3. Add testing/docker-test-runners.md for isolation
4. Add devops-infra/github-actions.md for CI
```

## File Format

Every tool doc follows this structure:
```
# Tool Name
## Summary - What it does, why it matters
## Best For - Ideal use cases
## Avoid When - When not to use
## Setup - Installation commands
## Common Commands - Key operations
## [Additional sections] - Patterns, examples, troubleshooting
```

## Key Files to Read First

For AI agents starting fresh:
1. **MANIFEST.json** (this file) - Complete structured index
2. **AGENTS.md** - Navigation and conventions
3. **README.md** - Human-readable overview

## Tags Reference

Common tags across tools:
- `mcp` - Model Context Protocol support
- `memory` - Memory/persistence features
- `optimization` - Performance optimization
- `automation` - Task automation
- `testing` - Test/QA tooling
- `llm` - LLM-related
- `agent` - Agent-specific
- `ci-cd` - Continuous integration/deployment
- `mobile` - Mobile development
- `web` - Web development

## Questions?

- **Structure unclear?** → Read `AGENTS.md`
- **Can't find a tool?** → Search `MANIFEST.json` by use_case or tag
- **Need examples?** → Check `prompts/` directory
- **Want to add content?** → Use `templates/` and follow `AGENTS.md` guidelines
