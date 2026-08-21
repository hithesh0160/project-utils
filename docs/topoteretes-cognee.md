# Cognee (`topoteretes/cognee`)

[Cognee](https://github.com/topoteretes/cognee) (developed by [Topoteretes](https://github.com/topoteretes)) is an open-source AI memory platform that provides AI agents with persistent, long-term memory using hybrid vector search and semantic knowledge graphs.

---

## Core Architecture: ECL Pipeline

Cognee uses an **Extract, Cognify, and Load (ECL)** pipeline to transform raw unstructured data into structured knowledge for AI agents:

```text
  Raw Unstructured Data (PDFs, Code, Text, APIs)
                      │
                      ▼
         1. EXTRACT (Tokenize & Chunk)
                      │
                      ▼
   2. COGNIFY (Extract Entities, Triplets & Ontologies)
                      │
                      ▼
   3. LOAD (Persist into Hybrid Vector + Graph Database)
                      │
                      ▼
  AI Agent Context Query (RAG + Graph Neighborhood Search)
```

---

## Key Features

- **Hybrid AI Memory:** Merges vector embeddings (semantic search) with property graphs (entity relationships) for accurate context retrieval.
- **Persistent Agent Memory:** Gives AI agents long-term memory across chat sessions, preventing repetitive context loading.
- **Codebase & Knowledge Ingestion:** Native loaders for Python/TypeScript codebases, markdown, PDFs, and external APIs.
- **Framework Integrations:** Official integrations monorepo (`topoteretes/cognee-integrations`) connecting Cognee to Claude Code, LangGraph, CrewAI, and AutoGen.

---

## Quick Start (Python)

```bash
pip install cognee
```

```python
import cognee
import asyncio

async def main():
    # 1. Add data to memory pipeline
    await cognee.add("AI agents require structured long-term memory to stay reliable.")
    
    # 2. Process data into knowledge graph
    await cognee.cognify()
    
    # 3. Search graph and vector memory
    results = await cognee.search("What do AI agents require?")
    print(results)

asyncio.run(main())
```

---

## Resources

- **Website:** [cognee.ai](https://cognee.ai)
- **GitHub Repository:** [topoteretes/cognee](https://github.com/topoteretes/cognee)
- **Integrations Monorepo:** [topoteretes/cognee-integrations](https://github.com/topoteretes/cognee-integrations)
- **License:** Apache 2.0 / Open Source
