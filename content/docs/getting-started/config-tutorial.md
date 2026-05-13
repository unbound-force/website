---
title: "Configuration Tutorial"
description: "Step-by-step guide to customizing your Unbound Force environment — package managers, embedding models, gateway ports, and config validation."
lead: "Customize your uf environment with .uf/config.yaml. Create a config file, set platform-specific values, and verify with uf config show."
date: 2026-05-03T00:00:00+00:00
draft: false
weight: 74
toc: true
---

## Prerequisites

The `uf` CLI must be installed. Run `uf version` to confirm.

## Step 1: Create a Config File

Generate a config file with all available settings commented out:

```bash
uf config init
```

This creates `.uf/config.yaml` in your project directory. The file contains every configuration key as a YAML comment with its default value. Nothing is active until you uncomment it.

```text
Created .uf/config.yaml (all values commented — defaults apply)
```

Open the file to see the available sections:

```yaml
# setup:
#   package_manager: auto    # auto, brew, dnf, apt
#   skip: []                 # tools to skip during setup

# scaffold:
#   language: ""             # auto-detected from go.mod, package.json, etc.

# embedding:
#   model: granite-embedding:30m
#   dimensions: 256

# sandbox:
#   runtime: auto            # auto, podman, docker
#   image: ""                # default: quay.io/unbound-force/opencode-dev:latest
#   ide: none                # none, vscode, openvscode, fleet, jupyternotebook, cursor

# gateway:
#   port: 53147
#   provider: ""             # auto, anthropic, vertex, bedrock

# doctor:
#   skip: []                 # checks to skip

# workflow:
#   execution_modes:
#     define: human           # human or swarm
```

Each key shows its default. Uncomment and change only what you need.

## Step 2: Customize for Your Platform

### Scenario: Fedora/RHEL Package Manager

By default, `uf setup` auto-detects your package manager (Homebrew on macOS, apt on Debian/Ubuntu). On Fedora or RHEL, set it explicitly:

```yaml
setup:
  package_manager: dnf
```

Verify the setting took effect:

```bash
uf config show
```

```text
setup:
  package_manager: dnf    ← your override
  skip: []
```

The output shows the effective value after all config layers merge. Your override is active.

### Scenario: Different Embedding Model

The default embedding model (`granite-embedding:30m`) is optimized for low-resource machines. If you have more VRAM or need higher-quality embeddings:

```yaml
embedding:
  model: nomic-embed-text
  dimensions: 768
```

The `dimensions` value must match what the model produces. Check the model's documentation for the correct dimension count.

### Scenario: Custom Gateway Port

The default gateway port (53147) avoids conflicts with common services. If you have a conflict:

```yaml
gateway:
  port: 9090
```

### Scenario: Skip Tools During Setup

Skip tools you do not need. This speeds up `uf setup` and avoids installing dependencies you will not use:

```yaml
setup:
  skip:
    - gaze
    - mxf
```

After setting this, `uf setup` will install OpenCode, Speckit, Replicator, and Dewey but skip Gaze and Mx F.

### Scenario: Default IDE for Sandbox Workspaces

When using DevPod persistent workspaces, set a default IDE so it opens automatically after workspace creation:

```yaml
sandbox:
  ide: vscode
```

Verify with `uf config show`:

```bash
uf config show
```

```text
sandbox:
  ide: vscode    ← your override (default: none)
```

Now `uf sandbox create` will open VS Code after the workspace is ready. Valid values: `none`, `vscode`, `openvscode`, `fleet`, `jupyternotebook`, `cursor`. You can override per-command with `uf sandbox create --ide cursor`.

## Step 3: Understand Config Precedence

Configuration is resolved through 5 layers. Each layer overrides the one below it:

```text
CLI flags           ← highest priority (always wins)
Environment vars
Repo config         ← .uf/config.yaml (project-specific, shared via git)
User config         ← ~/.config/uf/config.yaml (personal defaults)
Compiled defaults   ← lowest priority
```

### Precedence Example

Suppose your **user config** (`~/.config/uf/config.yaml`) sets a default gateway port:

```yaml
gateway:
  port: 8080
```

And your **repo config** (`.uf/config.yaml`) sets a project-specific port:

```yaml
gateway:
  port: 9090
```

The repo config wins (higher priority). Verify with `uf config show`:

```bash
uf config show
```

```text
gateway:
  port: 9090    ← repo config wins over user config
```

If you then run `uf gateway --port 3000`, the CLI flag wins over both config files.

## Step 4: Validate Your Config

Catch errors before they surface at runtime:

```bash
uf config validate
```

If the config is valid:

```text
✓ Config is valid
```

If there are problems (unknown keys, type mismatches):

```text
✗ Config validation failed:
  - unknown key: setup.package_manger (did you mean package_manager?)
  - invalid value for gateway.port: "nine thousand" (expected integer)
```

Fix the issues and re-run `uf config validate` until it passes.

## Step 5: View the Merged Config

See the final effective configuration after all layers merge:

```bash
uf config show
```

This shows every key with its resolved value — useful for debugging which layer a setting came from. For machine-parseable output:

```bash
uf config show --format json
```

## Summary

| Step | Command | Purpose |
|------|---------|---------|
| Create | `uf config init` | Generate `.uf/config.yaml` with commented defaults |
| Edit | Open `.uf/config.yaml` | Uncomment and change the settings you need |
| Validate | `uf config validate` | Catch typos and invalid values |
| Verify | `uf config show` | See the effective merged configuration |

## See Also

- [Configuration Reference](/docs/reference/config/) -- All config sections, fields, and environment variable overrides
- [Common Workflows](/docs/getting-started/common-workflows/) -- Workflow configuration with `.uf/config.yaml`
- [Quick Start](/docs/getting-started/quick-start/) -- Install and verify the toolchain
- [Developer Guide](/docs/getting-started/developer/) -- Daily workflow with the `uf` CLI
