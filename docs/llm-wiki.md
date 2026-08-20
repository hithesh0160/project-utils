# LLM Wiki (`Oshayr/LLM-Wiki`)

[LLM Wiki](https://github.com/Oshayr/LLM-Wiki) is an autonomous knowledge base plugin that captures research, ideas, and decisions into an interlinked local wiki (`.wiki/`) with semantic search, automatic research, and a Wikipedia-style web UI.

It is inspired by Andrej Karpathy's LLM Wiki pattern: raw sources remain immutable, the LLM maintains the structured wiki layer, and a schema governs file structure and behavior.

---

## Core Concept & Workflow

When you ask a question, LLM Wiki first checks its knowledge base. 
- **Hit:** Returns a cited answer directly from existing pages.
- **Miss ("Research-on-Miss"):** Automatically triggers research across available search/web tools, ingests findings into a new wiki page with proper citations, and answers your question without breaking context.

```text
User Question (/wiki-read)
       │
       ▼
Check Knowledge Base (.wiki/)
   ├──> Found ──> Return Cited Answer
   └──> Not Found ──> Fan-out Research ──> Ingest & Create Page ──> Return Cited Answer
```

---

## Key Features

1. **Autonomous Knowledge Capture**
   - Saves research, decisions, and ideas directly into markdown files under `.wiki/`.
   - Organizes content with structured frontmatter metadata, page types (`concept`, `idea`, `reference`, `rules`, etc.), and interlinks (`[[page-slug]]`).

2. **Research-on-Miss Pipeline**
   - Automatically executes web search and content fetching when requested knowledge isn't found in `.wiki/`.
   - Synthesizes search results into structured, cross-referenced wiki pages with source citations.

3. **Wikipedia-Style Web UI (`/wiki-serve`)**
   - Serves a web interface at `localhost:8420`.
   - **Interactive Knowledge Graph:** Uses Cytoscape.js for cluster & neighborhood visualization.
   - **Split-Pane Markdown Editor:** Live preview and AI assistance.
   - **Spaced Repetition (FSRS):** Review interface to reinforce saved knowledge.
   - **RAG-Augmented Chat:** WebSocket sidebar chat with wiki context.

4. **Maintenance & Hygiene (`/wiki-maintain`)**
   - Self-linting for broken links, orphan pages, and missing frontmatter.
   - Automatic deduplication (merges pages with >60% token overlap).
   - Dynamic freshness levels (9-tier TTL system from `live` to `permanent`).

---

## Command Reference

| Command | Purpose | Usage Example |
|---|---|---|
| `/wiki-write` | Ingest web pages, local files, or text into the wiki | `/wiki-write https://example.com/article` or `/wiki-write --batch ./docs` |
| `/wiki-read` | Query the wiki; triggers automatic research on miss | `/wiki-read "What is transformer attention?"` |
| `/wiki-serve` | Launch the local web server & knowledge graph UI | `/wiki-serve` (runs on `localhost:8420`) |
| `/wiki-maintain` | Run health checks, lint links, and deduplicate pages | `/wiki-maintain lint` or `/wiki-maintain dedup` |
| `/wiki-view` | View statistics, export knowledge base, generate graphs | `/wiki-view stats` or `/wiki-view export md` |

---

## Workspace Structure

When enabled in a project, LLM Wiki maintains:

```text
.wiki/
├── index.md           # Master index of all pages
├── pages/             # Interlinked markdown topic pages
│   ├── concept-*.md
│   └── reference-*.md
├── templates/         # Page schema templates
└── assets/            # Embedded images and visual elements
```

---

## Integration & Dependencies

- **Platform Support:** Designed as a plugin for Claude Code / LLM toolchains and MCP environments.
- **Obsidian Compatible:** `.wiki/` can be opened directly as an Obsidian vault.
- **Python Dependencies:** `fastapi`, `uvicorn`, `mcp`, `trafilatura` (optional fallback extraction), `sqlite-vec` / `numpy` (optional vector search).
