---
title: "Isolate Your AI Agents with uf sandbox — Containerized OpenCode Sessions via Podman"
description: "AI coding agents run with full access to your filesystem. uf sandbox wraps the entire session in a Podman container — one command replaces manual container setup, and your repo stays untouchable."
lead: "Your repo is untouchable. One command puts your AI agent in a container where it can read your code but cannot modify your files. Changes come out only through a reviewed patch extraction."
slug: "sandbox-isolation"
date: 2026-05-03T00:00:00+00:00
draft: false
weight: 70
toc: true
categories: ["Engineering"]
tags: ["sandbox", "podman", "isolation", "security"]
contributors: ["Unbound Force"]
---

## The Problem

AI coding agents are powerful collaborators, but they run with the same filesystem access as any other process on your machine. When an agent executes a command, it is real — `rm -rf`, `git push --force`, a corrupted `.git` directory, a rewritten configuration file. The damage is immediate and often irreversible.

The standard mitigations do not hold up. "Be careful with your prompts" is not a security boundary. `git stash` protects committed files but not untracked work. Running the agent in a separate directory means maintaining two copies of your project. None of these approaches provide actual isolation — the agent still has write access to whatever you mount or share.

The core tension: you want the agent to have full context of your codebase (it needs to read everything to be useful), but you do not want it to have full write access (one bad command and your work is gone). What you need is a read-only view of your project with a controlled exit path for changes.

## The Solution: `uf sandbox`

`uf sandbox start` wraps the entire OpenCode + Unbound Force toolchain in a rootless Podman container. Your project directory is mounted read-only by default — the agent can read every file but writes go to a container overlay. When the agent is done, `uf sandbox extract` generates a git patch from the container's work and presents it for your review before applying anything to the host repo.

One command replaces four manual steps: checking Podman availability, building the container run flags, waiting for the OpenCode server to start, and attaching the TUI. The sandbox handles platform detection (arm64/amd64), SELinux volume labels (`:Z` on enforcing systems), Ollama connectivity, and API key forwarding automatically.

Two mount modes give you flexibility:

- **Isolated** (default): Read-only mount. Changes stay inside the container until you extract them. Maximum safety.
- **Direct**: Read-write mount. Changes apply immediately to the host filesystem. Use when you need real-time sync (e.g., running tests on the host while the agent edits).

## Walkthrough: The Round-Trip

### 1. Start the Sandbox

```bash
uf sandbox start
```

The sandbox runs through a startup sequence:

```text
  Checking prerequisites...
  ✓ Podman available (v5.3.1)
  ✓ Container image available (quay.io/unbound-force/opencode-dev:latest)
  ✓ API key configured

  Starting container...
  ✓ Container started (isolated mode, read-only mount)
  ✓ Health check passed (OpenCode server ready)

  Attaching to OpenCode TUI...
```

You are now inside the sandbox. The TUI looks identical to a normal OpenCode session — the agent has full access to your project files (read-only) and all the Unbound Force tools (Speckit, Replicator, Divisor agents).

### 2. Work Inside the Sandbox

Run any workflow command as usual:

```text
/unleash
```

The agent reads your spec, plans the implementation, writes code, runs tests, and reviews its own work — all inside the container. Every file modification, every `git commit`, every test run happens in the container's overlay filesystem. Your host repo is untouched.

When `/unleash` finishes, detach from the sandbox with `Ctrl+C` or let it complete naturally.

### 3. Extract Changes

Back on the host, pull the agent's work out of the container:

```bash
uf sandbox extract
```

The extraction uses `git format-patch` to generate patches from the container's commit history, then presents them for review:

```text
  Extracting changes from sandbox...

  Found 3 commits:
    feat: add user authentication module
    test: add auth module unit tests
    docs: update API reference for auth endpoints

  Patch summary:
    5 files changed, 342 insertions(+), 12 deletions(-)

  Apply these changes to the host repo? [y/N]
```

You see every commit, every file, every line change before anything touches your repo. Type `y` to apply via `git am` (preserving commit history) or `N` to discard.

### 4. Verify on the Host

After applying, the commits are in your host repo with their original messages and authorship intact:

```bash
git log --oneline -3
```

```text
a1b2c3d feat: add user authentication module
d4e5f6g test: add auth module unit tests
h7i8j9k docs: update API reference for auth endpoints
```

Run your tests, review the diff, push when ready. The round-trip is complete.

## Lifecycle Management

Three additional commands handle the sandbox lifecycle:

```bash
uf sandbox status    # show container state, image, uptime
uf sandbox stop      # stop the container (removes ephemeral containers)
uf sandbox attach    # reconnect to a running sandbox's TUI
```

`status` is useful for checking if a sandbox is already running before starting a new one. `attach` lets you reconnect after detaching — the OpenCode session continues running in the background.

## Security Model

The sandbox provides multiple isolation layers:

| Property | Description |
|----------|-------------|
| **Rootless Podman** | Container runs without root privileges on the host |
| **Read-only mounts** | Isolated mode mounts the project read-only (default) |
| **No push credentials** | Git push credentials are never forwarded to the container |
| **Resource limits** | Memory (default 8g) and CPU (default 4) limits prevent runaway processes |
| **SELinux** | Auto-detects SELinux and applies `:Z` volume labels on enforcing systems |
| **Non-root user** | Container runs as UID 1000 (non-root) inside the namespace |

The agent can read your code but cannot: modify your files (isolated mode), push to your remote, consume unbounded resources, or escalate privileges. The blast radius of any agent mistake is contained to a disposable container.

## Google Cloud / Vertex AI Users

If you use Vertex AI for your LLM provider, the sandbox automatically starts the gateway (`uf gateway`) to proxy API requests. When using the gateway, no cloud credentials are mounted into the container — the gateway runs on the host and handles authentication. Your container receives `ANTHROPIC_BASE_URL` and `ANTHROPIC_AUTH_TOKEN` environment variables pointing to the gateway. As a fallback, the sandbox also supports direct credential mounting: service account key files are auto-mounted read-only, and the gcloud config directory is mounted for Application Default Credentials.

## Current Limitations

- **Single container at a time**: You cannot run multiple sandbox sessions concurrently for the same project. Parallel agent sessions require separate project directories.
- **Podman required**: The sandbox uses Podman, not Docker. On macOS, a Podman machine must be running (`podman machine start`).
- **Fixed health check timeout**: The OpenCode server health check waits up to 60 seconds — there is no `--timeout` flag to adjust this.

## Persistent Workspaces with DevPod

Ephemeral containers are the default, but for long-running sessions you can create persistent workspaces backed by DevPod:

```bash
uf sandbox create                        # persistent workspace
uf sandbox create --ide vscode           # open VS Code after creation
uf sandbox start --ide cursor            # resume and open Cursor
```

Persistent workspaces survive stop/start cycles — the container's filesystem state is retained across restarts. DevPod handles workspace provisioning and IDE integration. The `--ide` flag supports `vscode`, `openvscode`, `fleet`, `jupyternotebook`, and `cursor`.

The workspace uses a `.devcontainer/devcontainer.json` configuration generated per-developer by `uf sandbox init`. This file is gitignored because the configuration is OS-specific: macOS and Linux require different Podman user namespace flags for correct UID mapping inside the container. Each developer generates their own rather than committing a shared config that works on only one platform.

Eclipse Che / Dev Spaces support exists as an experimental alternative backend (`--backend che`) but has not been validated end-to-end with a production Che instance. For custom container images, the [opencode-dev containerfile](https://github.com/unbound-force/opencode-dev) repository provides the base image used by the sandbox.

## Get Started

Install or upgrade the Unbound Force CLI and start your first sandbox session:

```bash
brew install unbound-force/tap/unbound-force   # new install
brew upgrade unbound-force                      # existing users
uf sandbox start
```

Your repo stays untouchable. The agent works in a container. Changes come out only when you say so.

## See Also

- [Sandbox Reference](/docs/reference/sandbox/) -- Command reference for all `uf sandbox` subcommands, flags, and configuration
- [Quick Start](/docs/getting-started/quick-start/) -- Install and verify the toolchain
- [Developer Guide](/docs/getting-started/developer/) -- Daily workflow with the `uf` CLI
- [Common Workflows](/docs/getting-started/common-workflows/) -- End-to-end feature and review flows
