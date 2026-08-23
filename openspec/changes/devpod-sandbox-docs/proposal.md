## Why

The Unbound Force website has comprehensive reference documentation for sandbox commands and configuration, but lacks task-oriented guides that walk users through the DevPod-based development environment setup end-to-end. Key information is scattered across four reference pages (sandbox.md, config.md, cli.md, gateway.md) and users must piece together a workflow from fragmented details.

Seven GitHub issues (#83, #87, #120, #121, #199, #50, #119) have landed significant DevPod, sandbox, and multi-platform changes since the website was last updated. The reference pages cover most command syntax but miss the narrative "why" and "how" that helps users choose the right approach for their situation.

Additionally, multi-platform scaffold support (`uf init --platform`) for Cursor alongside OpenCode has no website coverage at all. The `--ide` flag for DevPod IDE selection and the `uf ollama-proxy` command for GPU-less workflows are also undocumented on the website.

## What Changes

1. **New getting-started guide: "Development Environment Setup"** — A task-oriented guide covering the full DevPod sandbox workflow from prerequisites through first AI agent session, with platform-specific paths for macOS and Linux.

2. **New getting-started guide: "Multi-Platform Support"** — Documents `uf init --platform` for OpenCode and Cursor, including dual-platform scaffolding and Cursor-specific output (`.cursor/` directories, `.mdc` rule files).

3. **Updated reference page: sandbox.md** — Incorporate changes from issues #83 (DevPod as primary backend, Che removal), #120 (--ide flag resolution chain), #199 (provider pre-flight simplification), and #87 (ollama-proxy for GPU-less workflows).

4. **Updated reference page: cli.md** — Add `uf ollama-proxy` command reference and update `uf sandbox` subcommands to reflect current flag set including `--ide` and `--provider`.

5. **Updated changelog** — Add entries for DevPod provider changes, IDE selection, ollama-proxy, and multi-platform scaffold support.

## Capabilities

### New Capabilities
- `devenv-setup-guide`: Task-oriented guide for setting up the DevPod-based development environment, covering prerequisites (Podman, DevPod), `uf sandbox init`, workspace lifecycle, IDE integration, and gateway auto-start.
- `multi-platform-guide`: Guide documenting `uf init --platform` for OpenCode and Cursor scaffolding, including dual-platform support and platform-specific output formats.

### Modified Capabilities
- `sandbox-reference`: Updated to reflect DevPod as the primary CDE backend, simplified provider pre-flight, IDE selection resolution chain, and ollama-proxy integration for GPU-less workflows.
- `cli-reference`: Updated with `uf ollama-proxy` command and current `uf sandbox` flag set.
- `changelog`: Updated with recent release entries covering DevPod, IDE, and multi-platform changes.

### Removed Capabilities
- None. Eclipse Che/Dev Spaces documentation is retained as "legacy/experimental" rather than removed, since the backend still exists in the CLI with a migration error message.

## Impact

- **Content files affected**: 2 new pages created, 3 existing pages updated
- **Navigation**: New pages added to Getting Started section sidebar via frontmatter weight
- **Homepage**: No changes (homepage does not need modification for documentation pages)
- **Styles**: No CSS/SCSS changes needed
- **Build**: All changes are Markdown content; no template or configuration changes

## Constitution Alignment

Assessed against the Unbound Force org constitution.

### I. Autonomous Collaboration

**Assessment**: N/A

This change adds website documentation pages (static Markdown content). It does not modify agent artifacts, communication formats, or inter-agent workflows. Documentation is a read-only artifact consumed by humans, not agents.

### II. Composability First

**Assessment**: N/A

Website documentation does not introduce dependencies between agents or require any agent to be present. The documentation describes existing composable features (sandbox works standalone, gateway auto-detects) without altering their behavior.

### III. Observable Quality

**Assessment**: N/A

This change produces static documentation. It does not affect agent output formats, provenance metadata, or quality metrics. Content accuracy is validated against upstream issue descriptions and repository artifacts.

### IV. Testability

**Assessment**: N/A

No production code, test code, or agent prompts are modified. Validation is the standard website gate: `npm run build` succeeds, pages render correctly, no broken links.

### V. Security by Default

**Assessment**: N/A

No dependencies added, no CI changes, no external inputs processed. Documentation content does not affect the security posture of any component.
