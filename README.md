# Project Utils

This repository collects reusable notes, workflows, prompts, and evaluation checklists for tools that help understand, document, and maintain software projects.

Use it for utilities such as Graphify, LLM-generated wikis, codebase maps, architecture explainers, repo analysis prompts, execution systems, and automation recipes.

## Repository Map

- `AGENTS.md` - Agent entry point with repo layout, navigation rules, and content conventions.
- `docs/agent-reach.md` - Agent Reach CLI for reading and searching Twitter, Reddit, YouTube, GitHub, and more.
- `docs/graphify.md` - Graphify setup, commands, and project workflow notes.
- `docs/get-shit-done.md` - Get Shit Done (`gsd-build/get-shit-done` / `open-gsd/gsd-core`) spec-driven framework to eliminate context rot.
- `docs/ultracontext.md` - Shared context infrastructure for AI agents and coding sessions.
- `docs/omniroute.md` - AI gateway notes for one endpoint, routing, fallback, MCP, and model providers.
- `docs/github-actions.md` - CI, scheduled jobs, artifacts, and repo automation patterns.
- `docs/python-fastapi-pytest.md` - Python service and testing stack notes.
- `docs/allure-reports.md` - Allure test report generation and publishing.
- `docs/telegram-automation.md` - Telegram bot notifications from scripts and CI.
- `docs/java-selenium-testng.md` - Maven, Selenium, and TestNG automation setup.
- `docs/playwright.md` - Playwright browser automation and MCP notes.
- `docs/appium-android.md` - Android Appium test workflow.
- `docs/docker-test-runners.md` - Dockerized test execution patterns.
- `docs/react-vite-capacitor.md` - React, Vite, Capacitor, Firebase app workflow.
- `docs/llm-wiki.md` - LLM Wiki (`Oshayr/LLM-Wiki`) plugin for autonomous knowledge management, research-on-miss, and local web UI.
- `docs/lenis-smooth-scroll.md` - Lenis smooth scroll library setup, options, React adapter, and GSAP integration.
- `docs/animate-ui.md` - Animate UI (`imskyleen/animate-ui`) animated React component library with shadcn/ui integration.
- `docs/inspira-ui.md` - Inspira UI (`unovue/inspira-ui`) Vue 3 & Nuxt 3 component library porting Aceternity & Magic UI.
- `docs/free-for-dev.md` - Free for Dev (`free-for.dev`) curated catalog of SaaS, PaaS, DBaaS, and APIs with free tiers.
- `docs/autoresearch.md` - Autoresearch (`karpathy/autoresearch`) autonomous ML research loop and experiment framework by Andrej Karpathy.
- `docs/voltagent-awesome-openclaw-skills.md` - Awesome OpenClaw Skills (`VoltAgent/awesome-openclaw-skills`) curated index of 5,000+ agent skills.
- `docs/different-ai-openwork.md` - OpenWork (`different-ai/openwork`) local-first desktop application and agent workspace.
- `docs/huggingface-upskill.md` - Hugging Face Upskill (`huggingface/upskill`) skill generation, distillation, and evaluation tool.
- `docs/skills-sh.md` - Skills.sh (`vercel-labs/skills`) open-source package manager and registry for AI agent skills.
- `docs/topoteretes-cognee.md` - Cognee (`topoteretes/cognee`) AI memory engine with hybrid vector search and semantic knowledge graphs.
- `docs/tool-evaluation.md` - Checklist for deciding whether a tool is worth keeping.
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

1. Add one note per tool in `docs/`.
2. Keep reusable prompts in `prompts/`.
3. Store repeatable formats in `templates/`.
4. Add project-specific examples only when they teach a reusable pattern.
5. Review notes after each real project use and update what actually worked.

## Agent Reach

This repo includes notes on [Agent Reach](https://github.com/Panniantong/Agent-Reach), a CLI that gives AI agents the ability to read and search Twitter, Reddit, YouTube, GitHub, and other platforms. See `docs/agent-reach.md` for setup, commands, and platform support.

## LLM Wiki

This repo includes notes on [LLM Wiki](https://github.com/Oshayr/LLM-Wiki), an autonomous knowledge base plugin featuring research-on-miss, semantic search, local `.wiki/` storage, and a Wikipedia-style web UI. See `docs/llm-wiki.md` for commands, features, and setup.

## Get Shit Done (GSD)

This repo includes notes on [Get Shit Done](https://github.com/gsd-build/get-shit-done) ([open-gsd/gsd-core](https://github.com/open-gsd/gsd-core)), a spec-driven development and meta-prompting framework to eliminate context rot for AI agents. See `docs/get-shit-done.md` for workflow phases and setup.

## Lenis Smooth Scroll

This repo includes notes on [Lenis](https://github.com/darkroomengineering/lenis), a free smooth scroll library for modern web applications. See `docs/lenis-smooth-scroll.md` for setup, API options, React adapter, and GSAP ScrollTrigger integration.

## Animate UI

This repo includes notes on [Animate UI](https://github.com/imskyleen/animate-ui), an open-source collection of animated components built with React, TypeScript, Tailwind CSS, and Motion (shadcn/ui compatible). See `docs/animate-ui.md` for setup and CLI usage.

## Inspira UI

This repo includes notes on [Inspira UI](https://github.com/unovue/inspira-ui), a free Vue 3 & Nuxt 3 component library porting Aceternity UI and Magic UI design systems to Vue. See `docs/inspira-ui.md` for setup and CLI commands.

## Free for Dev

This repo includes notes on [Free for Dev](https://free-for.dev) ([ripienaar/free-for-dev](https://github.com/ripienaar/free-for-dev)), a curated list of SaaS, PaaS, DBaaS, and APIs offering free tiers for developers. See `docs/free-for-dev.md` for categories and scope.

## Autoresearch

This repo includes notes on [Autoresearch](https://github.com/karpathy/autoresearch), Andrej Karpathy's autonomous ML research loop framework (`program.md`, `train.py`, `prepare.py`). See `docs/autoresearch.md` for workflow details.

## Awesome OpenClaw Skills

This repo includes notes on [Awesome OpenClaw Skills](https://github.com/VoltAgent/awesome-openclaw-skills), a curated catalog by VoltAgent indexing 5,000+ AI agent skills for OpenClaw. See `docs/voltagent-awesome-openclaw-skills.md` for categories and CLI usage.

## OpenWork

This repo includes notes on [OpenWork](https://github.com/different-ai/openwork), an open-source, local-first desktop application and agent workspace by Different AI. See `docs/different-ai-openwork.md` for details.

## Hugging Face Upskill

This repo includes notes on [Hugging Face Upskill](https://github.com/huggingface/upskill), a tool to distill teacher model traces into reusable agent skills (`SKILL.md`) for student models. See `docs/huggingface-upskill.md` for details.

## Skills.sh

This repo includes notes on [Skills.sh](https://skills.sh) ([vercel-labs/skills](https://github.com/vercel-labs/skills)), an open-source package manager and registry for AI agent skills across 38+ platforms. See `docs/skills-sh.md` for CLI usage.

## Cognee

This repo includes notes on [Cognee](https://github.com/topoteretes/cognee), an open-source AI memory platform by Topoteretes using ECL pipelines and semantic knowledge graphs. See `docs/topoteretes-cognee.md` for python quickstart and architecture details.

## Naming Convention

Use short lowercase filenames with hyphens:

```text
docs/graphify.md
docs/llm-wiki.md
docs/context-engineering.md
prompts/repo-review.md
templates/tool-note.md
```
