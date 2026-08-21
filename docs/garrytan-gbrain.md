# GBrain (`garrytan/gbrain`)

[GBrain](https://github.com/garrytan/gbrain) is an open-source AI agent memory system created by Garry Tan (President & CEO of Y Combinator). It transforms plain-text Markdown notes, emails, and tweets into a structured, typed knowledge graph to give AI agents persistent long-term memory.

---

## Core Architecture

GBrain replaces flat document search with a self-wiring typed knowledge graph combined with hybrid search:

- **Self-Wiring Knowledge Graph:** Automatically extracts typed entities and relationships (`works_at`, `invested_in`, `mentions`) directly from Markdown notes without needing extra LLM calls.
- **Hybrid Search Engine:** Combines HNSW vector embeddings search with BM25 keyword search using Reciprocal Rank Fusion (RRF).
- **Database Backend:** Powered by PostgreSQL + `pgvector` for production or `PGLite` for local embedded use.
- **MCP Integration:** Native Model Context Protocol (MCP) server integration for Claude Code, Cursor, Windsurf, OpenClaw, and Hermes Agent.

```text
  Markdown Notes / Tweets / Emails
                 │
                 ▼
     Self-Wiring Graph Extraction (AST / Rules)
                 │
                 ▼
  PostgreSQL (pgvector / PGLite) + Knowledge Graph
                 │
                 ▼
     MCP Server Interface (gbrain mcp)
                 │
                 ▼
  AI Agents (Claude Code, Cursor, OpenClaw, Hermes)
```

---

## Installation & Setup

> **Warning:** Do NOT run `npm install -g gbrain` (to avoid npm package squatting). Install directly from GitHub using `bun`.

```bash
# Recommended installation via Bun directly from GitHub
bun install -g github:garrytan/gbrain

# Start MCP server for AI agents
gbrain mcp
```

---

## Key Capabilities

1. **Persistent Memory Across Sessions:** Eliminates agent memory loss between coding or research sessions.
2. **Autopilot Ingestion:** Automated cron background tasks to ingest emails, calendar meetings, and browser notes into memory.
3. **Zero-LLM Graph Construction:** Low latency graph extraction keeping memory fast and cost-effective.

---

## Resources

- **GitHub Repository:** [garrytan/gbrain](https://github.com/garrytan/gbrain)
- **License:** Open Source / MIT
