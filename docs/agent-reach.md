# Agent Reach

Agent Reach gives AI agents one CLI to read and search the internet — Twitter, Reddit, YouTube, GitHub, LinkedIn, Bilibili, XiaoHongShu, V2EX, Xueqiu, and more. Zero API fees for most channels.

Source: https://github.com/Panniantong/agent-reach

## Best For

- Letting AI agents read tweets, Reddit threads, YouTube transcripts, GitHub repos, and web pages.
- Searching across social platforms from one tool.
- Feeding real-time internet content into agent workflows.
- Replacing scattered scraping scripts with a single maintained CLI.
- MCP server integration so compatible agents can call it directly.

## Install

Requires Python 3.10+.

```bash
pip install agent-reach
```

Or with uv:

```bash
uv pip install agent-reach
```

Verify:

```bash
reach doctor
```

## Common Commands

```bash
# Read content from a URL
reach read <url>

# Search a platform
reach search twitter "AI agents"
reach search reddit "python async"
reach search youtube "machine learning tutorial"
reach search github "agent framework"
reach search bilibili "编程教程"
reach search xiaohongshu "tech review"

# Health check — shows which channels work
reach doctor

# Install a skill for your agent
reach skill install
```

## Supported Platforms

| Platform     | Read | Search | Notes                        |
|-------------|------|--------|------------------------------|
| Twitter/X   | ✅   | ✅     | Cookie auth or OpenCLI       |
| Reddit      | ✅   | ✅     | Works out of the box         |
| YouTube     | ✅   | ✅     | Transcripts and metadata     |
| GitHub      | ✅   | ✅     | Repos, issues, code          |
| LinkedIn    | ✅   | —      | Cookie auth                  |
| Bilibili    | ✅   | ✅     | Video info and transcripts   |
| XiaoHongShu | ✅   | ✅     | Cookie auth                  |
| V2EX        | ✅   | ✅     | Topics and replies           |
| Xueqiu      | ✅   | ✅     | Finance/stock discussions    |
| Web (any)   | ✅   | —      | Generic web page reading     |
| RSS         | ✅   | —      | Feed parsing                 |
| Exa Search  | —    | ✅     | Requires Exa API key         |

## MCP Server

Agent Reach includes an MCP server so compatible agents (Claude, Cursor, etc.) can call it as a tool:

```bash
reach mcp
```

Configure in your agent's MCP settings to expose read and search as callable tools.

## Cookie Auth

Some platforms (Twitter, XiaoHongShu, LinkedIn) need browser cookies for access. Agent Reach can extract cookies from your browser:

```bash
reach cookie-export <platform>
```

See the setup guides in the repo for platform-specific instructions:
- `agent_reach/guides/setup-twitter.md`
- `agent_reach/guides/setup-xiaohongshu.md`
- `agent_reach/guides/setup-reddit.md`

## Environment Variables

Copy `.env.example` and set any needed keys:

```bash
# Optional — only needed for specific channels
GROQ_API_KEY=       # For transcription features
EXA_API_KEY=        # For Exa search channel
```

Most channels work with zero API keys.

## Good Workflow

1. Install with `pip install agent-reach`.
2. Run `reach doctor` to see which channels are healthy.
3. Set up cookie auth for platforms that need it.
4. Use `reach read <url>` to test reading content.
5. Use `reach search <platform> "query"` to test search.
6. Connect via MCP for agent integration.
7. Run `reach doctor` periodically to catch broken channels.

## Avoid When

- You only need to read a single known URL once — `curl` or a browser is simpler.
- You need write access to platforms (posting, commenting). Agent Reach is read-only.
- You need authenticated API access with rate limit guarantees.
- The target platform is not in the supported list.

## Checklist

- Python 3.10+ is installed.
- `reach doctor` passes for the channels you need.
- Cookie auth is configured for gated platforms.
- MCP server is running if using agent integration.
- `.env` is not committed to version control.
- Channels are tested with real queries before relying on them.
