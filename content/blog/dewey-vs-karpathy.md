---
title: "The Librarian vs The Index: Two Ways to Give AI Agents a Memory"
description: "Karpathy's LLM Knowledge Base compiles a wiki. Dewey indexes existing repos. Both reject RAG, both bet on markdown — but they diverge on who does the work and how far they scale."
lead: "Both reject RAG. Both bet on markdown. But they diverge on who does the work."
slug: "dewey-vs-karpathy"
date: 2026-04-07T00:00:00+00:00
draft: false
weight: 60
toc: true
categories: ["Engineering"]
tags: ["dewey", "knowledge-management", "semantic-search", "karpathy", "rag"]
contributors: ["Unbound Force"]
---

## The Problem Both Solve

Every developer using AI coding tools knows the feeling: you spend an hour building context with an agent — explaining the architecture, the naming conventions, the reasons behind a particular design decision — and then the session ends. The next session starts blank. The agent has no memory of what you discussed, no access to the decisions you made, no awareness of the other repositories in your organization.

Andrej Karpathy [described this as the "lobotomy on restart"](https://x.com/karpathy/status/2039805659525644595) problem. It is not a model limitation — it is an infrastructure problem. The model is capable of reasoning over context. It lacks the infrastructure to accumulate, organize, and retrieve that context across sessions.

Both Karpathy and the [Dewey](/docs/projects/dewey/) project address this problem. Both reject the standard RAG approach (chunk documents, embed into a vector database, retrieve by similarity). Both bet on markdown as the native format. But they take fundamentally different paths to get there.

## Karpathy's Approach: The AI Librarian

Karpathy's "LLM Knowledge Base" treats the LLM itself as a research librarian. The system works in three stages ([VentureBeat coverage](https://venturebeat.com/data/karpathy-shares-llm-knowledge-base-architecture-that-bypasses-rag-with-an)):

**1. Ingest.** Raw materials — research papers, GitHub repositories, datasets, web articles — are dumped into a `raw/` directory. Karpathy uses the Obsidian Web Clipper to convert web content to markdown, including images for vision model reference.

**2. Compile.** This is the core innovation. Instead of indexing the files, the LLM _reads_ them and _writes_ a structured wiki. It generates summaries, identifies key concepts, authors encyclopedia-style articles, and creates backlinks between related ideas. The LLM is not searching existing text — it is synthesizing new text from existing sources.

**3. Lint.** The system is not static. Karpathy describes running "health checks" where the LLM scans the wiki for inconsistencies, missing data, or new connections. The wiki heals itself over time as the LLM identifies gaps and fills them.

The result is a curated, human-readable knowledge base where every claim traces back to a specific markdown file. No embeddings, no vector database, no black box. The LLM navigates via summaries and index files — structured text, not mathematical similarity.

At a scale of roughly 100 articles and 400,000 words, this works well. The LLM's context window is large enough to hold the index and navigate to the relevant article. As Karpathy notes, "the fancy RAG infrastructure often introduces more latency and retrieval noise than it solves" at this scale.

## Dewey's Approach: The Searchable Index

Dewey takes the opposite approach. Instead of having the LLM organize knowledge, Dewey indexes existing repositories and makes them searchable through the [Model Context Protocol](https://modelcontextprotocol.io) (MCP).

**No LLM involvement in indexing.** Dewey reads markdown files, extracts blocks, computes embeddings with a local model (IBM Granite, running via Ollama), and stores everything in a SQLite database. The LLM never touches the indexing process — it consumes the search results.

**Four source types.** Dewey indexes content from:

- **Disk** — markdown files in local repositories (specs, READMEs, design documents, agent configurations)
- **GitHub** — issues, PRs, and READMEs from your organization's repositories
- **Web** — documentation crawled from toolstack websites (framework docs, stdlib references, library APIs)
- **Code** — Go source files parsed with `go/ast` to extract function signatures, CLI commands, MCP tool registrations, and package documentation

**Multi-repo by default.** When [`uf init`](/docs/getting-started/developer/#project-scaffolding-with-uf-init) configures Dewey, it scans `../` for sibling repositories and generates a multi-repo source configuration. A single Dewey instance indexes your entire organization's workspace — not just the current project.

**MCP protocol.** Dewey exposes 40 tools over MCP's stdio JSON-RPC protocol. Any MCP-compatible client (OpenCode, Claude Code, Cursor, or custom agents) can search across the entire index. Semantic search, structured graph traversal, page retrieval, block-level queries — all available as standard tool calls.

At a scale of 1,000+ pages, 10,000+ blocks, and 5+ repositories, Dewey's embedding-based retrieval becomes essential. A context window large enough to hold an index of 100 articles is not large enough to hold an index of 10,000 blocks across 5 repos. Embeddings solve the needle-in-a-haystack problem that structured navigation cannot.

## The Comparison

| Dimension          | Karpathy (AI Librarian)                 | Dewey (Searchable Index)                                 |
| ------------------ | --------------------------------------- | -------------------------------------------------------- |
| **Core metaphor**  | Librarian who reads and writes          | Index that catalogs and retrieves                        |
| **Who organizes**  | The LLM (active synthesis)              | The indexer (passive extraction)                         |
| **Search method**  | Summaries + index navigation            | Semantic search + graph traversal                        |
| **Embeddings**     | None — structured text only             | Local embeddings (IBM Granite)                           |
| **Scale target**   | ~100 articles, ~400K words              | 1,000+ pages, 10K+ blocks, 5+ repos                      |
| **Multi-repo**     | Manual (copy files to raw/)             | Automatic (scans sibling repos)                          |
| **Self-healing**   | Yes — LLM linting passes                | No — indexes reflect source reality                      |
| **Protocol**       | Custom scripts                          | MCP (Model Context Protocol)                             |
| **Multi-agent**    | Single-agent workflow                   | Multi-agent (Replicator coordination)                    |
| **Local-only**     | Yes (Obsidian + local LLM)              | Yes (SQLite + Ollama)                                    |
| **Code awareness** | No — markdown only                      | Yes — Go AST parsing (function signatures, CLI commands) |
| **LLM cost**       | High — compilation requires many tokens | Low — indexing is mechanical, no LLM tokens              |

## Where They Complement Each Other

These approaches are not competing — they solve different parts of the same problem.

**The compiled wiki becomes a Dewey source.** If you use Karpathy's approach to maintain a curated research wiki, that wiki is a folder of markdown files. Dewey indexes markdown files. Point a Dewey disk source at your compiled wiki and every agent in your swarm can search it.

**Dewey's `store_learning` is a primitive compilation step.** When the [/unleash](/docs/getting-started/common-workflows/#autonomous-pipeline-unleash) pipeline completes a task, its retrospective step stores a narrative learning in Dewey via `store_learning`. These learnings accumulate over sessions and surface via semantic search in future sessions. This is not full LLM-driven compilation — it is a single learning per session, not a synthesized wiki — but it serves the same purpose: the system remembers what it learned.

**The ideal system could use both.** Karpathy's LLM-as-librarian for active knowledge synthesis on curated research. Dewey for cross-repo semantic search on the full organizational knowledge base. The compiled wiki feeds into the searchable index.

## What Each Could Learn From the Other

**Dewey could add autonomous synthesis.** A `dewey compile` command that reads indexed content and generates summary articles — turning passive indexing into active knowledge curation. Karpathy's linting concept (scan for inconsistencies, fill gaps) could make Dewey's index self-improving.

**Dewey could add contamination separation.** Obsidian co-creator Steph Ango [suggested](https://x.com/kepano/status/2039831289533227446) keeping personal vaults clean and letting agents work in a "messy vault." Dewey could separate agent-generated learnings from human-authored documentation, promoting only validated content.

**Karpathy's approach could benefit from MCP standardization.** The "hacky collection of scripts" Karpathy describes could be exposed as MCP tools, making the compiled wiki accessible to any MCP-compatible agent — not just the specific LLM session that created it.

**Karpathy's approach could benefit from code awareness.** Dewey's Go AST parsing extracts function signatures, CLI commands, and MCP tool registrations from source code — structured knowledge that markdown compilation alone cannot capture.

## Which Should You Use?

**Use Karpathy's approach if:**

- You are a single researcher or small team
- Your knowledge base is under 100 high-signal documents
- You want active synthesis (the LLM writes new articles from raw materials)
- You value self-healing (the wiki improves itself over time)
- Your workflow is single-agent and single-project

**Use Dewey if:**

- You work across multiple repositories
- Your knowledge base exceeds what a context window can index
- You need code-level awareness (function signatures, CLI commands)
- You want multi-agent coordination (Replicator + Dewey)
- You need MCP protocol compatibility for tool-based access
- You want zero-LLM-cost indexing (mechanical extraction, no compilation tokens)

**Use both if:**

- You want LLM-driven knowledge synthesis AND cross-repo semantic search
- You maintain a curated research wiki AND an organizational codebase
- You want the librarian's synthesis AND the index's retrieval

## Getting Started

To try Dewey:

```bash
brew install unbound-force/tap/unbound-force
uf setup
```

`uf setup` installs Dewey and configures multi-repo sources automatically. See the [Quick Start guide](/docs/getting-started/quick-start/) for detailed installation, or the [knowledge retrieval guide](/docs/getting-started/knowledge/) for source configuration and web source templates.

To try Karpathy's approach, see his [X post](https://x.com/karpathy/status/2039805659525644595) for the architecture and the [VentureBeat article](https://venturebeat.com/data/karpathy-shares-llm-knowledge-base-architecture-that-bypasses-rag-with-an) for a detailed breakdown.
