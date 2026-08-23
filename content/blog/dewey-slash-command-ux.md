---
title: "Zero-Code UX — How a Markdown File Became Dewey's Best Feature"
description: "The most impactful feature in Dewey v3.1.0 is a markdown file, not a Go package. The /dewey-store slash command teaches AI agents three interaction patterns for knowledge capture — without changing a single line of code."
lead: "The most impactful UX improvement in an agent ecosystem can be a markdown file — not a Go package. /dewey-store teaches agents three knowledge capture patterns without writing code."
slug: "dewey-slash-command-ux"
date: 2026-08-23T00:00:00+00:00
draft: false
weight: 104
toc: true
categories: ["Engineering"]
tags: ["dewey", "slash-commands", "developer-experience", "UX"]
contributors: ["Unbound Force"]
---

## The Problem Nobody Talks About

Developer tools optimize for the power user who already knows the flags. Documentation lists every parameter, every option, every edge case — and assumes the reader arrives with a mental model of how the tool works. But the most common interaction with Dewey's `store_learning` tool looks nothing like a power user session.

The real scenario: a developer finishes a 45-minute architecture discussion in Slack. The thread contains three decisions, a rejected alternative, and a gotcha about rate limiting. The developer knows this knowledge should be captured. They open their agent, paste the thread, and type something like "store this."

What happens next is the problem. The agent calls `store_learning` with a vague tag like "architecture" and a category of "context." Forty-five minutes of nuanced discussion collapses into a single learning with a generic label. The knowledge is stored, but it's unfindable — buried under a tag so broad it matches hundreds of other entries. The developer needed guided extraction, not a raw API call.

## A Markdown File, Not a Go Package

Dewey v3.1.0 shipped the `/dewey-store` slash command. The implementation is a markdown instruction file — zero lines of Go changed, no new endpoints, no schema migrations. The file teaches AI agents three progressive interaction patterns for knowledge capture, each matching a different level of user intent.

This is the core insight: in an agent ecosystem, UX improvements don't require code changes. They require better instructions. The agent already has access to `store_learning`. What it lacked was a structured protocol for deciding *how* to use it based on what the user provides.

The slash command file acts as a decision tree. When a user invokes `/dewey-store`, the agent reads the instruction file and selects one of three modes based on the input it receives. The user never sees the mode selection — they experience a conversation that adapts to their needs.

## Three Modes for Three Intentions

### Mode 1: Fully Specified

Power users who know their tag taxonomy and category system can bypass all guidance. They provide everything upfront, and the agent executes without negotiation.

```
/dewey-store --tag auth-design --category decision

We decided to use short-lived JWTs with a 15-minute expiry
and opaque refresh tokens stored server-side. RSA256 signing
with key rotation every 90 days.
```

The agent calls `store_learning` with the exact tag and category provided. No suggestions, no confirmation prompts, no friction. This mode exists because experienced users should never be slowed down by guardrails designed for newcomers.

### Mode 2: Suggested

The most common mode. The user provides content but no metadata. The agent analyzes the content and proposes tags ranked by specificity, plus a category based on content patterns.

```
/dewey-store

We tried using WebSockets for the real-time dashboard but
hit connection limit issues behind our load balancer. Switched
to SSE (Server-Sent Events) which works through HTTP/2 without
special proxy configuration. Latency increased by ~200ms but
operational complexity dropped significantly.
```

The agent responds with ranked suggestions:

```
Proposed tag (ranked by specificity):
  1. sse-migration        — matches the specific technology switch
  2. real-time-dashboard  — matches the feature context
  3. infrastructure       — broad fallback

Proposed category: decision
Rationale: content describes evaluating alternatives and selecting
an approach with trade-off analysis.

Store with these? [Y/adjust/cancel]
```

The ranking by specificity matters. Generic tags like "infrastructure" appear last because they produce the least findable results. The agent nudges toward precise tags without forcing them — the user can accept, adjust, or override.

### Mode 3: Extract

The highest-value mode for meeting transcripts, Slack threads, and design discussions. The user pastes a long conversation, and the agent breaks it into multiple distinct learnings, each with its own tag and category proposal.

```
/dewey-store --extract

[Pasted Slack thread about authentication redesign, 47 messages]
```

The agent identifies distinct knowledge units and presents them individually:

```
Extracted 4 learnings from this thread:

Learning 1/4:
  Content: "JWT refresh tokens must be opaque (not JWT themselves)
  to allow server-side revocation without maintaining a blocklist
  of signed tokens."
  Tag: jwt-refresh-design
  Category: decision
  [Store / Edit / Skip]

Learning 2/4:
  Content: "Auth0 rate-limits token refresh to 30 requests per
  minute per user. Batch operations that trigger multiple refreshes
  need a token cache layer."
  Tag: auth0-rate-limits
  Category: gotcha
  [Store / Edit / Skip]

Learning 3/4:
  Content: "Session duration was set to 8 hours based on the
  average developer workday. Shorter sessions (1-2 hours) caused
  excessive re-authentication during long debugging sessions."
  Tag: session-duration
  Category: context
  [Store / Edit / Skip]

Learning 4/4:
  Content: "The team rejected PASETO tokens in favor of JWT
  because PASETO's Go library lacks FIPS 140-2 compliance,
  which is required for FedRAMP authorization."
  Tag: paseto-rejection
  Category: decision
  [Store / Edit / Skip]
```

Each learning gets individual review. The user can store all four, edit specific ones, or skip learnings that aren't worth preserving. A 47-message Slack thread becomes four precise, tagged, findable knowledge entries — each with a category that reflects its nature.

## Before and After

**Before `/dewey-store`**: The developer pastes a Slack thread and says "remember this." The agent calls `store_learning` once with `tag: "auth"` and `category: "context"`. One monolithic entry. Unfindable in six months.

```go
// What the agent did before
store_learning({
  information: "[entire 47-message Slack thread pasted verbatim]",
  tag: "auth",
  category: "context"
})
// Result: 1 learning, generic tag, wrong category for 3 of 4 insights
```

**After `/dewey-store`**: The same thread produces four distinct learnings. Each has a specific tag that matches its content. Each has a category that reflects whether it's a decision, a gotcha, or background context. Each is individually reviewable before storage.

```go
// What the agent does now
store_learning({
  information: "JWT refresh tokens must be opaque...",
  tag: "jwt-refresh-design",
  category: "decision"
})
store_learning({
  information: "Auth0 rate-limits token refresh to 30/min...",
  tag: "auth0-rate-limits",
  category: "gotcha"
})
// ... 2 more, each reviewed and approved by the user
// Result: 4 learnings, specific tags, accurate categories
```

The difference isn't in the agent's capabilities — `store_learning` accepted tags and categories before the slash command existed. The difference is in the interaction design. The agent now follows a structured protocol that guides the user through metadata selection instead of guessing on their behalf.

## Why This Matters Beyond Dewey

Slash commands as markdown instruction files represent a pattern that applies to any agent-augmented tool. The pattern has three properties worth noting.

**Zero deployment cost.** A markdown file ships with the project. No build step, no versioning headaches, no API compatibility concerns. The instruction file can be updated independently of the tool it teaches agents to use.

**Progressive disclosure.** The three modes — fully specified, suggested, extract — match the user's level of intent. Power users skip the guidance. New users get suggestions. Users with bulk content get extraction. The same entry point serves all three without branching the UI.

**Composability.** The slash command doesn't replace `store_learning` — it wraps it with interaction design. The underlying tool remains a stable API. The instruction file adds a UX layer that can evolve independently. New modes can be added (batch import from files, periodic review of stored learnings) without changing the tool's contract.

This is the leverage point in agent ecosystems: the gap between a tool's API and a user's intent is bridged by instructions, not code. Every tool in your stack has this gap. Slash commands are one way to close it — with a markdown file and zero deploys.

## Get Started

The `/dewey-store` slash command shipped in [Dewey v3.1.0](https://github.com/unbound-force/dewey). To use it:

1. Update Dewey to v3.1.0 or later
2. Invoke `/dewey-store` in any agent session with Dewey MCP tools configured
3. Paste content — the agent selects the appropriate mode based on your input

To build your own slash commands for other tools, examine the `/dewey-store` instruction file as a template. The pattern — mode selection based on input analysis, ranked suggestions, and per-item review — transfers to any `store` or `create` operation where metadata selection is the friction point.

Read the [Dewey documentation](/docs/projects/dewey/) for the full tool reference, or explore the [source on GitHub](https://github.com/unbound-force/dewey) to see how the instruction file integrates with the MCP tool surface.
