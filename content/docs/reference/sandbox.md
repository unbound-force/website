---
title: "Sandbox"
description: "Containerized AI agent sessions — Podman backends, mount modes, UID mapping, persistent workspaces, and security model."
lead: "The uf sandbox command launches containerized OpenCode sessions. Your project runs inside an isolated Podman container with controlled file access, resource limits, and API key forwarding — no manual container setup required."
date: 2026-05-02T00:00:00+00:00
draft: false
weight: 25
toc: true
---

## Overview

`uf sandbox` provides containerized development sessions for AI agents. Instead of running OpenCode directly on your host, the sandbox creates a Podman container with your project mounted, OpenCode installed, and API keys forwarded. The agent works inside the container where file changes are isolated (or directly applied, depending on mount mode).

**Prerequisites**: Podman must be installed. On macOS, a Podman machine must be running (`podman machine start`).

## Session Management

### sandbox start

Launch a containerized OpenCode session. If a persistent workspace exists (from `uf sandbox create`), resumes it. Otherwise, starts an ephemeral container.

```bash
uf sandbox start                    # ephemeral, isolated mode
uf sandbox start --mode direct      # read-write mounts
uf sandbox start --detach           # start without attaching
uf sandbox start --image my-image   # custom container image
```

| Flag | Description |
|------|-------------|
| `--mode <string>` | Mount mode: `isolated` (read-only, default) or `direct` (read-write) |
| `--detach` | Start container without attaching the TUI |
| `--image <string>` | Container image (default from `UF_SANDBOX_IMAGE` or `quay.io/unbound-force/opencode-dev:latest`) |
| `--memory <string>` | Container memory limit (default `8g`) |
| `--cpus <string>` | Container CPU limit (default `4`) |
| `--backend <string>` | Backend: `auto`, `podman`, or `che` (default `auto`) |
| `--uidmap` | Use explicit UID/GID mapping (macOS escape hatch) |
| `--no-parent` | Mount only the project directory (disable parent directory mount) |

### sandbox stop

Stop the running sandbox. For persistent workspaces, state is preserved. For ephemeral containers, the container is removed.

```bash
uf sandbox stop
```

### sandbox attach

Connect to a running sandbox's OpenCode TUI. Requires the sandbox to be running.

```bash
uf sandbox attach
```

### sandbox extract

Generate a patch from the container's git history and apply it to the host repo. Uses `git format-patch` / `git am` for commit-preserving round-trip extraction.

```bash
uf sandbox extract          # interactive confirmation
uf sandbox extract --yes    # skip confirmation
```

| Flag | Description |
|------|-------------|
| `--yes` | Skip the confirmation prompt |

### sandbox status

Display the current sandbox state: workspace name, backend, image, state, project directory, server URL, demo endpoints, and uptime.

```bash
uf sandbox status
```

## Mount Modes

The sandbox supports two mount modes that control how your project files are shared with the container:

### Isolated Mode (default)

```bash
uf sandbox start --mode isolated
```

Your project is mounted **read-only** inside the container. The agent can read your code but changes are written to a container overlay. To get changes back to the host, use `uf sandbox extract` which generates git patches.

**When to use**: Default for most development. Provides the strongest isolation — the agent cannot accidentally modify or delete host files. Use `extract` to review and apply changes selectively.

### Direct Mode

```bash
uf sandbox start --mode direct
```

Your project is mounted **read-write**. Changes the agent makes inside the container are immediately visible on the host filesystem.

**When to use**: When you need real-time file synchronization (e.g., running tests on the host while the agent edits in the container). Provides less isolation — the agent can modify any mounted file directly.

## UID Mapping

Podman's rootless containers run as a non-root user, but file ownership can mismatch between the container and host. The sandbox handles this automatically using `--userns=keep-id:uid=1000,gid=1000`, which maps the container's user (UID 1000) to your host user.

**Requirements**: Podman >= 4.3 (for `keep-id` support).

### macOS Setup

On macOS, the Podman machine must be configured for virtiofs UID mapping. If you see files appearing as `root:nobody` inside the container, the machine's volume driver needs updating.

Use the `--uidmap` flag as an escape hatch when the Podman machine's virtiofs does not support `--userns=keep-id`:

```bash
uf sandbox start --uidmap
```

This uses explicit UID/GID mapping instead of the namespace-based approach.

### Troubleshooting: Files Appear as root:nobody

If files inside the sandbox appear owned by `root:nobody`, the UID mapping is not working. Common causes:

1. **Podman version too old**: Upgrade to Podman >= 4.3
2. **macOS Podman machine**: Restart the machine or use `--uidmap`
3. **Linux**: Check that `/etc/subuid` and `/etc/subgid` have entries for your user

## Persistent Workspaces

By default, `uf sandbox start` creates ephemeral containers that are removed on `stop`. For long-running development sessions, create a persistent workspace that survives stop/start cycles.

### sandbox create

Provision a persistent workspace backed by named Podman volumes. The workspace retains the container's filesystem state across restarts.

```bash
uf sandbox create                              # auto-named
uf sandbox create --name my-project-sandbox    # custom name
uf sandbox create --demo-ports 3000,8080       # expose demo ports
```

| Flag | Description |
|------|-------------|
| `--name <string>` | Workspace name override (default `uf-sandbox-<project-name>`) |
| `--backend <string>` | Backend: `auto`, `podman`, or `che` (default `auto`) |
| `--demo-ports <ints>` | Additional ports to expose for demos (comma-separated) |
| `--image <string>` | Container image (Podman only) |
| `--memory <string>` | Memory limit (default `8g`) |
| `--cpus <string>` | CPU limit (default `4`) |
| `--detach` | Start without attaching the TUI |
| `--uidmap` | Use explicit UID/GID mapping |

### sandbox destroy

Permanently delete a persistent workspace and all associated state (named volumes, CDE workspace).

```bash
uf sandbox destroy          # interactive confirmation
uf sandbox destroy --yes    # skip confirmation
uf sandbox destroy --force  # destroy even if running
```

| Flag | Description |
|------|-------------|
| `--yes` | Skip the confirmation prompt |
| `--force` | Force destroy even if the workspace is running |

### Workspace Detection

Once a persistent workspace exists, `start`, `stop`, and `extract` automatically detect and use it. You don't need to pass any extra flags — the commands recognize that a workspace exists for the current project and operate on it.

### Bidirectional Git Sync

Persistent workspaces maintain bidirectional git synchronization between the host and container. When the workspace starts, it syncs the host's git state into the container. When you extract changes, they flow back via `git format-patch` / `git am`.

### Demo Endpoint Port Forwarding

Ports specified with `--demo-ports` during `create` are forwarded from the container to the host. Use this for web applications or API servers that need to be accessed from outside the container. `uf sandbox status` displays the active demo endpoints.

## CDE Backend (Experimental)

> **Experimental**: The Eclipse Che / Dev Spaces backend has API coverage and mocked tests, but has not been validated end-to-end with a production Eclipse Che instance. Use with caution.

The sandbox supports an alternative backend for Eclipse Che / Dev Spaces cloud development environments (CDEs).

### Backend Selection

```bash
uf sandbox create --backend che     # force Che backend
uf sandbox create --backend podman  # force Podman backend
```

Backend resolution follows a priority chain:

| Priority | Source | Example |
|----------|--------|---------|
| 1 | CLI flag | `--backend che` |
| 2 | Environment variable | `UF_SANDBOX_BACKEND=che` |
| 3 | Config file | `sandbox.backend: che` in `.uf/config.yaml` |
| 4 | Auto-detect | Podman (default) |

## Security Model

The sandbox provides several isolation layers:

| Property | Description |
|----------|-------------|
| **Rootless Podman** | Container runs without root privileges on the host |
| **Read-only mounts** | Isolated mode mounts the project read-only (default) |
| **No push credentials** | Git push credentials are never forwarded to the container |
| **Resource limits** | Memory (default 8g) and CPU (default 4) limits prevent runaway processes |
| **SELinux** | Auto-detects SELinux and applies `:Z` volume labels on enforcing systems |
| **Non-root user** | Container runs as UID 1000 (non-root) inside the namespace |

### API Key Forwarding

The sandbox forwards LLM API keys to the container so the agent can make API calls:

- `ANTHROPIC_API_KEY` — Anthropic direct API access
- `OPENAI_API_KEY` — OpenAI API access
- `GEMINI_API_KEY` — Google Gemini API access
- `OPENROUTER_API_KEY` — OpenRouter API access

### Google Cloud / Vertex AI Integration

For Vertex AI users, the sandbox supports two credential paths:

1. **Service account key**: If `GOOGLE_APPLICATION_CREDENTIALS` points to a key file, the file is mounted into the container
2. **Application Default Credentials (ADC)**: The `~/.config/gcloud/` directory is mounted for `gcloud auth application-default` token flow

When a cloud provider is detected, the sandbox auto-starts the LLM gateway (`uf gateway`) to proxy API requests. The container receives `ANTHROPIC_BASE_URL` and `ANTHROPIC_AUTH_TOKEN` environment variables pointing to the gateway instead of direct credential mounts.

## See Also

- [Quick Start](/docs/getting-started/quick-start/) -- Install and verify the toolchain
- [Developer Guide](/docs/getting-started/developer/) -- Daily workflow with the `uf` CLI
- [Common Workflows](/docs/getting-started/common-workflows/) -- End-to-end feature and review flows
