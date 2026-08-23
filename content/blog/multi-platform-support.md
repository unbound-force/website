---
title: "Multi-Platform Support — uf init Now Scaffolds for Cursor"
description: "Unbound Force gains multi-platform scaffold support. uf init --platform cursor generates native Cursor configuration alongside or instead of OpenCode files — from a single canonical asset set."
lead: "From OpenCode-only to platform-agnostic. uf init --platform cursor translates convention packs into .mdc rules and MCP config into .cursor/mcp.json — one command, two platforms."
slug: "multi-platform-support"
date: 2026-08-23T00:00:00+00:00
draft: false
weight: 109
toc: true
categories: ["Engineering"]
tags: ["multi-platform", "cursor", "opencode", "scaffolding"]
contributors: ["Unbound Force"]
---

## The Single-Platform Ceiling

Until now, Unbound Force was an OpenCode integration. Convention packs, MCP server configuration, agent prompts, and skill definitions all targeted one platform's file layout. If your team used Cursor, you were out of luck — or stuck maintaining a parallel set of hand-written configuration files that drifted from the canonical source within days.

This constraint was artificial. The *knowledge* encoded in convention packs — coding standards, architectural patterns, review severity definitions — has nothing to do with which editor runs the AI agent. A rule like "exported Go functions require GoDoc comments" is equally valid whether the agent reads it from `.opencode/uf/packs/go.md` or from a Cursor `.mdc` rule file. The platform-specific part is the container format, not the content.

Spec 035 eliminates this ceiling. `uf init --platform cursor` now generates native Cursor configuration from the same canonical asset set that powers OpenCode scaffolding.

## One Asset Set, Multiple Outputs

The core design follows the Strategy pattern. A `Platform` interface defines the contract for translating canonical assets into platform-specific files. Each platform implementation — `OpenCodePlatform`, `CursorPlatform`, and future additions — handles its own file layout, naming conventions, and format requirements.

```go
type Platform interface {
    Name() string
    Scaffold(ctx context.Context, assets AssetSet, opts Options) error
}
```

Convention packs are the clearest example of this translation. In OpenCode, a convention pack is a Markdown file at `.opencode/uf/packs/go.md` that agents read during code review. In Cursor, the equivalent is an `.mdc` rule file with YAML frontmatter specifying glob patterns for auto-attachment. The content — the actual coding rules — stays identical. The scaffold engine handles the format translation.

```yaml
# .cursor/rules/go.mdc (generated)
---
description: "Go convention pack — coding standards for Go source files"
globs: ["**/*.go"]
alwaysApply: false
---

[CS-001] Exported functions MUST have GoDoc comments...
```

MCP server configuration follows the same principle. OpenCode stores MCP config in `opencode.json` under the `mcpServers` key. Cursor expects it in `.cursor/mcp.json` with a slightly different schema. The scaffold engine reads the canonical MCP server definitions and writes the correct format for each target platform.

## Dual-Platform Projects

Teams that use both OpenCode and Cursor — or that are migrating between them — can scaffold for multiple platforms in a single command:

```bash
uf init --platform opencode --platform cursor
```

This generates both `.opencode/` and `.cursor/` directory trees from the same source assets. The two configurations coexist in the same repository without conflict. Each developer uses whichever platform they prefer, and both see the same convention packs, the same MCP servers, and the same agent capabilities.

This matters for adoption. Teams do not switch tools overnight. A migration path that lets both platforms run side-by-side, drawing from a single source of truth, removes the "all or nothing" barrier that blocks tool adoption in larger organizations.

## Backward Compatibility

Existing `uf init` behavior is unchanged. Without an explicit `--platform` flag, the command defaults to OpenCode — the same output it has always produced. No existing workflows break. No configuration files move. The multi-platform capability is additive.

Projects that have already run `uf init` can add Cursor support incrementally:

```bash
uf init --platform cursor
```

This generates the Cursor-specific files without touching the existing OpenCode configuration. The scaffold engine detects what already exists and fills in the gaps rather than overwriting.

## Extensibility: The Platform Interface

The `Platform` interface is deliberately minimal. Adding support for a new AI coding tool — Claude Code, GitHub Copilot, or whatever ships next quarter — requires implementing a single interface with a handful of methods. The canonical asset set does not change. The translation logic is isolated in the new platform implementation.

This design decision reflects a bet about the market: AI coding tools will proliferate, and teams will use more than one. Building Unbound Force around a platform-agnostic asset model means the project's value compounds with each new platform rather than fragmenting across incompatible configurations.

The `Platform` interface also opens the door for community contributions. A team using an internal or niche AI tool can implement the interface for their platform without modifying the core scaffold engine.

## What This Means for the Project

This release transforms Unbound Force from a single-tool integration into a platform-agnostic agent framework. Convention packs, MCP servers, agent prompts, and skills are now portable assets that target any supported platform through a clean translation layer.

The architectural significance goes beyond Cursor support. By separating *what* the agents know (convention packs, specs, constitutions) from *where* they run (OpenCode, Cursor, future platforms), Unbound Force becomes the canonical source of engineering standards for a team — regardless of which AI coding tool individual developers prefer.

## Get Started

Update to the latest `uf` binary and scaffold for Cursor:

```bash
uf init --platform cursor
```

For dual-platform projects:

```bash
uf init --platform opencode --platform cursor
```

Review the generated `.cursor/rules/` directory to see your convention packs translated into `.mdc` format, and `.cursor/mcp.json` for your MCP server configuration. File an issue on the [unbound-force/unbound-force](https://github.com/unbound-force/unbound-force) repository if you hit edge cases in the translation — the platform interface is new, and real-world feedback sharpens it.
