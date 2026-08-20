# Project Utils

This repository collects reusable notes, workflows, prompts, and evaluation checklists for tools that help understand, document, and maintain software projects.

Use it for utilities such as Graphify, LLM-generated wikis, codebase maps, architecture explainers, repo analysis prompts, execution systems, and automation recipes.

## Repository Map

- `AGENTS.md` - Agent entry point with repo layout, navigation rules, and content conventions.
- `docs/agent-reach.md` - Agent Reach CLI for reading and searching Twitter, Reddit, YouTube, GitHub, and more.
- `docs/graphify.md` - Graphify setup, commands, and project workflow notes.
- `docs/get-shit-done.md` - A practical execution system for turning messy work into finished outcomes.
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

## Naming Convention

Use short lowercase filenames with hyphens:

```text
docs/graphify.md
docs/llm-wiki.md
docs/context-engineering.md
prompts/repo-review.md
templates/tool-note.md
```
