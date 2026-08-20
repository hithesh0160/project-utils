# OmniRoute

OmniRoute is an AI gateway that gives coding tools one OpenAI-compatible endpoint for many providers, models, routing strategies, fallbacks, compression, and agent protocols.

Source: https://github.com/chiencbd/omniroute

## Best For

- Routing Codex CLI, Claude Code, Cursor, Cline, Gemini CLI, or other tools through one endpoint.
- Using fallback chains when one provider is rate-limited or down.
- Experimenting with free or low-cost model providers.
- Tracking provider usage and quota.
- Exposing AI gateway controls through MCP or A2A.

## Install

Requires Node.js 22+.

```bash
npm install -g omniroute
omniroute
```

Dashboard:

```text
http://localhost:20128
```

OpenAI-compatible API base:

```text
http://localhost:20128/v1
```

## Docker

```bash
docker run -d \
  --name omniroute \
  --restart unless-stopped \
  -p 20128:20128 \
  -v omniroute-data:/app/data \
  diegosouzapw/omniroute:latest
```

## Common Commands

```bash
omniroute
omniroute setup
omniroute doctor
omniroute providers available
omniroute providers list
omniroute --port 3000
omniroute --no-open
omniroute --help
```

## Connect A Coding Tool

Use these values in any OpenAI-compatible client:

```text
Base URL: http://localhost:20128/v1
API Key:  copy from OmniRoute dashboard endpoint settings
Model:    choose from OmniRoute providers or combos
```

## MCP And A2A

```bash
omniroute --mcp
```

Useful endpoints:

```text
MCP HTTP: http://localhost:20128/api/mcp/stream
MCP SSE:  http://localhost:20128/api/mcp/sse
A2A:      http://localhost:20128/.well-known/agent.json
```

## Good Workflow

1. Install and start OmniRoute.
2. Connect at least one provider in the dashboard.
3. Create an endpoint API key.
4. Create a fallback combo.
5. Point coding tools at `http://localhost:20128/v1`.
6. Monitor usage, failures, and routing quality.

## Avoid When

- You need the simplest possible direct provider setup.
- A client requires provider-specific features that the gateway does not translate.
- You cannot safely store provider keys on the machine running OmniRoute.

## Checklist

- Dashboard is reachable.
- Endpoint key is created.
- Provider keys or OAuth connections are configured.
- Fallback strategy is documented.
- Tool configs use the local base URL.
- Secrets are not committed.

