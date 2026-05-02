---
title: "uf CLI Reference"
description: "Complete command reference for the uf CLI — all command groups, subcommands, and flags for the Unbound Force toolchain."
lead: "The uf CLI is the entry point for the Unbound Force toolchain. It scaffolds projects, installs tools, runs health checks, manages configuration, launches sandboxed sessions, and provides an LLM gateway."
date: 2026-05-02T00:00:00+00:00
draft: false
weight: 10
toc: true
---

> This page reflects `uf` v0.12.0. Run `uf --help` for the latest.

## Overview

```text
uf [command] [flags]
```

The `uf` CLI (alias for `unbound-force`) manages the full Unbound Force toolchain. Each command group handles a distinct concern — from project scaffolding to containerized development sessions.

**Global flags:**

| Flag | Description |
|------|-------------|
| `-h`, `--help` | Help for any command |
| `-v`, `--version` | Print the uf version |

## init

Scaffold the Unbound Force specification framework into the current directory. Creates Speckit templates, scripts, OpenCode commands and agents, Divisor review personas, convention packs, and OpenSpec schema files.

User-owned files (templates, scripts, agents, config) are skipped if they already exist. Tool-owned files (Speckit commands, OpenSpec schema, convention packs) are updated if their content has changed.

```bash
uf init [flags]
```

| Flag | Description |
|------|-------------|
| `--divisor` | Deploy only Divisor review agents and convention packs |
| `--force` | Overwrite all existing files |
| `--lang <string>` | Project language for convention pack selection (auto-detected from `go.mod`, `package.json`, etc. if omitted) |

## setup

Install and configure the Unbound Force development toolchain. Detects existing version and package managers, installs missing tools through the appropriate manager, configures the Swarm plugin in `opencode.json`, and scaffolds project files. Idempotent — safe to run multiple times.

```bash
uf setup [flags]
```

| Flag | Description |
|------|-------------|
| `--dir <string>` | Target directory for setup (default `.`) |
| `--dry-run` | Print actions without executing |
| `--yes` | Skip confirmation prompts for curl\|bash installs |

See the [Quick Start](/docs/getting-started/quick-start/) for a walkthrough of `uf setup`.

## doctor

Check for required tools, version managers, scaffolded files, hero availability, Swarm plugin status, MCP server configuration, and agent/skill integrity. Produces a colored terminal report by default, or structured JSON for CI pipelines.

Exit code 0 when all checks pass or only warnings exist. Exit code 1 when any check fails.

```bash
uf doctor [flags]
```

| Flag | Description |
|------|-------------|
| `--dir <string>` | Target directory to check (default `.`) |
| `--format <string>` | Output format: `text` or `json` (default `text`) |

## config

Manage the unified `.uf/config.yaml` configuration file.

```bash
uf config [command] [flags]
```

### Subcommands

| Subcommand | Description |
|------------|-------------|
| `init` | Create or update `.uf/config.yaml` |
| `show` | Display effective configuration after all layers merge |
| `validate` | Validate config file against known field values |

### config init

```bash
uf config init [flags]
```

| Flag | Description |
|------|-------------|
| `--dir <string>` | Target directory (default `.`) |

### config show

```bash
uf config show [flags]
```

| Flag | Description |
|------|-------------|
| `--dir <string>` | Target directory (default `.`) |
| `--format <string>` | Output format: `text` or `json` (default `text`) |

### config validate

```bash
uf config validate [flags]
```

| Flag | Description |
|------|-------------|
| `--dir <string>` | Target directory (default `.`) |
| `--format <string>` | Output format: `text` or `json` (default `text`) |

## sandbox

Launch, manage, and extract changes from containerized OpenCode development sessions. Supports Podman (local) and Eclipse Che / Dev Spaces (CDE) backends.

```bash
uf sandbox [command] [flags]
```

### Subcommands

| Subcommand | Description |
|------------|-------------|
| `create` | Provision a persistent sandbox workspace |
| `start` | Launch or resume a sandbox |
| `stop` | Stop a sandbox (preserves persistent state) |
| `attach` | Connect to a running sandbox's TUI |
| `extract` | Extract changes from the sandbox as git patches |
| `status` | Show sandbox workspace status |
| `destroy` | Permanently delete a sandbox workspace |

## gateway

Start a local reverse proxy that serves the Anthropic Messages API. The gateway auto-detects the cloud provider from environment variables and injects host-side credentials into upstream requests.

**Supported providers:**

- Anthropic (`ANTHROPIC_API_KEY`)
- Vertex AI (`CLAUDE_CODE_USE_VERTEX=1` + `ANTHROPIC_VERTEX_PROJECT_ID`)
- AWS Bedrock (`CLAUDE_CODE_USE_BEDROCK=1`)

```bash
uf gateway [flags]
```

| Flag | Description |
|------|-------------|
| `--detach` | Run gateway in the background |
| `--port <int>` | Port to listen on (default `53147`) |
| `--provider <string>` | Provider override: `anthropic`, `vertex`, or `bedrock` (auto-detected if omitted) |

### Subcommands

| Subcommand | Description |
|------------|-------------|
| `status` | Show gateway status |
| `stop` | Stop a running gateway |

## See Also

- [Quick Start](/docs/getting-started/quick-start/) -- Install and verify the toolchain
- [Developer Guide](/docs/getting-started/developer/) -- Daily workflow with the `uf` CLI
- [Common Workflows](/docs/getting-started/common-workflows/) -- End-to-end feature, bug fix, and review flows
