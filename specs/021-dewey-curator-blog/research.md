# Research: How Dewey Became a Knowledge Curator Blog Post

**Branch**: `021-dewey-curator-blog` | **Date**: 2026-04-11

## R1: Upstream Source Material — Dewey PR #42

**Question**: What exactly shipped in Dewey v3.0.0 that the blog post needs to describe?

**Source**: `gh pr view 42 --repo unbound-force/dewey` (verified 2026-04-11)

**Findings**:

PR #42 title: "feat: add knowledge compilation, temporal intelligence, linting, and trust tiers"

- **Scale**: 8,598 additions, 152 deletions across 39 files. Largest feature in Dewey's history.
- **Four interconnected capabilities**:
  1. **Temporal Awareness**: `store_learning` gains required `tag`, optional `category` (decision/pattern/gotcha/context/reference), `{tag}-{sequence}` identity, and `created_at` timestamp. Breaking API change with backward-compat fallback.
  2. **Knowledge Compilation**: `dewey compile` clusters learnings by tag, synthesizes current-state articles via configurable LLM with category-aware resolution (decisions=temporal merge, patterns=accumulate, gotchas=dedup). New `llm/` package with `Synthesizer` interface + `OllamaSynthesizer`.
  3. **Knowledge Linting**: `dewey lint` detects stale decisions (>30 days), uncompiled learnings, embedding gaps, and potential contradictions. `--fix` auto-repairs mechanical issues.
  4. **Contamination Separation**: Trust tiers (authored/validated/draft) via new `tier` column. `dewey promote` moves draft→validated after human review. `semantic_search_filtered` gains `tier` parameter.
- **Event Sourcing Model**: Raw learnings are the append-only event log (never deleted). Compiled articles are the materialized view (ephemeral, rebuilt from learnings).
- **Deliverables**: Schema v1→v2 migration, 3 new MCP tools (#45-47), 3 new CLI commands, 2 new slash commands, new `llm/` package (leaf dependency, zero Dewey imports), 124 new tests across 7 packages.
- **Spec artifacts**: Full Speckit spec in `specs/013-knowledge-compile/` — spec (6 user stories, 28 FRs), plan, research (13 items), quickstart (10 design decisions), 6 contracts, tasks (40 tasks, all complete).

**Decision**: Use PR #42 body as the authoritative source for all technical claims. Cross-reference with the website's already-published docs pages (updated by spec 020) for consistency.

## R2: Predecessor Blog Post — Narrative Thread

**Question**: What did the predecessor post ("The Librarian vs The Index") speculate about that this post needs to close the loop on?

**Source**: `content/blog/dewey-vs-karpathy.md` (spec 019, published)

**Findings**:

The predecessor post's "What Each Could Learn From the Other" section explicitly speculated about two features that Dewey v3.0.0 shipped:

1. **"Dewey could add autonomous synthesis."** — Quote: "A `dewey compile` command that reads indexed content and generates summary articles — turning passive indexing into active knowledge curation. Karpathy's linting concept (scan for inconsistencies, fill gaps) could make Dewey's index self-improving."
   - **Status**: Shipped. `dewey compile` does exactly this.

2. **"Dewey could add contamination separation."** — Quote: "Dewey could separate agent-generated learnings from human-authored documentation, promoting only validated content."
   - **Status**: Shipped. Trust tiers (authored/validated/draft) with `dewey promote` for tier transitions.

The predecessor post also established key framing:

- Karpathy as "AI Librarian" (active synthesis), Dewey as "Searchable Index" (passive extraction)
- The comparison table showed "Self-healing: No — indexes reflect source reality" for Dewey
- The post credited Karpathy fairly and positioned the approaches as complementary, not competing

**Decision**: The sequel post should reference these specific speculations and show they became real features. The framing shifts from "Dewey is a passive index" to "Dewey became an active curator" — but must maintain the fair, complementary tone toward Karpathy's approach.

## R3: Existing Documentation Pages (Spec 020)

**Question**: What docs pages exist that the blog post should link to instead of duplicating?

**Source**: Website content pages (updated by spec 020, now merged)

**Findings**:

1. **Knowledge getting-started guide** (`/docs/getting-started/knowledge/`):
   - Knowledge Lifecycle section with `dewey compile`, `dewey lint`, `dewey promote` CLI usage
   - Trust Tiers section with authored/validated/draft definitions
   - Storing and Retrieving Learnings section with `store_learning` parameters
   - Full CLI flag reference

2. **Dewey project page** (`/docs/projects/dewey/`):
   - Knowledge Lifecycle feature summary (compile, lint, promote)
   - "48 MCP tools across 12 categories" (updated from 40)

3. **Dewey team page** (`/docs/team/dewey/`):
   - Knowledge Management tools table (compile, lint, promote) — "New in Dewey v3.0.0"
   - Learning tools table (store_learning with required tag and optional category)

**Decision**: The blog post links to the knowledge getting-started guide for CLI details and the Dewey project page for feature overview. No CLI syntax is duplicated in the post — concepts and architecture only.

## R4: Blog Post Frontmatter Pattern

**Question**: What frontmatter format do existing blog posts use?

**Source**: Existing blog posts in `content/blog/`

**Findings**:

All existing blog posts use this frontmatter pattern:

```yaml
---
title: "Post Title"
description: "SEO description."
lead: "Brief intro shown at top."
slug: "url-slug"
date: 2026-MM-DDT00:00:00+00:00
draft: false
weight: NN
toc: true
categories: ["Engineering"]
tags: ["tag1", "tag2"]
contributors: ["Unbound Force"]
---
```

Weight values for existing posts: 50 (unleash), 55 (gaze), 60 (dewey-vs-karpathy), 65 (why-contract-coverage), 70 (dewey-knowledge-retrieval).

**Decision**: Use weight 58 to place the new post between the Karpathy comparison (60) and the unleash post (50) in sidebar ordering. Use `slug: "dewey-curator"` for a clean URL. Categories: `["Engineering"]`. Tags: `["dewey", "knowledge-management", "knowledge-compilation", "karpathy", "event-sourcing"]`.

## R5: Dual-Mode LLM Pattern

**Question**: How does the dual-mode LLM pattern work, and what makes it architecturally interesting?

**Source**: PR #42 body, `llm/` package structure

**Findings**:

Dewey uses two different LLM invocation modes:

1. **Agent-as-LLM (MCP tool calls)**: When an agent calls `dewey compile` via MCP, the agent's own LLM (Claude, GPT-4, etc.) is the synthesizer. The MCP tool returns the clustered learnings and the agent generates the compiled article. This works because the agent already has context and can reason about the learnings.

2. **Local Ollama (CLI compilation)**: When a human runs `dewey compile` from the CLI, Dewey uses a local Ollama model via the new `llm/` package (`Synthesizer` interface + `OllamaSynthesizer`). This keeps CLI compilation local and free — no API keys, no cloud calls, no data leaving the machine.

The `llm/` package is a leaf dependency with zero Dewey imports — it only knows about the `Synthesizer` interface. This makes it testable in isolation and reusable.

**Decision**: This dual-mode pattern is a genuinely novel architectural insight worth highlighting in the blog post. It solves the "who pays for LLM tokens" problem elegantly: agents already have an LLM, so use it; CLI users get a free local model.

## R6: Category-Aware Resolution

**Question**: What is category-aware resolution and why does it matter?

**Source**: PR #42 body

**Findings**:

When `dewey compile` clusters learnings by tag and synthesizes articles, it uses category-aware resolution strategies:

- **Decisions** → temporal merge (latest decision supersedes earlier ones)
- **Patterns** → accumulate (all patterns are preserved, grouped by theme)
- **Gotchas** → dedup (duplicate gotchas are merged, unique ones preserved)

This means the compiled article is not a naive concatenation — it understands the _type_ of knowledge and applies the appropriate synthesis strategy. A decision from March that was superseded by a decision in April results in the April decision being authoritative, not both decisions appearing side by side.

**Decision**: This is a concrete engineering detail that makes the "event sourcing" section more tangible. Include it as an example of how materialized views are rebuilt intelligently.

## R7: Knowledge Linting — Mechanical vs Semantic

**Question**: What is the distinction between mechanical and semantic fixes in `dewey lint`?

**Source**: Website docs (`/docs/getting-started/knowledge/`), PR #42

**Findings**:

`dewey lint` detects four quality issues:

1. **Stale decisions** (>30 days old) — semantic fix (requires human review or recompilation)
2. **Uncompiled learnings** — semantic fix (requires `dewey compile` to synthesize)
3. **Embedding gaps** — mechanical fix (auto-fixable with `--fix`, just needs embedding generation)
4. **Contradictions** — semantic fix (requires human judgment to resolve)

The `--fix` flag auto-repairs mechanical issues only. Semantic issues are reported but not auto-fixed — they require compilation or human judgment.

**Decision**: This mechanical/semantic distinction is the "self-healing index" concept from Karpathy's approach, adapted for Dewey. The blog post should explain this as Dewey adopting Karpathy's linting concept but distinguishing between what machines can fix and what requires intelligence.

## R8: Test Coverage and Engineering Rigor

**Question**: What concrete engineering metrics can the "What We Learned" section cite?

**Source**: PR #42 body, Dewey GitHub PRs

**Findings**:

- 124 new tests across 7 packages in PR #42
- Schema v1→v2 migration with backward compatibility
- `llm/` package as a leaf dependency (zero Dewey imports) — clean separation of concerns
- Category-aware resolution in compilation prompts
- 6 contracts in the spec (compile-tool, lint-tool, llm-interface, promote-tool, schema-migration, store-learning)
- 40 tasks in the spec, all completed
- Breaking API change on `store_learning` with backward-compat fallback

Earlier Dewey PRs for context:

- PR #2 (core implementation): 308 tests across 10 packages
- PR #4 (quality ratchets): CRAPload 48→15, 245+ tests

**Decision**: The "What We Learned" section should cite: (1) category-aware resolution as a prompt engineering insight, (2) the leaf dependency pattern for the `llm/` package, and (3) the 124 tests / 7 packages metric as evidence of engineering rigor. Avoid overclaiming — these are genuine insights, not marketing claims.
