---
title: "Quick Start"
description: "Get started with the Unbound Force tools — install OpenCode, Specify, and Swarm to begin using the AI agent swarm for software engineering."
lead: "Install the tools, set up the workflow, and start building with the swarm."
date: 2026-02-23T00:00:00+00:00
draft: false
weight: 20
toc: true
---

## The Stack

Unbound Force runs on three tools that form a layered stack. You can adopt them incrementally — each is useful on its own — but they work best together.

## OpenCode — The Agent Layer

OpenCode is the AI coding environment where you interact, write code, and run commands. The Unbound Force personas run inside OpenCode as agents with specialized instructions, tools, and commands.

### Install OpenCode

```bash
curl -fsSL https://opencode.ai/install | bash
```

### Why OpenCode?

OpenCode provides the runtime for AI-powered development. When you work inside OpenCode, you get access to slash commands (like `/gaze` for test quality analysis), agent personas (like Cobalt-Crush for development), and tool integrations that connect the swarm to your codebase.

## Specify — The Planning Layer

Specify is a specification pipeline that turns ideas into structured specs, plans, and tasks before a single line of code is written. It enforces a sequential workflow: specify, clarify, plan, then implement.

### Install Specify

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

Then initialize it in your project:

```bash
specify init <project> --ai opencode
```

### Why Specify?

Without structured planning, AI agents tend to build the wrong thing or drift from the original intent. Specify solves this by creating a pipeline — constitution, specification, clarification, plan, tasks — that each agent follows. The result: less rework, clearer requirements, and implementations that match what was actually requested.

## Swarm — The Coordination Layer

Swarm is an OpenCode plugin that enables multi-agent parallelism. It spawns workers, coordinates tasks via file reservations, and enables learning amongst team members.

### Install Swarm

```bash
npm install -g opencode-swarm-plugin@latest
```

Then set it up:

```bash
swarm setup
```

### Why Swarm?

A single AI agent can only do one thing at a time. Swarm lets you run multiple Unbound Force personas in parallel — Cobalt-Crush coding one feature while Gaze writes tests for another — with coordination that prevents conflicts. As the team works together, Swarm captures learnings that make each persona more effective over time.

## Putting It Together

The typical Unbound Force workflow looks like this:

1. **Plan** with Specify — turn your idea into a spec, clarify the requirements, generate a task list
2. **Execute** with OpenCode — the personas implement, test, and review the tasks
3. **Coordinate** with Swarm — run multiple personas in parallel for faster delivery

Start with OpenCode to get comfortable with the agent environment, add Specify when you want structured planning, and bring in Swarm when you are ready for multi-agent parallelism.
