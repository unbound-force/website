---
title: "Prompt Hardening for Compression Resilience — Engineering Agent Prompts That Survive DCP"
description: "When LLM context windows fill up, compressors summarize agent prompts. Critical constraints get dropped, causing silent quality gate bypasses. Six PRs systematically hardened all embedded prompts against Dynamic Context Protocol compression."
lead: "Your agent's instructions are being silently compressed. Critical constraints get dropped, quality gates get bypassed, and nobody notices. Here is how we engineered prompts that survive."
slug: "prompt-hardening"
date: 2026-08-23T00:00:00+00:00
draft: false
weight: 108
toc: true
categories: ["Engineering"]
tags: ["prompt-engineering", "DCP", "compression", "reliability"]
contributors: ["Unbound Force"]
---

## The Silent Failure Mode Nobody Talks About

AI agent systems run on prompts — long, detailed instructions that define behavior, enforce constraints, and gate quality. These prompts work flawlessly in short sessions. But as context windows fill up during complex tasks, something happens that most teams never notice: the LLM's context compressor kicks in and *summarizes your instructions*.

Dynamic Context Protocol (DCP) compression is a practical necessity. Context windows are finite, and when a multi-step workflow generates thousands of tokens of tool output, conversation history, and intermediate reasoning, older content gets compressed to make room. The problem is that "older content" includes your agent's system prompt — the document that tells it what rules to follow, what gates to enforce, and what behaviors are forbidden.

When a compressor summarizes "NEVER reserve files you don't intend to edit" into "manage file reservations appropriately," the constraint evaporates. The agent starts reserving files speculatively. Nobody gets an error. The behavior change is silent. You discover it three days later when a parallel worker deadlocks on a reservation that should never have existed.

## Four Fragility Patterns

We audited every embedded prompt across the Unbound Force agent swarm — coordinator prompts, worker prompts, skill instructions, command templates — and identified four structural patterns that make constraints fragile under compression.

### 1. Constraints in Trailing Sections

Compressors operate with a positional bias: opening content survives at higher rates than closing content. Constraints placed at the end of a prompt — after workflow steps, after examples, after the "main" instructions — are the first candidates for summarization or outright removal. A "Behavioral Constraints" section at the bottom of a 2,000-token prompt has a measurably lower survival rate than the same constraints placed in the first 200 tokens.

### 2. Implicit Ordering via Numbered Lists

Numbered lists suggest sequence, but compressors treat them as enumerable items that can be merged. A five-step workflow like "1. Reserve files, 2. Read existing code, 3. Implement changes, 4. Run tests, 5. Complete task" gets compressed into "Reserve files, implement changes, and complete the task." Steps 2 and 4 — reading context and running tests — vanish because the compressor treats them as elaboration on the core action.

### 3. Safety Rules in Mid-List Positions

Research on LLM attention patterns shows a "lost in the middle" effect: items at the start and end of a list receive more attention than items in the middle. Compressors exhibit the same bias. A safety constraint at position 4 of 7 in a bulleted list has the lowest survival rate of any position. If your "do not force-push" rule sits between "use conventional commits" and "write descriptive PR titles," it is the most likely to be dropped.

### 4. Parallel Do/Don't Lists

A common prompt pattern presents a "Do" list and a "Don't" list side by side. Under compression, these lists get summarized independently. The "Do" list compresses to its most prominent actions. The "Don't" list compresses to its most prominent prohibitions. The subtle interplay between them — "Do X *but* don't do Y *while* doing X" — collapses into two disconnected summaries that lose the conditional relationship.

## Six Hardening Techniques

Each fragility pattern has a corresponding structural defense. These techniques are not stylistic preferences — they are engineering decisions backed by observed compression behavior.

### Identity-First Opening with Embedded Constraints

Compressors preserve opening sentences at the highest rate. The first paragraph of a prompt is treated as the "topic sentence" that anchors all subsequent summarization. By embedding critical constraints directly into the identity statement, they inherit the positional survival advantage.

```markdown
# Before (fragile)
You are a code review agent. Review code for quality issues.

...200 lines of workflow...

## Constraints
- Never approve code that fails tests.
```

```markdown
# After (hardened)
You are the Quality Gate agent. You MUST NOT approve any code
that fails tests. You enforce quality through structured review.
```

The constraint "MUST NOT approve code that fails tests" now lives in the opening sentence. A compressor summarizing this prompt retains the identity and its embedded constraint as the anchor for the summary.

### Dedicated Critical Constraints Section (Early Position)

Constraints that govern safety, correctness, or gate integrity get their own section placed *before* the workflow — not after it. This exploits positional bias: the compressor reaches the constraints before it reaches the workflow steps, and treats them as higher-priority content.

```markdown
## Critical Constraints

- **File Reservation**: MUST reserve files BEFORE editing. MUST release after completion.
- **Review Gate**: MUST NOT call complete() without prior review approval.
- **No Force Push**: NEVER force-push to shared branches.

## Workflow

FIRST, reserve the files you will modify...
```

### RFC 2119 Keywords as Compression Signals

The keywords MUST, MUST NOT, SHALL, SHALL NOT, SHOULD, SHOULD NOT, MAY, REQUIRED, and RECOMMENDED carry semantic weight that compressors recognize. An uppercase MUST signals a non-negotiable requirement. A lowercase "should" signals a preference that can be dropped under space pressure.

This is not about shouting. It is about giving the compressor a machine-readable importance signal. When space is limited and the compressor must choose between retaining "MUST run tests before completing" and "should add helpful comments," the RFC 2119 keyword tips the decision toward retaining the constraint.

### Decision Tables Replacing Parallel Lists

Parallel do/don't lists compress independently and lose their relational structure. A decision table encodes the same information as a single coherent unit that compressors treat atomically.

```markdown
# Before (fragile parallel lists)
## Do
- Reserve files before editing
- Run tests after changes

## Don't
- Reserve files you won't edit
- Skip tests for "small" changes
```

```markdown
# After (hardened decision table)
| Situation | Action | Rationale |
|---|---|---|
| Before editing a file | Reserve ONLY that file | Prevents speculative locks |
| After any code change | Run full test suite | No change is "too small" to test |
| File editing complete | Release reservation | Prevents worker deadlocks |
```

The table survives as a unit. A compressor summarizing it retains the situation-action pairing rather than splitting actions from their conditions.

### Inline Parameters at Point-of-Use

Configuration values (timeouts, thresholds, retry counts) placed in a separate "Configuration" section get dropped when that section is compressed away. The same values placed inline — at the exact point where the action occurs — survive with their parent action.

```markdown
# Before (fragile)
## Workflow
1. Report progress at milestones.

## Configuration
- Progress milestones: 25%, 50%, 75%
- Max retries: 3
```

```markdown
# After (hardened)
## Workflow
FIRST, report progress at 25%, 50%, and 75% milestones (max 3 retries per report).
```

### Temporal Ordering Markers

Bare numbered lists lose their sequence under compression. Explicit temporal markers — FIRST, THEN, NEXT, AFTER, FINALLY — encode ordering as semantic content rather than structural formatting. A compressor that merges list items still retains "FIRST reserve files, THEN implement changes, FINALLY run tests" because the ordering words are part of the content, not the formatting.

### Redundant Constraint Placement (Defense-in-Depth)

For the highest-severity constraints — the ones where a single violation causes data loss, deadlocks, or silent quality gate bypasses — redundancy is the final defense. Place the constraint in the identity opening, in the Critical Constraints section, and inline at the point of action. If compression removes two of the three instances, one survives.

This violates DRY. That is intentional. DRY optimizes for human maintainability. Agent prompts optimize for machine reliability under degraded conditions. The trade-off is explicit: three copies of "MUST NOT force-push" cost 15 extra tokens but prevent a catastrophic failure mode.

## The Engineering Process

Each prompt hardening was not a quick find-and-replace. Every prompt went through a full OpenSpec workflow: proposal, design document, specification, task breakdown, implementation, and structural tests. Six pull requests landed across the replicator repository (PRs #54 through #59), each targeting a specific prompt scope:

1. **Coordinator prompt** (PR #54) — The orchestration layer that manages worker lifecycle
2. **Forge command prompt** (PR #55) — The swarm decomposition and dispatch logic
3. **Forge-coordination skill** (PR #56) — The skill injected into coordinators for multi-worker management
4. **Worker prompt** (PR #57) — The template given to every spawned worker agent
5. **Handoff command prompt** (PR #58) — The session handoff and context transfer logic
6. **Forge-global and always-on-guidance skills** (PR #59) — Cross-cutting skills loaded into every agent session

Each PR included structural tests that verify compression-resilience properties: critical constraints appear in the first N tokens, RFC 2119 keywords are present on safety rules, decision tables replace parallel lists, and temporal markers are present on sequential workflows. These tests catch regressions — if a future edit moves a constraint to a trailing section, the test fails.

## The Trade-Offs We Accepted

Hardened prompts are longer. The coordinator prompt grew by approximately 20% in token count. Every redundant constraint placement, every inline parameter, every temporal marker adds tokens. In a system where context window budget matters, this is a real cost.

Hardened prompts are less scannable for humans. The identity-first opening packs constraints into dense paragraphs instead of clean bulleted lists. Decision tables are harder to skim than parallel do/don't lists. A developer reading the prompt for the first time needs more effort to parse the structure.

Hardened prompts intentionally violate DRY. The same constraint appears in multiple locations within the same document. A maintainer updating the constraint must update it in every location or risk inconsistency.

We accepted all three trade-offs because these prompts are not documentation for humans. They are executable specifications for machines operating under adversarial conditions. The "adversary" is not a malicious actor — it is a well-intentioned compressor doing its job of freeing context window space. Reliability under compression outweighs aesthetics, scannability, and maintenance convenience.

## What This Means for Your Agent Systems

If you are building AI agent systems with long-running sessions, multi-step workflows, or tool-heavy interactions, your prompts are being compressed. The question is not *whether* compression happens but *whether your constraints survive it*.

Start with an audit. Take your agent's system prompt and ask: where are the safety constraints? Are they in the opening paragraph or buried at the bottom? Are they marked with RFC 2119 keywords or written as casual suggestions? Are sequential steps encoded with temporal markers or bare numbered lists?

Then test. Run your agent through a session long enough to trigger compression. Check whether the post-compression behavior matches the pre-compression behavior. If your agent starts skipping review gates, reserving files speculatively, or force-pushing to shared branches after a long session, you have a compression-resilience problem.

The six hardening techniques described here are available in the [Unbound Force replicator repository](https://github.com/unbound-force/replicator). The structural tests that verify compression resilience are in the same PRs. Fork them, adapt them, and apply them to your own agent prompts — before the compressor does it for you.
