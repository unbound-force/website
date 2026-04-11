---
title: "Introducing The Curator: Automated Documentation Governance for AI Agent Teams"
description: "The Curator is a Divisor agent that detects documentation gaps during code review and auto-files tracking issues — the first with bash access."
lead: "Documentation drift is the most common quality gap in AI agent workflows. The Curator catches it."
slug: "introducing-the-curator"
date: 2026-04-11T00:00:00+00:00
draft: false
weight: 56
toc: true
categories: ["Engineering"]
tags: ["divisor", "documentation", "code-review", "agents"]
contributors: ["Unbound Force"]
---

## The Problem — Documentation Drift

Every AI coding agent in a swarm can ship features. They can write code, add tests, refactor internals, and open pull requests. What they do not do — unless someone explicitly tells them to — is check whether the documentation still matches.

This is not a hypothetical gap. It is the default outcome. A new CLI command ships. The README does not mention it. A new agent persona gets added to the swarm. The team page still says the old count. A feature lands that deserves a blog post. Nobody notices, because noticing is not in anyone's job description.

The problem compounds silently. Each merged PR that changes user-facing behavior without a corresponding documentation update widens the gap between what the software does and what the documentation says it does. After enough PRs, the documentation is not wrong in any single dramatic way — it is wrong in dozens of small ways that erode trust in the entire docs surface.

AI agents are excellent at the task in front of them. They write the code they are asked to write. They review the code they are asked to review. But they do not spontaneously ask themselves: "Does the getting-started guide still work after this change?" That question requires a perspective that sits outside the scope of any individual coding or review task.

The problem is not that documentation is hard. It is that nobody is assigned to check.

## The Solution — A Dedicated Triage Agent

The Curator is the 9th Divisor persona, joining the existing five review personas (Guard, Architect, Adversary, SRE, Tester) and three content creation personas (Scribe, Herald, Envoy). It runs during every code review alongside the rest of the council.

It does one thing: detect documentation gaps and file GitHub issues to track them.

During every code review, The Curator evaluates the PR against five audit checklist items:

1. **Documentation gap detection** — Were AGENTS.md or README files updated when the PR changes user-facing behavior?
2. **Website documentation issue check** — Should an issue be filed in the website repository to track the needed documentation update?
3. **Duplicate issue check** — Does an open issue already cover this gap, or would filing a new one create noise?
4. **Blog opportunity identification** — Does this PR introduce a significant new capability that deserves a blog post?
5. **Tutorial opportunity identification** — Does this PR introduce a new workflow that users need to learn?

When The Curator detects a gap, it files a GitHub issue with the appropriate label — `docs`, `blog`, or `tutorial` — in the website repository. That issue becomes a trackable work item that the content agents can pick up.

This is the important distinction: The Curator triages. It identifies what needs documenting. It does not create the documentation itself. The Scribe handles technical documentation. The Herald writes blog posts and announcements. The Envoy handles communications. The Curator files the ticket; the content agents fill it.

The separation is deliberate. Detection and creation are different tasks that require different capabilities, different perspectives, and different tool access. A triage agent that also writes documentation would need a broader set of permissions than either task alone requires. By keeping the roles separate, each agent operates with only the access it needs.

For the full breakdown of all nine personas and how they coordinate during code review, see the [Divisor team page](/docs/team/the-divisor/).

## The Interesting Bits — Design Decisions

Three design decisions from building The Curator are worth sharing, because they reveal patterns that apply broadly to multi-agent system design.

### First Divisor Agent with Bash Access

All eight previous Divisor personas have their bash access disabled. The Guard cannot run shell commands. The Architect cannot. The Adversary — whose entire job is security auditing — cannot. This is a deliberate constraint. Code review agents that can execute arbitrary shell commands have an attack surface that is difficult to reason about.

The Curator breaks this pattern. It is the first Divisor persona with bash access enabled.

The reason is straightforward: filing GitHub issues requires running `gh issue create`, and checking for duplicates requires running `gh issue list`. These are shell commands. There is no way to file a GitHub issue from a code review context without bash access.

The restriction is minimal. The Curator's permitted operations are limited to those two `gh` CLI commands, scoped to the website repository. No other shell operations are authorized.

But a self-imposed restriction in an agent's instructions is not the same as enforcement. This is where the trust architecture becomes interesting. The Adversary — another Divisor persona that runs during the same code review — includes a "Gate Tampering" check that monitors for bash misuse across the council. The Curator's bash access is constrained not just by its own instructions, but by a peer agent that actively verifies compliance.

This is a pattern worth naming: grant the minimum capability needed, then have a peer verify that the capability is not being misused. It is the principle of least privilege with a peer audit layer — trust, but verify, with the verification built into the same system that grants the trust.

### The Bootstrap Paradox

The Curator cannot review the PR that creates it, because it does not exist until that PR merges.

This is not a thought experiment. It is what happened during PR #89 in the upstream repository. That PR added The Curator's agent definition file to the codebase. The Divisor review council discovers personas by scanning for agent definition files in the repository. During the review of PR #89, The Curator's file existed in the diff — it was part of the proposed change — but it was not yet deployed. The review council runs against the current state of the repository, not the proposed state.

The result: PR #89 was reviewed by the existing eight personas. The Curator could not participate in its own review because it did not exist as a running agent yet. After PR #89 merged, The Curator became active for all subsequent PRs. The first PR that The Curator actually reviewed was the one after its own creation.

This means The Curator's first act of documentation governance could not apply to the documentation of its own creation. The PR that added a documentation triage agent to the review council was itself not triaged for documentation gaps by that agent.

This is a genuine self-referential problem in self-modifying agent systems. Any agent that is added to a review pipeline through the same review pipeline it participates in will encounter this bootstrap boundary. The first instance is always unreviewed by itself. There is no clean solution — only the acknowledgment that the bootstrap case is handled by the existing agents, and the new agent picks up from the next review forward.

### Triage vs Creation — Why The Curator Does Not Write Docs

The Divisor council now operates across three functional layers:

- **Review** (5 personas) — Guard, Architect, Adversary, SRE, Tester evaluate code quality, security, and operational readiness
- **Triage** (1 persona) — The Curator identifies documentation gaps and files tracking issues
- **Creation** (3 personas) — Scribe, Herald, Envoy produce documentation, blog posts, and communications

The flow is directional. A PR changes user-facing behavior. The Curator detects the gap during review and files a GitHub issue. The content agents pick up that issue and create the actual content.

Why not combine triage and creation in a single agent? Because they require different tool access. Detection needs the ability to read diffs and file issues — read access and bash access for `gh` CLI commands. Creation needs the ability to write and edit files — write and edit access to produce content. Combining both in one agent would grant it all four capabilities, violating the principle of least privilege.

The Curator files the ticket. The Scribe writes the docs. The Herald writes the blog post. No single agent does everything.

This separation of concerns mirrors a pattern that works well in human organizations too. The person who notices that documentation is outdated is rarely the best person to write the update. Noticing requires breadth — awareness of what changed and what the documentation currently says. Writing requires depth — understanding the feature well enough to explain it clearly. Different skills, different roles, different agents.

## Honest Limitations

The Curator uses heuristic file-path matching, not semantic understanding. It classifies files as user-facing or internal based on where they live in the repository structure. A change to a CLI command file triggers a documentation check. A change to an internal utility file does not. This is a proxy — a reasonable one, but a proxy nonetheless.

The Curator does not read the content of changed files to determine whether documentation is actually needed. A cosmetic refactoring of a CLI command — renaming a variable, cleaning up error messages with no behavioral change — would still trigger a documentation gap warning if the file lives in a user-facing path. The Curator does not understand whether existing documentation already covers the change. It checks whether documentation files were modified in the same PR, not whether the existing docs are already accurate.

Duplicate detection is keyword-based. The Curator searches existing open issues before filing new ones, but the search uses keyword matching, not semantic similarity. Two issues describing the same documentation gap with different wording might not be recognized as duplicates.

The Curator is a first-pass filter, not an oracle. It catches the obvious gaps — the ones where user-facing code changed and no documentation file was touched. It misses the subtle ones — the cases where documentation exists but is now semantically inaccurate. This is a starting point, not a finished solution.

For how The Curator integrates into the broader review workflow, see the [common workflows guide](/docs/getting-started/common-workflows/). For the full persona breakdown — all nine agents, their roles, and their access levels — see the [Divisor team page](/docs/team/the-divisor/).
