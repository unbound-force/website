---
title: "Configuration"
description: "Unified configuration for the uf CLI — layered loading, 7 config sections, and common customization examples."
lead: "The uf config command group manages .uf/config.yaml — a single file that controls setup, scaffolding, embedding, sandbox, gateway, doctor, and workflow behavior."
date: 2026-05-02T00:00:00+00:00
draft: false
weight: 20
toc: true
---

## Overview

The `uf` CLI uses a unified configuration system. All settings live in `.uf/config.yaml` at the repository root. The file is optional — when absent, compiled defaults apply with no error. You never need to create this file manually; `uf config init` generates it with all values commented out so you can see what's available and uncomment what you want to change.

## Commands

### config init

Create or update `.uf/config.yaml`. Generates a fully-commented template showing all available settings with their defaults. Existing files are updated with new sections while preserving your customizations.

```bash
uf config init              # current directory
uf config init --dir ./myproject
```

| Flag | Description |
|------|-------------|
| `--dir <string>` | Target directory (default `.`) |

### config show

Display the effective configuration after all layers merge. Shows the final resolved values — useful for debugging which layer a setting came from.

```bash
uf config show              # human-readable text
uf config show --format json  # machine-parseable JSON
```

| Flag | Description |
|------|-------------|
| `--dir <string>` | Target directory (default `.`) |
| `--format <string>` | Output format: `text` or `json` (default `text`) |

### config validate

Validate the config file against known field values. Reports invalid keys, unknown sections, and type mismatches.

```bash
uf config validate
uf config validate --format json
```

| Flag | Description |
|------|-------------|
| `--dir <string>` | Target directory (default `.`) |
| `--format <string>` | Output format: `text` or `json` (default `text`) |

## Layered Loading

Configuration is resolved through 5 layers. Each layer overrides the one below it:

```text
CLI flags           ← highest priority (always wins)
Environment vars    ← override config files
Repo config         ← .uf/config.yaml in the project
User config         ← ~/.config/uf/config.yaml (per-user defaults)
Compiled defaults   ← lowest priority (built into the binary)
```

**Repo config** (`.uf/config.yaml`) is for project-specific settings shared across the team via version control. **User config** (`~/.config/uf/config.yaml`) is for personal defaults that apply across all projects — like your preferred package manager or a custom embedding host.

Missing config files are not an error. If neither file exists, compiled defaults apply for all settings.

### Environment Variable Overrides

Every config field can be overridden with an environment variable. The naming convention is `UF_` + section + `_` + field in uppercase:

| Environment Variable | Config Field |
|---------------------|--------------|
| `UF_SETUP_PACKAGE_MANAGER` | `setup.package_manager` |
| `UF_SCAFFOLD_LANGUAGE` | `scaffold.language` |
| `UF_EMBEDDING_MODEL` | `embedding.model` |
| `UF_EMBEDDING_DIMENSIONS` | `embedding.dimensions` |
| `UF_SANDBOX_RUNTIME` | `sandbox.runtime` |
| `UF_SANDBOX_IMAGE` | `sandbox.image` |
| `UF_SANDBOX_IDE` | `sandbox.ide` |
| `UF_GATEWAY_PORT` | `gateway.port` |
| `UF_GATEWAY_PROVIDER` | `gateway.provider` |

### Precedence Example

If your user config sets `gateway.port: 8080` and your repo config sets `gateway.port: 9090`, the repo config wins (it's higher priority). If you then run `uf gateway --port 3000`, the CLI flag wins over both.

## Configuration Sections

The config file has 7 sections. Each controls a specific part of the `uf` toolchain:

| Section | Purpose | Key Settings |
|---------|---------|-------------|
| **setup** | Controls how `uf setup` installs tools | `package_manager` (auto, brew, dnf, apt), `skip` (tools to skip) |
| **scaffold** | Controls what `uf init` deploys | `language` (auto-detected from go.mod, package.json, etc.) |
| **embedding** | Embedding model for Dewey semantic search | `model` (default: granite-embedding:30m), `dimensions` (default: 256) |
| **sandbox** | Controls `uf sandbox` containerized sessions | `runtime` (auto, podman, docker), `image`, `ide` (none, vscode, cursor, etc.), `resources.memory` |
| **gateway** | Controls `uf gateway` LLM reverse proxy | `port` (default: 53147), `provider` (auto, anthropic, vertex, bedrock) |
| **doctor** | Controls `uf doctor` health checks | `skip` (checks to skip), `tools` (custom tool paths) |
| **workflow** | Controls hero lifecycle execution modes | `execution_modes.define` (human/swarm), `spec_review` (true/false) |

## Common Customizations

### Set the package manager (Fedora/RHEL)

```yaml
setup:
  package_manager: dnf
```

By default, `uf setup` auto-detects your package manager (Homebrew on macOS, apt on Debian/Ubuntu). Override this for platforms where auto-detection picks the wrong one.

### Change the embedding model

```yaml
embedding:
  model: nomic-embed-text
  dimensions: 768
```

The default model (`granite-embedding:30m`) is optimized for low-resource machines. If you have more VRAM or need higher-quality embeddings, switch to a larger model and update dimensions to match.

### Change the gateway port

```yaml
gateway:
  port: 9090
```

The default port (53147) avoids conflicts with common services. Change it if you have a port conflict or prefer a standard port.

### Open an IDE with sandbox sessions

```yaml
sandbox:
  ide: vscode
```

By default, `uf sandbox start` creates the workspace without opening an IDE (`none`). Set `ide` to have DevPod automatically open your preferred editor after workspace creation. Valid values: `none`, `vscode`, `openvscode`, `fleet`, `jupyternotebook`, `cursor`.

### Skip tools during setup

```yaml
setup:
  skip:
    - gaze
    - mxf
```

Skip tools you don't need. This speeds up `uf setup` and avoids installing dependencies for tools you won't use.

## See Also

- [Common Workflows](/docs/getting-started/common-workflows/) -- Workflow configuration with `.uf/config.yaml`
- [Quick Start](/docs/getting-started/quick-start/) -- Install and verify the toolchain
- [Developer Guide](/docs/getting-started/developer/) -- Daily workflow with the `uf` CLI
