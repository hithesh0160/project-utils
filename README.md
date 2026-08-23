# Project Utils

This repository collects reusable notes, workflows, prompts, and evaluation checklists for tools that help understand, document, and maintain software projects.

Use it for utilities such as AI agents, LLM inference optimization, test automation, codebase analysis, and development workflows.

## Repository Map

### Core Files
- `AGENTS.md` - Agent entry point with repo layout, navigation rules, and content conventions.
- `QUICK_START.md` - Fast entry point for AI agents with common queries and example workflows.
- `MANIFEST.json` - Structured index with tags, use cases, and tool relationships for programmatic access.

### AI Agents (`docs/agents/`)

#### Agent Frameworks (`docs/agents/frameworks/`)
Agent orchestration, execution control, and workspaces.

- `docs/agents/frameworks/crewai-llm-hooks.md` - CrewAI LLM Hooks for intercepting and controlling language model interactions.
- `docs/agents/frameworks/get-shit-done.md` - Get Shit Done (`gsd-build/get-shit-done` / `open-gsd/gsd-core`) spec-driven framework to eliminate context rot.
- `docs/agents/frameworks/different-ai-openwork.md` - OpenWork (`different-ai/openwork`) local-first desktop application and agent workspace.

#### Agent Memory (`docs/agents/memory/`)
Memory engines, knowledge graphs, and context management.

- `docs/agents/memory/garrytan-gbrain.md` - GBrain (`garrytan/gbrain`) AI agent memory engine with self-wiring typed knowledge graphs by Garry Tan.
- `docs/agents/memory/topoteretes-cognee.md` - Cognee (`topoteretes/cognee`) AI memory engine with hybrid vector search and semantic knowledge graphs.
- `docs/agents/memory/llm-wiki.md` - LLM Wiki (`Oshayr/LLM-Wiki`) plugin for autonomous knowledge management, research-on-miss, and local web UI.
- `docs/agents/memory/ultracontext.md` - Shared context infrastructure for AI agents and coding sessions.

#### Agent Skills (`docs/agents/skills/`)
Skill systems, registries, and procedural knowledge.

- `docs/agents/skills/skills-sh.md` - Skills.sh (`vercel-labs/skills`) open-source package manager and registry for AI agent skills.
- `docs/agents/skills/unlazy.md` - unlazy (`Leonxlnx/unlazy`) anti-laziness skill using Depth Tree method with gate-based verification.
- `docs/agents/skills/voltagent-awesome-openclaw-skills.md` - Awesome OpenClaw Skills (`VoltAgent/awesome-openclaw-skills`) curated index of 5,000+ agent skills.
- `docs/agents/skills/huggingface-upskill.md` - Hugging Face Upskill (`huggingface/upskill`) skill generation, distillation, and evaluation tool.
- `docs/agents/skills/dietrichgebert-ponytail.md` - Ponytail (`DietrichGebert/ponytail`) anti-over-engineering decision ladder skill for AI agents.

#### Agent Capabilities (`docs/agents/capabilities/`)
Agent tools, capabilities, and specialized functions.

- `docs/agents/capabilities/agent-browser.md` - Agent Browser (Vercel Labs) fast native Rust CLI for AI agent browser automation with MCP support.
- `docs/agents/capabilities/agent-reach.md` - Agent Reach CLI for reading and searching Twitter, Reddit, YouTube, GitHub, and more.
- `docs/agents/capabilities/apify.md` - Apify full-stack web scraping platform with 53,000+ Actors for platform-specific scraping and serverless automation.
- `docs/agents/capabilities/autoresearch.md` - Autoresearch (`karpathy/autoresearch`) autonomous ML research loop and experiment framework by Andrej Karpathy.
- `docs/agents/capabilities/firecrawl.md` - Firecrawl API-first web context platform for AI agents converting web pages to LLM-ready markdown.
- `docs/agents/capabilities/microsoft-agent-lightning.md` - Agent Lightning (`microsoft/agent-lightning`) RL & prompt optimization framework for AI agents.

### LLM Inference (`docs/llm-inference/`)
LLM serving optimization, routing, and caching techniques.

- `docs/llm-inference/kv-caching.md` - KV (Key-Value) caching fundamentals for optimizing transformer inference by storing attention states.
- `docs/llm-inference/paged-attention.md` - PagedAttention memory optimization for LLM inference with block-based KV cache management.
- `docs/llm-inference/omniroute.md` - AI gateway notes for one endpoint, routing, fallback, MCP, and model providers.

### Testing (`docs/testing/`)
Test automation frameworks across languages.

- `docs/testing/allure-reports.md` - Allure test report generation and publishing.
- `docs/testing/appium-android.md` - Android Appium test workflow.
- `docs/testing/docker-test-runners.md` - Dockerized test execution patterns.
- `docs/testing/java-selenium-testng.md` - Maven, Selenium, and TestNG automation setup.
- `docs/testing/playwright.md` - Playwright browser automation and MCP notes.
- `docs/testing/python-fastapi-pytest.md` - Python service and testing stack notes.

### UI Components (`docs/ui-components/`)
Frontend UI libraries and animation.

- `docs/ui-components/animate-ui.md` - Animate UI (`imskyleen/animate-ui`) animated React component library with shadcn/ui integration.
- `docs/ui-components/inspira-ui.md` - Inspira UI (`unovue/inspira-ui`) Vue 3 & Nuxt 3 component library porting Aceternity & Magic UI.
- `docs/ui-components/lenis-smooth-scroll.md` - Lenis smooth scroll library setup, options, React adapter, and GSAP integration.

### DevOps & Infrastructure (`docs/devops-infra/`)
CI/CD, notifications, and infrastructure.

- `docs/devops-infra/github-actions.md` - CI, scheduled jobs, artifacts, and repo automation patterns.
- `docs/devops-infra/telegram-automation.md` - Telegram bot notifications from scripts and CI.
- `docs/devops-infra/digitalplatdev-freedomain.md` - FreeDomain (`DigitalPlatDev/FreeDomain`) open-source free domain registration and DNS service.

### Development Stacks (`docs/dev-stacks/`)
Complete development stacks.

- `docs/dev-stacks/react-vite-capacitor.md` - React, Vite, Capacitor, Firebase app workflow.
- `docs/dev-stacks/karpathy-nanochat.md` - nanochat (`karpathy/nanochat`) full-stack single-GPU ChatGPT training harness by Andrej Karpathy.

### Analysis & Documentation (`docs/analysis-docs/`)
Tools for understanding and documenting codebases.

- `docs/analysis-docs/graphify.md` - Graphify setup, commands, and project workflow notes.
- `docs/analysis-docs/neilsonnn-image-blaster.md` - Image Blaster (`neilsonnn/image-blaster`) image-to-3D-world generative pipeline for Claude Code.

### Resources (`docs/resources/`)
Meta-resources, catalogs, and evaluation checklists.

- `docs/resources/free-for-dev.md` - Free for Dev (`free-for.dev`) curated catalog of SaaS, PaaS, DBaaS, and APIs with free tiers.
- `docs/resources/tool-evaluation.md` - Checklist for deciding whether a tool is worth keeping.

### Prompts & Templates
- `prompts/get-shit-done.md` - Prompts for planning, unblocking, and finishing work.
- `prompts/graphify-analysis.md` - Reusable prompts for graph-based codebase analysis.
- `templates/gsd-plan.md` - Template for a focused execution plan.
- `templates/tool-note.md` - Standard template for documenting a new utility.
- `templates/project-knowledge-map.md` - Template for recording how a project is documented.

## What Belongs Here

- Tool setup steps and troubleshooting notes.
- Reusable commands that you run across projects.
- Prompts that produce consistently useful codebase summaries.
- Execution workflows that help finish important work.
- Comparison notes between similar tools.
- Examples of generated outputs that are worth preserving.
- Decisions about when to use a tool and when to avoid it.

## Suggested Workflow

1. Add one note per tool in `docs/<category>/`.
2. Keep reusable prompts in `prompts/`.
3. Store repeatable formats in `templates/`.
4. Add project-specific examples only when they teach a reusable pattern.
5. Review notes after each real project use and update what actually worked.
6. Choose the appropriate category for new docs:
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

## Naming Convention

Use short lowercase filenames with hyphens:

```text
docs/agents/frameworks/crewai-llm-hooks.md
docs/agents/memory/gbrain.md
docs/llm-inference/kv-caching.md
docs/testing/playwright.md
prompts/repo-review.md
templates/tool-note.md
```
