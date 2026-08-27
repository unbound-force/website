---
title: "Multi-Platform Support"
description: "Scaffold projects for OpenCode, Cursor, or both AI coding platforms with uf init --platform for consistent coding standards."
lead: "Configure uf init --platform to generate platform-specific configurations for your preferred AI coding environment."
date: 2026-05-02T00:00:00+00:00
draft: false
weight: 27
toc: true
---

## Overview

Unbound Force works with multiple AI coding platforms. The `--platform` flag on `uf init` controls which platform-specific configuration files are generated during project scaffolding. You can target a single platform or scaffold for both simultaneously.

Supported platforms:

| Platform     | Flag Value   | Output Directory | What Gets Generated                                                  |
| ------------ | ------------ | ---------------- | -------------------------------------------------------------------- |
| **OpenCode** | `opencode`   | `.opencode/`     | MCP server config, convention packs, agent definitions, commands     |
| **Cursor**   | `cursor`     | `.cursor/`       | `.mdc` rule files (translated convention packs), `.cursor/mcp.json`  |

OpenCode is the default. If you omit `--platform`, `uf init` scaffolds for OpenCode.

## Scaffolding for a Single Platform

### OpenCode (Default)

OpenCode is the full Unbound Force experience. When you run `uf init` without a `--platform` flag, it generates the OpenCode configuration:

```bash
uf init
```

This is equivalent to:

```bash
uf init --platform opencode
```

The `.opencode/` directory receives:

- **MCP server configuration** -- entries for Dewey (knowledge retrieval) and Replicator (multi-agent coordination) in `opencode.json`
- **Convention packs** -- numbered, severity-classified coding rules in `.opencode/uf/packs/` (e.g., `default.md`, `go.md`, `content.md` plus user-owned `*-custom.md` variants)
- **Agent definitions** -- Divisor review personas, Cobalt-Crush, Constitution Check, and other hero agents
- **Commands** -- Speckit pipeline commands (`/speckit.specify`, `/uf.unleash`, `/uf.finale`, etc.)

See the [Developer Guide](/docs/getting-started/developer/#project-scaffolding-with-uf-init) for the full list of deployed files and the file ownership model.

### Cursor

To scaffold for Cursor instead of OpenCode:

```bash
uf init --platform cursor
```

The `.cursor/` directory receives:

- **`.mdc` rule files** -- convention packs translated from OpenCode's Markdown format into Cursor's native rule format. MUST/SHOULD/MAY rules from `.opencode/uf/packs/*.md` files are converted to `.mdc` files that Cursor reads automatically.
- **`.cursor/mcp.json`** -- MCP server configuration translated to Cursor's expected format, enabling Dewey and Replicator integration within Cursor.

Cursor uses its own IDE features for agent interaction, so the full agent definitions and Speckit commands are not deployed. Instead, you get Unbound Force's coding standards and MCP tool access through Cursor's native mechanisms.

## Dual-Platform Scaffolding

If your team uses both OpenCode and Cursor, scaffold for both platforms in a single command:

```bash
uf init --platform opencode --platform cursor
```

The `--platform` flag is repeatable. This generates both sets of configuration files:

- `.opencode/` -- full OpenCode configuration (agents, commands, convention packs, MCP config)
- `.cursor/` -- Cursor-native configuration (`.mdc` rule files, `.cursor/mcp.json`)

Both configurations coexist in the same repository. Each platform's files live in separate directories, so there are no conflicts. Team members using different editors can work on the same codebase with consistent coding standards enforced through their preferred platform.

## Divisor Subset Mode

For teams that want Unbound Force's code review standards without the full swarm workflow, combine `--divisor` with `--platform`:

```bash
uf init --divisor --platform cursor
```

This deploys only:

- **Divisor review agents** -- the multi-persona review council (Guard, Architect, Adversary, SRE, Testing, Content)
- **Convention packs** -- coding standards translated to Cursor's `.mdc` rule format

This is useful when a team already has an established workflow but wants to adopt Unbound Force's structured code review and convention enforcement. The Divisor subset works with any platform -- `--divisor --platform opencode` deploys the same subset for OpenCode.

See the [Code Review Tutorial](/docs/getting-started/code-review-tutorial/) for how the Divisor review council works.

## Choosing a Platform

### When to Use OpenCode

OpenCode provides the full Unbound Force workflow:

- **Speckit pipeline** -- structured specification, planning, and task decomposition before implementation
- **Hero personas** -- specialized agents (Cobalt-Crush for implementation, The Divisor for review, Mx F for management)
- **Swarm coordination** -- parallel workers via Replicator with file reservations and semantic memory
- **Convention packs** -- MUST/SHOULD/MAY rules with automated enforcement during review

Choose OpenCode when you want the complete AI-assisted software engineering workflow from specification through implementation and review.

### When to Use Cursor

Cursor provides a lighter-weight integration:

- **Native IDE features** -- Cursor's built-in AI capabilities with UF's coding standards layered on top
- **Convention enforcement** -- the same MUST/SHOULD/MAY rules, delivered through Cursor's `.mdc` rule format
- **MCP tool access** -- Dewey knowledge retrieval and Replicator coordination available through Cursor's MCP support

Choose Cursor when you prefer Cursor's IDE experience and want Unbound Force's coding standards and MCP tools without adopting the full Speckit/hero workflow.

### Switching Platforms

To add support for a second platform, run `uf init` with the new platform flag:

```bash
# Already using OpenCode, adding Cursor support
uf init --platform cursor

# Already using Cursor, adding OpenCode support
uf init --platform opencode
```

Existing configuration files are preserved. `uf init` follows the same file ownership model regardless of platform -- user-owned files are never overwritten, and tool-owned files are updated when the embedded version changes. See the [Developer Guide](/docs/getting-started/developer/#file-ownership-model) for details.

## See Also

- [Quick Start](/docs/getting-started/quick-start/) -- Install the toolchain and scaffold your first project
- [Developer Guide](/docs/getting-started/developer/#project-scaffolding-with-uf-init) -- Full `uf init` documentation including file ownership and sub-tool initialization
- [CLI Reference](/docs/reference/cli/) -- Complete `uf init` flag documentation
- [Code Review Tutorial](/docs/getting-started/code-review-tutorial/) -- How the Divisor review council works with convention packs
- [Convention Packs](/blog/convention-packs/) -- How MUST/SHOULD/MAY rules work and how to customize them
