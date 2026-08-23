---
title: "Development Environment Setup"
description: "Set up a DevPod-based containerized development environment for AI agent sessions with Podman, mount modes, and IDE integration."
lead: "Walk through DevPod sandbox setup from prerequisites to your first AI agent session."
date: 2026-05-02T00:00:00+00:00
draft: false
weight: 25
toc: true
---

## Why a Containerized Dev Environment?

AI agents edit files, run commands, and install packages. Giving them direct access to your host filesystem means a bad suggestion is one Enter key away from modifying files you didn't intend to change. The `uf sandbox` command solves this by running agent sessions inside rootless Podman containers — your project is mounted with controlled access, resource limits are enforced, and credentials are handled safely. You get the full power of the AI agent swarm without risking your host environment.

This guide walks you through setting up the containerized development environment from scratch. If you already have `uf setup` completed and just need the command reference, see the [Sandbox Reference](/docs/reference/sandbox/).

## Prerequisites

### Install Podman and DevPod

Both Podman and DevPod are installed automatically by `uf setup`:

- **Step 14**: Installs Podman (the container runtime)
- **Step 15**: Installs DevPod (the workspace manager)
- **Step 16**: Configures the DevPod Podman provider

```bash
uf setup
```

On **macOS**, both are installed via Homebrew. On **Fedora/RHEL**, Podman is installed via `dnf` (it's in the default repos) and DevPod uses its official curl installer with an interactive confirmation prompt.

### Verify with `uf doctor`

After setup, confirm everything is healthy:

```bash
uf doctor
```

Doctor runs platform-aware checks for the sandbox infrastructure:

| Check | What It Verifies | Severity |
|-------|-----------------|----------|
| **Podman runtime** | Podman is installed, running, and healthy (on macOS, the Podman machine is started) | Fail |
| **Docker-to-Podman shim** | The Docker compatibility shim is configured so tools expecting `docker` use Podman | Fail |
| **DevPod version** | DevPod >= 0.5.0 is installed | Warn |
| **DevPod provider** | The Podman provider is registered and configured | Warn |

Podman checks are **Fail** severity — the sandbox cannot function without a working container runtime. DevPod checks are **Warn** severity — ephemeral sandbox sessions work without DevPod, but persistent workspaces require it.

## Initialize the Devcontainer

Before creating a workspace, generate the devcontainer configuration for your project:

```bash
uf sandbox init
```

This scaffolds a `.devcontainer/devcontainer.json` file tailored to your operating system. The file is **gitignored** — each developer generates their own because the configuration differs by platform.

### Why Per-Developer Configuration?

macOS and Linux require different Podman user namespace flags to get correct file ownership inside the container:

- **macOS**: Uses `--userns=keep-id:uid=1000,gid=1000` — the Podman VM needs explicit UID/GID values to map correctly through the virtiofs layer
- **Linux**: Uses `--userns=keep-id` without explicit UID/GID — avoids subuid range conflicts that can occur on container restart when explicit mappings are used

Committing a shared devcontainer would work on one platform and break on the other. Each developer generates their own with `uf sandbox init` and gets the right flags automatically.

> **Note**: `uf init` scaffolds project configuration (agents, convention packs, commands) but does **not** generate the devcontainer. Run `uf sandbox init` separately for sandbox support.

## Workspace Lifecycle

The sandbox supports two workspace types: **ephemeral containers** (the default) and **persistent workspaces**. Understanding the difference helps you choose the right one for your session.

### Ephemeral Containers

Start a quick session that is removed when you stop it:

```bash
uf sandbox start
```

The container is created, your project is mounted, and the OpenCode server starts automatically. When you run `uf sandbox stop`, the container is removed — any uncommitted changes inside the container are lost unless you extract them first.

**Best for**: Quick agent sessions, one-off tasks, experimentation.

### Persistent Workspaces

For longer development sessions, create a persistent workspace that survives stop/start cycles:

```bash
uf sandbox create
```

This provisions a workspace backed by named Podman volumes. The container's filesystem state is retained across restarts — you can stop work, come back later, and resume exactly where you left off.

```bash
uf sandbox start     # resume the workspace
uf sandbox stop      # pause (state preserved)
uf sandbox destroy   # permanently delete the workspace
```

**Best for**: Multi-day feature work, sessions that span multiple sittings, projects with complex in-container state.

### What Happens on Start

Whether ephemeral or persistent, the sandbox follows the same startup sequence:

1. The container is created (or resumed) with your project mounted
2. The OpenCode server auto-starts via a `postStartCommand` in the devcontainer configuration
3. A health check runs with exponential backoff (500ms to 5s intervals, 60s timeout) to confirm the server is ready
4. The TUI attaches once the server is healthy

Persistent workspaces also maintain **bidirectional git sync** between the host and container — the host's git state is synced into the container on start, and changes flow back via `uf sandbox extract`.

For the full command reference including all flags, see the [Sandbox Reference](/docs/reference/sandbox/).

## Mount Modes

The sandbox supports two mount modes that control how your project files are shared with the container. Choose based on how much isolation you want.

### Isolated Mode (Default)

```bash
uf sandbox start --mode isolated
```

Your project is mounted **read-only**. The agent can read every file, but writes go to a container overlay. When the agent finishes, use `uf sandbox extract` to generate a git patch and review the changes before applying them to your host repo.

**When to use**: Most development sessions. You get the strongest isolation — the agent cannot accidentally modify or delete host files. Review changes selectively before they touch your working tree.

### Direct Mode

```bash
uf sandbox start --mode direct
```

Your project is mounted **read-write**. Changes the agent makes inside the container are immediately visible on the host filesystem.

**When to use**: When you need real-time file synchronization — for example, running tests on the host while the agent edits code in the container. Provides less isolation since the agent can modify any mounted file directly.

## IDE Integration

The `--ide` flag tells DevPod which IDE to open after the container is ready and the health check passes. Supported values:

| Value | IDE |
|-------|-----|
| `none` | No IDE opened (default) |
| `vscode` | Visual Studio Code |
| `openvscode` | OpenVSCode Server (browser-based) |
| `fleet` | JetBrains Fleet |
| `jupyternotebook` | Jupyter Notebook |
| `cursor` | Cursor |

```bash
uf sandbox start --ide vscode
uf sandbox create --ide cursor
```

### Resolution Chain

The IDE selection follows a priority chain — the first match wins:

| Priority | Source | Example |
|----------|--------|---------|
| 1 | CLI flag | `--ide vscode` |
| 2 | Environment variable | `UF_SANDBOX_IDE=cursor` |
| 3 | Config file | `sandbox.ide: vscode` in `.uf/config.yaml` |
| 4 | Default | `none` |

Set the environment variable or config file value to avoid passing `--ide` on every command. See the [CLI Reference](/docs/reference/cli/) for the full flag documentation.

## Cloud LLM Provider Integration

When you use a cloud LLM provider (Vertex AI, Bedrock, or Anthropic), the sandbox automatically starts the [Gateway](/docs/reference/gateway/) to handle credential isolation.

### How It Works

Instead of forwarding cloud credentials into the container, the sandbox starts `uf gateway` on the host as a local reverse proxy. The container receives two environment variables:

- `ANTHROPIC_BASE_URL` — points to the gateway on the host
- `ANTHROPIC_AUTH_TOKEN` — a placeholder token accepted by the gateway

The agent's API requests go to the gateway, which injects real credentials on the host side and forwards the request to the cloud provider. **No cloud credentials enter the container.** The gateway handles token refresh, provider translation, and authentication transparently.

### Direct API Keys

If you use direct API keys (`ANTHROPIC_API_KEY`, `OPENAI_API_KEY`, `GEMINI_API_KEY`, `OPENROUTER_API_KEY`), these are forwarded directly to the container — the gateway is not needed for direct key-based authentication.

For the full credential isolation design, see the [Gateway](/docs/reference/gateway/) reference.

## Platform-Specific Notes

### macOS

**Podman machine must be running.** Podman on macOS runs containers inside a Linux VM. Start it before using the sandbox:

```bash
podman machine start
```

**UID mapping uses explicit values.** The devcontainer generated by `uf sandbox init` on macOS uses `--userns=keep-id:uid=1000,gid=1000` because the Podman VM's virtiofs layer requires explicit UID/GID values to map file ownership correctly between the VM and the host.

### Linux

**Rootless containers run natively.** No VM is needed — Podman runs containers directly using user namespaces.

**SELinux is auto-detected.** On systems with SELinux enforcing, the sandbox automatically applies `:Z` volume labels to mounted directories. No manual configuration is needed.

**subuid/subgid must be configured.** Rootless Podman requires entries in `/etc/subuid` and `/etc/subgid` for your user. Most distributions configure this automatically when Podman is installed. If you see permission errors, verify your user has entries:

```bash
grep $USER /etc/subuid
grep $USER /etc/subgid
```

Each line should show your username with a subordinate UID/GID range (e.g., `youruser:100000:65536`).

## Troubleshooting

### Files Appear as root:nobody

If files inside the sandbox are owned by `root:nobody`, the UID mapping is not working correctly. Common causes and fixes:

1. **Podman version too old** — Upgrade to Podman >= 4.3 (required for `keep-id` support)
2. **macOS Podman machine needs restart** — Run `podman machine stop && podman machine start`, then try again. If the issue persists, use the `--uidmap` escape hatch: `uf sandbox start --uidmap`
3. **Linux subuid/subgid missing** — Check that `/etc/subuid` and `/etc/subgid` have entries for your user (see the Linux section above)

### General Diagnostics with `uf doctor`

When something isn't working, `uf doctor` is the first tool to reach for. It checks Podman runtime health, Docker-to-Podman shim detection, DevPod version and provider configuration, and provides copy-pasteable commands to fix each issue.

```bash
uf doctor
```

### DevPod Provider Failures

If `devpod up` fails, the issue is usually with the DevPod provider configuration rather than Podman itself. The sandbox uses a registered DevPod provider — it does not require `podman` to be in your `PATH`. Run `uf doctor` to check provider health, or re-run `uf setup` to reconfigure the provider.

## Next Steps

- **[Sandbox Reference](/docs/reference/sandbox/)** — Full command reference with all flags and options
- **[CLI Reference](/docs/reference/cli/)** — Complete `uf` command documentation
- **[Gateway](/docs/reference/gateway/)** — LLM reverse proxy and credential isolation details
- **[Developer Guide](/docs/getting-started/developer/)** — Daily workflow with the `uf` CLI
- **[Common Workflows](/docs/getting-started/common-workflows/)** — End-to-end feature and review flows
