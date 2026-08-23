---
title: "uf CLI Reference"
description: "Complete command reference for the uf CLI — all command groups, subcommands, and flags for the Unbound Force toolchain."
lead: "The uf CLI is the entry point for the Unbound Force toolchain. It scaffolds projects, installs tools, runs health checks, manages configuration, launches sandboxed sessions, and provides an LLM gateway."
date: 2026-05-02T00:00:00+00:00
draft: false
weight: 10
toc: true
---

> This page reflects `uf` v0.16.0. Run `uf --help` for the latest.

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

Scaffold the Unbound Force specification framework into the current directory. Creates Speckit templates, scripts, OpenCode commands and agents, Divisor review personas, convention packs, and OpenSpec schema files. Also initializes sub-tools when available — including Dewey (knowledge retrieval), Speckit (`.specify/` configuration), and OpenCode integration.

User-owned files (templates, scripts, agents, config) are skipped if they already exist. Tool-owned files (Speckit commands, OpenSpec schema, convention packs) are updated if their content has changed.

```bash
uf init [flags]
```

| Flag | Description |
|------|-------------|
| `--divisor` | Deploy only Divisor review agents and convention packs |
| `--force` | Overwrite all existing files and re-index Dewey workspace (with `--no-embeddings` — embedding generation is deferred) |
| `--lang <string>` | Project language for convention pack selection: `go`, `python`, or `typescript` (auto-detected from `go.mod`, `pyproject.toml`, `setup.py`, `package.json`, etc. if omitted) |
| `--platform <string>` | Target AI coding platform: `opencode` (default) or `cursor`. Repeatable for dual-platform scaffolding (e.g., `--platform opencode --platform cursor`) |

If any sub-tool fails during initialization, `uf init` displays the actual error output from the failing command so you can diagnose the issue directly.

See [Multi-Platform Support](/docs/getting-started/multi-platform/) for platform comparison and dual-platform setup.

## setup

Install and configure the Unbound Force development toolchain. Detects existing versions, package managers, and platform capabilities, then installs missing tools through the appropriate method. Configures the Swarm plugin in `opencode.json` and scaffolds project files. Idempotent — safe to run multiple times.

```bash
uf setup [flags]
```

| Flag | Description |
|------|-------------|
| `--dir <string>` | Target directory for setup (default `.`) |
| `--dry-run` | Print actions without executing |
| `--yes` | Skip confirmation prompts (does **not** auto-confirm third-party curl installers — see below) |

### Install cascade

For each tool, `uf setup` tries install methods in order until one succeeds:

1. **Homebrew** — used when `brew` is available (macOS, or Linux with Linuxbrew)
2. **System package manager** — `dnf` on Fedora/RHEL, `apt` on Debian/Ubuntu (used when Homebrew is absent and the tool has a native package)
3. **Curl installer** — official install scripts from the tool's vendor (Ollama, DevPod). Requires interactive confirmation (see below)
4. **Skip** — prints a download link and continues to the next tool

The cascade is automatic. On macOS with Homebrew, all tools install via `brew`. On Fedora without Homebrew, Podman installs via `dnf`, while Ollama and DevPod use their official curl installers.

### Interactive prompts for third-party installers

When `uf setup` falls back to a curl installer (e.g., `curl -fsSL https://ollama.com/install.sh | sh`), it asks for explicit **y/n confirmation** before running the script. This is a security gate — these scripts execute third-party code that may request elevated privileges.

- **Interactive terminal**: You see a prompt like `Install Ollama via curl installer? [y/N]`
- **Non-interactive mode** (CI pipelines, `--yes` flag): Curl installers are **skipped**, not auto-confirmed. The `--yes` flag confirms package manager installs but deliberately does not confirm third-party script execution. The tool is skipped with a download link instead.

If you want to audit an install script before running it, download and inspect it first:

```bash
curl -fsSL https://ollama.com/install.sh -o install-ollama.sh
less install-ollama.sh
sh install-ollama.sh
```

If a tool installation or configuration step fails, `uf setup` displays the actual error output from the failing command so you can diagnose the issue directly.

### Podman and DevPod installation

Setup includes Podman and DevPod as part of the install cascade:

- **Step 14** — Podman installation (container runtime for sandboxes)
- **Step 15** — DevPod installation (workspace management)
- **Step 16** — DevPod Podman provider configuration

On macOS, both install via Homebrew. On Fedora/RHEL, Podman installs via `dnf` and DevPod uses the official curl installer.

See the [Quick Start](/docs/getting-started/quick-start/) for a walkthrough of `uf setup`.

## doctor

Check for required tools, version managers, scaffolded files, hero availability, Swarm plugin status, MCP server configuration, and agent/skill integrity. When a Python project is detected, additionally checks Python-specific tooling (python3, pytest, formatters, linters, type checkers). Produces a colored terminal report by default, or structured JSON for CI pipelines.

Exit code 0 when all checks pass or only warnings exist. Exit code 1 when any check fails.

```bash
uf doctor [flags]
```

| Flag | Description |
|------|-------------|
| `--dir <string>` | Target directory to check (default `.`) |
| `--format <string>` | Output format: `text` or `json` (default `text`) |

### Platform-aware install hints

When doctor detects a missing tool, the suggested install command matches your platform and configured package manager:

- **macOS / Homebrew**: `brew install <formula>`
- **Fedora / RHEL (dnf)**: `dnf install <package>` for tools available in Fedora repos (e.g., Podman)
- **No package manager**: a download URL for manual installation

This means a Fedora user running `uf doctor` sees `dnf install podman` rather than `brew install podman`. The hints reflect what will actually work on your system.

### Podman and DevPod health checks

Doctor includes checks for the containerized development toolchain:

- **Podman runtime health** — verifies Podman is installed and functional (platform-aware: checks Podman machine on macOS, rootless on Linux). Detects Docker-to-Podman shim configuration. Severity: **Fail** (exit code 1 if missing).
- **DevPod version** — verifies DevPod is installed and meets the minimum version requirement (>= 0.5.0). Severity: **Warn** (exit code 0 if missing).
- **DevPod provider configuration** — verifies a DevPod provider is registered and available. Severity: **Warn** (exit code 0 if unconfigured).

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

Key flags available on sandbox subcommands:

| Flag | Description |
|------|-------------|
| `--ide <string>` | IDE for DevPod to open: `none` (default), `vscode`, `openvscode`, `fleet`, `jupyternotebook`, `cursor` |
| `--provider <string>` | DevPod provider name (default `podman`) |

See [Sandbox Reference](/docs/reference/sandbox/) for complete flag tables and mount mode details.

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

## ollama-proxy

Start a proxy for remote Ollama model access, enabling GPU-less development workflows. Developers without local GPUs can use `uf ollama-proxy` to route embedding requests through a remote Ollama instance, providing embedding support for Dewey without requiring local GPU hardware.

```bash
uf ollama-proxy [command] [flags]
```

| Flag | Description |
|------|-------------|
| `--port <int>` | Port to listen on |
| `--embed-model <string>` | Embedding model name for Dewey reindexing |
| `--gateway-url <string>` | Gateway URL for upstream requests |

### Subcommands

| Subcommand | Description |
|------------|-------------|
| `start` | Start the ollama proxy |
| `stop` | Stop a running ollama proxy |
| `status` | Show ollama proxy status |

> **Note:** When switching from local Ollama to ollama-proxy, run `dewey reindex` to update embeddings generated by the new model endpoint.

## See Also

- [Quick Start](/docs/getting-started/quick-start/) -- Install and verify the toolchain
- [Developer Guide](/docs/getting-started/developer/) -- Daily workflow with the `uf` CLI
- [Common Workflows](/docs/getting-started/common-workflows/) -- End-to-end feature, bug fix, and review flows
