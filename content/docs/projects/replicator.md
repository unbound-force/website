---
title: "Replicator"
description: "Multi-agent coordination for AI coding agents — 53 MCP tools for work tracking, orchestration, and semantic memory."
lead: "Multi-agent coordination for AI coding agents."
date: 2026-04-06T00:00:00+00:00
draft: false
weight: 40
toc: true
---

## What Replicator Does

AI coding agents are powerful individually, but coordinating multiple agents on the same codebase requires structure — work tracking, file reservations, parallel orchestration, and shared memory. Replicator provides all of this as a single Go binary with zero runtime dependencies.

Replicator exposes 53 MCP tools over stdio JSON-RPC. AI agents (OpenCode, Claude Code, or any MCP-compatible client) connect to Replicator and gain access to work item tracking, inter-agent messaging, parallel task orchestration with git worktrees, and semantic memory via Dewey integration.

## Installation

### Homebrew (macOS)

```bash
brew install unbound-force/tap/replicator
```

### Go Install

```bash
go install github.com/unbound-force/replicator/cmd/replicator@latest
```

### Binary Download

Download from [GitHub Releases](https://github.com/unbound-force/replicator/releases). Available for macOS (arm64), Linux (amd64, arm64).

## MCP Tools (53)

Replicator exposes 53 tools via the [MCP protocol](https://modelcontextprotocol.io/) across four categories:

| Category       | Tools | Purpose                                                                 |
| -------------- | ----- | ----------------------------------------------------------------------- |
| **Hive**       | 11    | Work item tracking: create, query, update, close, epics, sessions, sync |
| **Swarm Mail** | 10    | Agent messaging: send, inbox, ack, file reservations                    |
| **Swarm**      | 24    | Orchestration: decompose, spawn, worktrees, progress, review, insights  |
| **Memory**     | 8     | Dewey proxy: store/find learnings, deprecated tool stubs                |

## CLI Commands

Replicator provides 9 subcommands for setup, monitoring, and diagnostics:

```bash
# Per-repo setup (creates .hive/ directory)
replicator init

# Per-machine setup (creates ~/.config/swarm-tools/ + SQLite DB)
replicator setup

# Start MCP server (AI agents connect via stdio)
replicator serve

# List work items
replicator cells

# Check environment health
replicator doctor

# Activity summary
replicator stats

# Run preset analytics queries
replicator query cells_by_status

# Generate tool reference docs
replicator docs

# Version info
replicator version
```

## Connecting an AI Agent

Add Replicator to your `opencode.json`:

```json
{
  "mcp": {
    "replicator": {
      "type": "stdio",
      "command": "replicator",
      "args": ["serve"]
    }
  }
}
```

For Claude Code, add to `mcp_servers` in your config:

```json
{
  "mcp_servers": {
    "replicator": {
      "command": "replicator",
      "args": ["serve"]
    }
  }
}
```

## Architecture

Replicator is a single Go binary (~15MB) with <50ms startup time. It uses SQLite (WAL mode) for persistent storage and communicates with AI agents via stdio JSON-RPC.

```text
AI Agent (OpenCode, Claude)
  └── stdin/stdout → MCP Server (stdio JSON-RPC)
                       └── Tool Registry (53 tools)
                             └── Domain Logic (hive, swarm, mail)
                                   ├── SQLite (WAL mode)
                                   ├── HTTP proxy → Dewey (semantic memory)
                                   └── os/exec → Git (worktrees)
```

## Heritage

Go rewrite of [cyborg-swarm](https://github.com/unbound-force/cyborg-swarm), originally forked from [swarm-tools](https://github.com/joelhooks/swarm-tools) by [Joel Hooks](https://github.com/joelhooks).

## Links

- [GitHub Repository](https://github.com/unbound-force/replicator)
- [License: MIT](https://github.com/unbound-force/replicator/blob/main/LICENSE)
