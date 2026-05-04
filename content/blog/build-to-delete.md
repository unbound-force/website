---
title: "Build to Delete: Which Parts of Your AI Agent Harness Survive the Next Model Upgrade"
description: "Every harness component is a bet that the model cannot do something reliably. When the model improves, some bets expire. Here is how to tell which."
lead: "The harness you build today will need to be partially dismantled tomorrow. The question is which parts."
slug: "build-to-delete"
date: 2026-05-03T00:00:00+00:00
draft: false
weight: 44
toc: true
categories: ["Engineering"]
tags: ["agents", "architecture", "quality", "swarm"]
contributors: ["Unbound Force"]
---

## Every Component Is a Bet

Every component in an AI agent harness exists because someone believed the model could not do something reliably enough on its own. A multi-agent review council exists because one agent cannot reliably self-review. A specification pipeline exists because agents cannot reliably plan and execute in the same pass. Convention packs exist because agents do not consistently follow coding standards without explicit rules.

These are bets on model limitations. Some of them are good bets that will remain true for years. Some of them are already becoming unnecessary.

Anthropic demonstrated this concretely: moving from Opus 4.5 to Opus 4.6, they stripped their sprint decomposition mechanism entirely because the newer model handled task breakdown reliably without external scaffolding. The harness component that was essential one model version ago became dead weight the next (as described in Yanli Liu, "Harness Engineering," *AI Advances*, Apr 2026).

The article's advice is blunt: "build to delete." Design every harness component so it can be removed when the time comes. The hard part is not building the harness — it is having the discipline to prune it.

## What Persists: Organizational Intent

Some harness components do not bet on model limitations. They encode organizational intent — values, process rules, and deterministic constraints that exist independently of how capable the model is.

**The constitution and governance model.** The [constitution](/docs/getting-started/constitution/) defines principles like Autonomous Collaboration and Observable Quality. These are organizational values, not model capability compensators. A model that can write perfect code still needs to know what the organization values. The governance model (amendments via PR, semantic versioning, compliance review) is a process discipline, not a technical workaround.

**CI pipeline checks.** Test suites, linters, vulnerability scanners, type checkers — these are deterministic constraints that do not decay with model improvements. A model that writes better code still produces code that must compile, pass tests, and clear security scans. These checks exist because the organization requires them, not because the model is unreliable.

**Convention packs.** Naming rules, security policies, test standards, documentation requirements — these encode codebase properties, not model properties. A naming convention is a team decision about how code should look. It does not become unnecessary because a model gets smarter.

**Branch protection and commit gates.** Workflow safety rails exist to enforce process discipline. The Spec Commit Gate (all spec artifacts must be committed before implementation begins) is a process rule that applies regardless of model capability.

These components will likely persist across multiple generations of model improvements because they answer the question "what does this organization care about" rather than "what can this model not do."

## What May Decay: Model Compensators

Other components exist specifically because current models have limitations. As those limitations diminish, the components become candidates for removal.

**The 5+ agent Divisor Council.** Five specialized review agents running in parallel, each with exclusive ownership boundaries. This is a powerful quality mechanism, but it is also heavy. As models improve at self-review — and early data from Opus 4.7 suggests they are (Liu, "Harness Engineering," *AI Advances*, Apr 2026) — the multi-agent review structure may be overkill. A future configuration might use three agents instead of five, or a single agent with computational-only validation.

**Detailed step-by-step instructions in agent personas.** Reading lists, checklists, ordered initialization sequences — these compensate for models that need explicit scaffolding to follow complex procedures. As models require less hand-holding, these instructions become noise rather than signal.

**The 3-iteration review cap.** The current cap assumes that three review-fix cycles are needed to converge on quality. If models produce higher-quality output on the first pass, the cap might drop to two or even one.

**Dewey's current role.** A semantic knowledge layer that indexes repositories, issues, and web documentation. As models develop better native codebase understanding and longer context windows, some of Dewey's value proposition shifts. It may evolve from "essential context provider" to "nice-to-have optimization."

**Explicit ownership boundaries on review agents.** The "Out of Scope" sections that prevent overlap between Divisor agents may become unnecessary as models improve at respecting role constraints without structural enforcement.

## The Pruning Methodology

The article recommends a simple protocol: after each model upgrade, test whether quality holds without specific harness components.

1. **Run the same task with and without the component.** Pick a representative task. Execute it once with the full harness, once with a specific component disabled. Compare output quality.

2. **Measure the delta.** If quality is equivalent with and without the component, the component is a candidate for removal. If quality degrades, the component is still earning its place.

3. **Delete if quality holds.** Not "deprecate" or "make optional" — delete. Dead harness weight is worse than no harness at all because it consumes context window, adds latency, and gives a false sense of security.

Concrete experiments for Unbound Force:
- Run with 3 Divisor agents instead of 5
- Skip the Gaze feedback loop for a sprint
- Remove Dewey knowledge retrieval from agent initialization
- Simplify agent persona instructions to one paragraph each

If quality holds in any of these experiments, that component has outlived its usefulness.

## The Discipline Problem

The hardest part of "build to delete" is cultural, not technical. Every harness component was built for a reason. Someone debugged a production issue, added a check, and watched it prevent the same class of error for months. Deleting it feels irresponsible — what if the error comes back?

But keeping every component creates a different problem: harness weight. The total token cost of loading all agent context at session start — AGENTS.md, convention packs, constitution, Dewey results, agent personas — can consume a significant portion of the context window. Every line of harness configuration is a line that is not available for the actual task.

The honest answer is that we do not yet know which components of Unbound Force will prove to be permanent architecture and which will turn out to be temporary scaffolding. The system is comprehensive and heavy. Some of that weight is organizational intent that will persist. Some of it is compensation for model limitations that future models will outgrow.

The discipline is to keep testing. After your next model upgrade, run the same task with and without each harness component. Measure quality. Delete what does not contribute. The harness should shrink over time — that is a sign of progress, not regression.
