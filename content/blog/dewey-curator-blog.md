---
title: "How Dewey Became a Knowledge Curator: From Passive Index to Active Synthesis"
description: "Dewey v3.0.0 shipped knowledge compilation, trust tiers, and linting — turning speculative ideas from the Karpathy comparison into real features."
lead: "We compared approaches. Then we built the features. Here is what happened."
slug: "dewey-curator"
date: 2026-04-11T00:00:00+00:00
draft: false
weight: 58
toc: true
categories: ["Engineering"]
tags:
  [
    "dewey",
    "knowledge-management",
    "knowledge-compilation",
    "karpathy",
    "event-sourcing",
  ]
contributors: ["Unbound Force"]
---

## The Problem — Learnings That Never Compound

AI coding agents learn things. They discover that a particular test helper expects a specific argument order. They figure out that the CI pipeline needs a cache warm-up step before integration tests run. They learn that a naming convention exists, and why it exists, and what breaks when you ignore it.

Then the session ends. The next agent starts blank.

Dewey's `store_learning` tool addressed half of this problem. Agents could store a learning at the end of a session and future agents could retrieve it via semantic search. Learnings accumulated across sessions — hundreds of them, across dozens of repositories.

But accumulation is not the same as synthesis. A learning stored in March might say "use the v1 API endpoint." A learning stored in April might say "the v1 endpoint is deprecated — use v2." Both exist in the index. Both surface in search results. The agent consuming those results has no way to know which one is current without reading both, reasoning about dates, and hoping there is not a third learning that contradicts both.

Contradictions between sessions are invisible. There is no mechanism to distinguish a reviewed architectural decision from an unreviewed guess an agent made at 2 AM during an autonomous pipeline run. Learnings accumulate, but they never compound into something more than the sum of their parts.

In [The Librarian vs The Index](/blog/dewey-vs-karpathy/), we compared Dewey's passive indexing approach with Karpathy's active synthesis model. The comparison table listed "Self-healing: No — indexes reflect source reality" for Dewey. That post speculated about two features that could change this: a `dewey compile` command that would turn passive indexing into active knowledge curation, and contamination separation that would distinguish agent-generated content from human-authored documentation.

That post was a comparison. This one is an engineering story. Those speculative ideas became real features in Dewey v3.0.0 — the largest single feature addition in Dewey's history, spanning 8,598 additions across 39 files. Here is what happened.

## From Karpathy to Code — Three Key Ideas

The Karpathy comparison was not just an academic exercise. Three ideas from that comparison directly influenced what got built.

**Autonomous synthesis became `dewey compile`.** Karpathy's core innovation is having the LLM _read_ raw materials and _write_ structured articles — not indexing, but synthesizing. Dewey adapted this for a multi-agent context: `dewey compile` clusters learnings by tag, then synthesizes a current-state article that resolves contradictions, deduplicates redundancies, and produces a single coherent view of what the system knows about a topic. The raw learnings remain intact. The compiled article is a synthesis layer on top.

**Contamination separation became trust tiers.** Steph Ango's suggestion (referenced in the predecessor post) about keeping personal vaults clean while agents work in a "messy vault" pointed to a real problem: when agents and humans both contribute knowledge, how do you know which content has been reviewed? Dewey v3.0.0 introduces three trust tiers — _authored_ (human-written), _validated_ (machine-generated, human-reviewed), and _draft_ (machine-generated, not yet reviewed). The `dewey promote` command moves content between tiers after review, and search can filter by tier.

**Self-healing quality became `dewey lint`.** Karpathy described running "health checks" where the LLM scans the wiki for inconsistencies and fills gaps. Dewey adapted this into a concrete linting system that detects four categories of quality issues and distinguishes between problems that machines can fix automatically and problems that require human judgment.

Credit where it is due: Karpathy's approach excels in domains where it was designed to work — small, curated datasets for personal research, where a single researcher actively curates a wiki of roughly 100 high-signal articles. The ideas that inspired Dewey's features are genuine contributions to how we think about knowledge management for AI systems. The adaptation was not "make it better" — it was "make it work for multi-agent, multi-repo workflows at a different scale."

## The Event Sourcing Insight

The architectural insight that unlocked the design came from an unexpected direction: event sourcing.

In event sourcing, you never modify or delete events. The raw event log is the source of truth — append-only, immutable. When you need a current-state view, you build a materialized view by replaying the events through a projection function. If the projection logic changes, you rebuild the view from the same events.

Dewey's knowledge system follows this pattern exactly. Raw learnings are the append-only event log. They are never deleted, never modified after storage. Each learning carries a timestamp, a tag, and a category (decision, pattern, gotcha, context, or reference). Compiled articles are the materialized view — ephemeral, rebuilt from learnings whenever `dewey compile` runs.

This matters because of category-aware resolution. Not all knowledge merges the same way. When `dewey compile` synthesizes an article from clustered learnings, it applies different strategies based on the category of each learning:

- **Decisions** use temporal merge — the latest decision supersedes earlier ones. A decision from March that was overridden in April results in the April decision being authoritative, not both appearing side by side.
- **Patterns** accumulate — all observed patterns are preserved and grouped by theme, because patterns do not supersede each other.
- **Gotchas** deduplicate — if three sessions discovered the same gotcha independently, the compiled article contains it once, not three times.

The second architectural pattern is the dual-mode LLM design. When an agent calls `dewey compile` via MCP, the agent's own LLM performs the synthesis. The agent already has a language model in context — why invoke another one? But when a human runs `dewey compile` from the CLI, there is no agent LLM available. For this path, Dewey uses a local Ollama model via the `llm/` package, keeping CLI compilation local and free — no API keys, no cloud calls, no data leaving the machine. This solves the "who pays for tokens" problem: agents already have an LLM, so use it; CLI users get a free local model.

## Knowledge Linting — The Self-Healing Index

The predecessor post's comparison table showed "Self-healing: No" for Dewey. That line is no longer accurate.

`dewey lint` performs four quality checks against the knowledge base:

1. **Stale decisions** — decisions older than 30 days that have not been reaffirmed or superseded. A decision made months ago might still be correct, but it might also reflect a context that no longer exists.
2. **Uncompiled learnings** — learnings that have accumulated since the last compilation pass. These are raw events that have not been synthesized into a current-state view.
3. **Embedding gaps** — stored content that lacks computed embeddings, making it invisible to semantic search.
4. **Contradictions** — learnings within the same tag that assert conflicting things.

The design distinguishes between mechanical fixes and semantic fixes. Embedding gaps are mechanical — the content exists, it just needs an embedding computed. The `--fix` flag handles these automatically: re-embed, re-index, done. Stale decisions, uncompiled learnings, and contradictions are semantic — they require either compilation (to synthesize a current-state view) or human judgment (to resolve a genuine disagreement). `dewey lint` reports these issues but does not auto-fix them.

Trust tiers interact with linting. A compiled article starts as _draft_ — it was machine-generated and has not been reviewed. When a human reviews the article and confirms its accuracy, `dewey promote` moves it to _validated_. Human-authored content enters as _authored_ directly. Search queries can filter by tier, so an agent that needs high-confidence information can restrict results to _authored_ and _validated_ content, excluding unreviewed _draft_ articles.

This is Dewey's version of Karpathy's "self-healing" concept, adapted for a context where machines and humans contribute knowledge at different levels of reliability. The system does not just index — it maintains quality over time, distinguishing between what it can fix mechanically and what needs intelligence.

## What We Learned Building It

Dewey v3.0.0's knowledge compilation was the largest single feature in the project's history — 124 new tests across 7 packages, a schema migration from v1 to v2 with backward compatibility, and three new CLI commands. Two engineering insights stood out.

**Category-aware resolution lives in prompts, not code.** The decision to use temporal merge for decisions, accumulation for patterns, and deduplication for gotchas is not implemented in Go code — it is encoded in the synthesis prompts that drive compilation. This means the resolution strategy can be tuned, extended, or corrected without recompiling the binary. It is a prompt engineering insight: when the "business logic" is about how to reason over text, the right place for that logic is in the prompt that instructs the reasoner, not in the code that invokes it.

**The `llm/` package is a leaf dependency.** The new package that wraps Ollama for CLI compilation has zero imports from the rest of the Dewey codebase. It knows about the `Synthesizer` interface and nothing else. This made it testable in complete isolation — the package's tests do not need a running Dewey server, a database, or any MCP infrastructure. For a feature that touches the core of the knowledge system, this level of decoupling was essential for maintaining test reliability. It is a reminder that the most important dependency is the one you do not take.

We are not overclaiming the maturity of these features. Knowledge compilation is new. The resolution strategies are the first iteration. Trust tiers work, but the workflow of reviewing and promoting content is still being refined through real usage across the Unbound Force swarm. What shipped is a foundation — a sound architecture that can evolve, not a finished product.

If you want to try these features, the [knowledge getting-started guide](/docs/getting-started/knowledge/) covers the CLI commands and configuration for compilation, linting, and trust tiers. The [Dewey project page](/docs/projects/dewey/) has the full feature overview, including the 48 MCP tools available across 12 categories.
