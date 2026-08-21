# UltraContext

UltraContext is context infrastructure for AI agents. It is useful when work moves between tools such as Codex, Claude Code, OpenClaw, or custom agents and you want each tool to understand what happened elsewhere.

Source: https://github.com/ultracontext/ultracontext

## Best For

- Sharing agent session context across tools.
- Continuing work from one agent in another.
- Preserving plans, decisions, and summaries between coding sessions.
- Building context-aware agent workflows with a simple Context API.
- Reducing repeated "catch up on this project" work.

## Install

Requires Node.js 22 or newer.

```bash
npm install -g ultracontext
```

## Common Commands

```bash
ultracontext
ultracontext sync
ultracontext stop
ultracontext config
ultracontext update
```

## MCP Usage

UltraContext can expose synced context through an MCP server so compatible agents can inspect or reuse context from other sessions.

Use it when you want prompts like:

```text
Grab the last plan from my other agent session and continue from there.
```

## Context API

UltraContext also provides SDKs for direct context storage and retrieval.

```bash
npm install ultracontext
pip install ultracontext
```

Core operations:

- `create` - start a context.
- `get` - retrieve current or historical context.
- `append` - add messages or events.
- `update` - modify stored context.
- `delete` - remove context.

## Good Workflow

1. Start UltraContext sync before serious agent work.
2. Let it capture Codex and other agent sessions.
3. Use MCP or API access to retrieve prior context.
4. Add important decisions to project docs when they become stable.
5. Treat UltraContext as working memory, not the only source of truth.

## Avoid When

- The project has strict privacy rules and context capture is not approved.
- You only need a static architecture document.
- The agent workflow is short enough that context transfer is unnecessary.

## Checklist

- Node version is 22+.
- Sensitive projects are excluded or handled deliberately.
- MCP configuration is documented.
- Durable decisions are copied into repo docs.

