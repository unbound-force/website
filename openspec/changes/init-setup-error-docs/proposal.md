## Why

Upstream PR `unbound-force/unbound-force#269` (fixing #215) changed how `uf init` and `uf setup` report sub-tool failures. Previously, these commands showed hardcoded summary strings (e.g., "specify init failed"). Now they surface the actual error output from the failing sub-tool, giving users actionable diagnostic information without needing to re-run commands manually.

The website's CLI reference and developer guide describe `uf init` and `uf setup` but do not mention error reporting behavior. This change adds a brief note about the improved error output to both pages, keeping the documentation accurate with the shipped CLI behavior.

Tracks GitHub issue #212 (`docs`, `next-release`).

## What Changes

Two small documentation updates:

1. **CLI Reference (`content/docs/reference/cli.md`)**: Add a sentence to the `init` and `setup` command sections noting that sub-tool failures show the actual error output from the failing command.

2. **Developer Guide (`content/docs/getting-started/developer.md`)**: Add a note in the "Sub-Tool Initialization" section clarifying that if any sub-tool step fails, the actual error output is displayed to help diagnose the issue.

## Capabilities

### New Capabilities

- None

### Modified Capabilities

- `docs/reference/cli`: `init` and `setup` sections updated with error reporting behavior
- `docs/getting-started/developer`: Sub-Tool Initialization section updated with error output note

### Removed Capabilities

- None

## Impact

- 2 Markdown content pages modified
- No layout, style, or configuration changes
- No new pages created
- All changes are text additions within existing content sections

## Constitution Alignment

Assessed against the Unbound Force website project constitution (`.specify/memory/constitution.md`).

### I. Content Accuracy

**Assessment**: PASS

This change exists specifically to satisfy Content Accuracy requirements. The upstream CLI behavior changed (error output is now surfaced from sub-tools), and the website must reflect this. The added text is derived from the actual shipped behavior in `unbound-force/unbound-force#269`.

### II. Minimal Footprint

**Assessment**: PASS

Two sentences added to existing pages. No new pages, layouts, CSS, JavaScript, or dependencies. Uses existing Markdown content structure.

### III. Visitor Clarity

**Assessment**: PASS

Users who encounter a sub-tool failure during `uf init` or `uf setup` will benefit from knowing that the error output they see comes directly from the failing tool. This sets correct expectations and improves the troubleshooting experience.
