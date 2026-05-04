---
title: "Why Your AI Code Reviewer Cannot Have Write Access"
description: "Telling an AI agent 'do not modify files during review' is behavioral. Removing write access entirely is structural. The difference matters."
lead: "Telling an AI agent 'do not modify files during review' is like telling an auditor 'please do not adjust the books.' The enforcement must be structural, not behavioral."
slug: "ai-code-reviewer-write-access"
date: 2026-05-03T00:00:00+00:00
draft: false
weight: 45
toc: true
categories: ["Engineering"]
tags: ["code-review", "agents", "divisor", "quality", "architecture"]
contributors: ["Unbound Force"]
---

## The Feedback Loop That Eats Itself

When the same AI agent writes code and reviews it, the review is compromised. The agent has context about why it made each decision. It remembers the trade-offs it considered. It is, in the most literal sense, reviewing its own homework.

Anthropic discovered this when building their multi-agent systems: agents tasked with both producing and evaluating work would "confidently praise broken implementations" (as described in Yanli Liu, "Harness Engineering," *AI Advances*, Apr 2026). The agent was not lying — it genuinely believed the code was correct because it had rationalized each decision during implementation.

The obvious fix is to use a separate agent for review. But separation is not enough if the review agent can modify what it reviews. A reviewer with write access is not a reviewer — it is a second developer. The moment the reviewer can "fix" an issue instead of reporting it, two things happen: the fix bypasses the normal implementation pipeline (no tests, no spec alignment check), and the reviewer loses objectivity because it is now invested in the code it touched.

## Behavioral vs. Structural Enforcement

There are two ways to prevent a review agent from modifying code:

**Behavioral**: Add an instruction to the agent's prompt — "Do not modify files during review." This works until it does not. Models interpret instructions probabilistically. A sufficiently compelling reason ("this is a one-line fix, it would be more efficient to just change it") can override the instruction. The more capable the model, the more creative it gets at justifying exceptions.

**Structural**: Remove the tools. If the agent does not have write, edit, or bash access, it cannot modify files regardless of what it wants to do. There is no prompt injection, no reasoning chain, no "just this once" exception that can override a missing capability. The constraint is architectural, not behavioral.

In Unbound Force, [the Divisor Council's](/docs/team/the-divisor/) Guard agent operates at temperature 0.1 with explicit tool restrictions:

```
tools:
  write: false
  edit: false
  bash: false
```

The Guard can read files, search code, and reason about what it finds. But it physically cannot change a single character. Every finding must be reported as a review comment, routed back through the implementation agent, and subjected to the normal quality pipeline before it reaches the codebase.

This is not a minor implementation detail. It is the difference between a review process and a second pass at implementation.

## The Layered Feedback Stack

Structural doer/judge separation is one part of a larger feedback architecture. The review pipeline in Unbound Force runs in layers, from cheapest to most expensive:

**Layer 1 — Computational feedback.** Tests, linters, vulnerability scanners, and [Gaze](/docs/team/gaze-tester/) static analysis (CRAP scores, coverage analysis, testability classification). These checks are deterministic, fast, and cheap. If a test fails or a linter reports an error, there is no ambiguity — the code does not meet the standard. This layer runs first because there is no point spending cycles on semantic review if the code does not compile or fails its tests.

**Layer 2 — Inferential feedback.** The Divisor Council — five or more specialized review agents running in parallel. Each agent evaluates from a different perspective: the Guard checks for intent drift, the Architect reviews structural integrity, the Adversary stress-tests for security and edge cases, the Tester evaluates test quality, and the SRE assesses operational readiness. Each agent has explicit "Out of Scope" sections to prevent overlap.

This layering follows a principle identified independently by multiple teams building AI agent systems: use computational feedback first, then inferential feedback (ThoughtWorks' terminology, as described in Liu, "Harness Engineering," *AI Advances*, Apr 2026). Deterministic checks are cheaper and more reliable; inferential review is powerful but expensive. Running them in order maximizes the value of each layer.

## The Three-Iteration Cap

What happens when the reviewer finds issues? The implementation agent fixes them and resubmits. The reviewer runs again. If it finds more issues, the cycle repeats — but only three times.

The 3-iteration review cap is a pragmatic constraint: if a piece of code cannot pass review in three attempts, the problem is not a typo or a missed edge case. It is a design issue that requires human judgment. Continuing to iterate would waste compute on a problem the agents are not equipped to resolve.

This cap also prevents a subtle failure mode: the implementation agent and review agent entering an infinite loop where each fix introduces a new issue that the previous fix was avoiding. Three iterations is enough to catch genuine oversights while avoiding oscillation.

## What This Means for Your Review Pipeline

If you are building AI code review into your workflow, the enforcement model matters more than the review prompt. A sophisticated review prompt with full write access produces worse outcomes than a simple review prompt with structural separation.

The checklist:
- **Separate the agents.** The agent that writes code and the agent that reviews code must be different instances with different context.
- **Remove write access from reviewers.** Not "do not write" — actually remove the tools. No write, no edit, no bash.
- **Layer your feedback.** Run deterministic checks first, semantic review second. Do not waste inferential review on code that fails to compile.
- **Cap the iterations.** If it is not fixed in three passes, escalate to a human. The agents are circling.

If your AI review agent can edit files, it is not a reviewer. It is a second developer with a different prompt.
