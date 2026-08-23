---
title: "Changelog"
description: "Release history for the Unbound Force toolchain — user-facing changes per release, sourced from actual release artifacts."
lead: "User-facing changes per release."
date: 2026-05-02T00:00:00+00:00
draft: false
weight: 10
toc: true
---

## v0.13.0 — 2026-08-23

### Added

- **DevPod as primary CDE backend** -- DevPod with Podman is now the default and recommended container backend. Eclipse Che / Dev Spaces is deprecated; `--backend che` returns a migration error with guidance to switch to DevPod.
- **Sandbox IDE selection** -- New `--ide` flag on `uf sandbox start` and `uf sandbox create` for launching workspaces with a specific IDE (vscode, openvscode, fleet, jupyternotebook, cursor, or none). Resolution chain: CLI flag > `UF_SANDBOX_IDE` env var > `.uf/config.yaml` > default `none`.
- **`uf ollama-proxy` command** -- Start, stop, and check status of a local Ollama-compatible proxy for GPU-less embedding workflows. Enables developers without GPUs to use cloud-hosted embedding models through a local endpoint.
- **Multi-platform scaffold deployment** -- New `uf init --platform` flag supports `opencode` and `cursor` targets. Repeatable flag (`--platform opencode --platform cursor`) scaffolds both platforms simultaneously. Cursor output generates `.cursor/` directory with `.mdc` rule files and `.cursor/mcp.json`.
- **Podman and DevPod in `uf setup` and `uf doctor`** -- `uf setup` now installs Podman (step 14), DevPod (step 15), and configures the DevPod Podman provider (step 16). `uf doctor` checks Podman runtime health (Fail severity) and DevPod version/provider (Warn severity).

### Changed

- **DevPod provider pre-flight simplified** -- Removed `podman` PATH lookup pre-flight check. DevPod provider references the registered provider name, not a binary in PATH. Diagnostic hint on `devpod up` failure points to `uf doctor` and `uf setup`.

## v0.12.0 — 2026-04-14

### Added

- **Containerized development sessions** -- New `uf sandbox` command group for launching, managing, and extracting changes from containerized OpenCode sessions. Supports Podman (local) and Eclipse Che / Dev Spaces (CDE) backends.
- **Linux cross-platform install** -- RPM packages and Homebrew Formula for Linux installations.
- **Externalized Speckit commands** -- Speckit, OpenSpec, and Gaze init assets are now externalized from embedded assets, enabling independent updates.
- **Workflow phase boundaries** -- Speckit commands now enforce phase boundaries (spec vs. implementation), preventing out-of-phase changes.

### Fixed

- Guardrails added to `/opsx-propose` to prevent phase boundary violations.
- Anthropic Vertex environment variables now forwarded to sandbox containers.
- Google Cloud config directory mounted for Vertex AI auth in sandbox.
