## Why

The `uf` CLI has grown to 6 command groups with 15+ subcommands, but the website has no single CLI reference page. Users looking for "what can `uf` do?" have no landing page. Additionally, the website's "Three Tools" stack table (OpenCode + Speckit + Replicator) omits the CLI layer entirely, and there is no changelog or "what's new" page despite 84+ commits shipping since v0.12.0.

This change addresses GitHub issues #62 (CLI reference page) and #68 (changelog page + stack positioning).

## What Changes

1. **CLI reference page**: New page at `content/docs/reference/cli.md` listing all current command groups with subcommands, flags, and brief descriptions — sourced from `uf --help` output at implementation time.

2. **Stack table update**: Add `uf` CLI as a fourth layer in the stack table on `content/docs/getting-started/_index.md` and `content/docs/getting-started/quick-start.md` — positioned above the existing three tools as the glue layer. Update "Three Tools" heading/prose to reflect the 4-layer model.

3. **Changelog page**: New page at `content/docs/changelog/` tracking the current release with user-facing changes.

4. **Code review persona clarification**: Add a note to the Code Review persona table in `content/docs/getting-started/common-workflows.md` clarifying that content personas (Scribe, Herald, Envoy) are invoked separately and do not participate in code review.

## Capabilities

### New Capabilities
- `docs/reference/cli`: Complete CLI reference page with all command groups and subcommands
- `docs/changelog/`: Release changelog page tracking user-facing changes per version

### Modified Capabilities
- `docs/getting-started/_index.md`: Stack table and heading updated to include `uf` CLI layer (4 layers)
- `docs/getting-started/quick-start`: Stack table and prose updated to include `uf` CLI layer
- `docs/getting-started/common-workflows.md`: Clarifying note added to Code Review persona table
- `config/_default/menus/menus.en.toml`: Navigation updated for new reference and changelog sections

### Removed Capabilities
- None

## Impact

- 2 new content pages created (CLI reference, changelog)
- 3 existing pages modified (getting-started index, quick-start, common-workflows)
- Navigation menu updated for Reference and Changelog sections
- This change is a foundation for subsequent command-specific docs (#57-#61)
- GitHub issues: [#62](https://github.com/unbound-force/website/issues/62), [#68](https://github.com/unbound-force/website/issues/68)

## Constitution Alignment

Assessed against the Unbound Force website project constitution (`.specify/memory/constitution.md`).

### I. Content Accuracy

**Assessment**: PASS

CLI reference content will be sourced from actual `uf --help` output and upstream repository source code at implementation time. Changelog entries will reflect actual shipped releases only, derived from git tags and release notes. A version marker on the CLI reference page will make staleness detectable.

### II. Minimal Footprint

**Assessment**: PASS

New pages use standard Markdown content with Doks built-in sidebar navigation. No custom layouts, CSS, or JavaScript required. The changelog introduces an ongoing maintenance commitment — this is justified by GitHub issue #68 and serves Visitor Clarity by answering "what's new?" Changelog updates will be part of the release process; if the changelog falls more than 2 releases behind, it should be removed rather than left stale.

### III. Visitor Clarity

**Assessment**: PASS

The CLI reference provides a single discoverable landing page for "what can `uf` do?" — currently this information is scattered across getting-started pages. The changelog answers "what's new?" for returning visitors. The 4-layer stack table clarifies the tool hierarchy by making the CLI's role as the entry point explicit. The new Reference navigation section improves discoverability for command-level documentation.
