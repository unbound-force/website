## Why

The `uf` CLI has grown to 6 command groups with 15+ subcommands, but the website has no single CLI reference page. Users looking for "what can `uf` do?" have no landing page. Additionally, the website's "Three Tools" stack table (OpenCode + Speckit + Replicator) omits the CLI layer entirely, and there is no changelog or "what's new" page despite 84+ commits shipping since v0.12.0.

This change addresses GitHub issues #62 (CLI reference page) and #68 (changelog page + stack positioning).

## What Changes

1. **CLI reference page**: New page at `content/docs/reference/cli.md` listing all 6 command groups (`init`, `setup`, `doctor`, `config`, `sandbox`, `gateway`) with subcommands, flags, and brief descriptions.

2. **Stack table update**: Add `uf` CLI as a fourth layer in the stack table on `_index.md` and `quick-start.md` — positioned above the existing three tools as the glue layer.

3. **Changelog page**: New page at `content/docs/changelog/` tracking major releases with user-facing changes per version.

## Capabilities

### New Capabilities
- `docs/reference/cli`: Complete CLI reference page with all command groups and subcommands
- `docs/changelog/`: Release changelog page tracking user-facing changes per version

### Modified Capabilities
- `_index.md` (homepage): Stack table updated to include `uf` CLI layer
- `docs/getting-started/quick-start`: Stack table updated to include `uf` CLI layer
- `config/_default/menus/menus.en.toml`: Navigation updated for new reference section

### Removed Capabilities
- None

## Impact

- 2 new content pages created
- 2-3 existing pages modified (stack table updates)
- Navigation menu updated
- This change is a foundation for subsequent command-specific docs (#57-#61)

## Constitution Alignment

Assessed against the Unbound Force org constitution.

### I. Autonomous Collaboration

**Assessment**: N/A

Documentation pages do not affect artifact-based communication between heroes.

### II. Composability First

**Assessment**: PASS

The CLI reference documents standalone functionality of each command group. The changelog page is independent of other sections. No mandatory dependencies introduced.

### III. Observable Quality

**Assessment**: PASS

A CLI reference page improves discoverability and makes the tool surface observable to users. The changelog provides provenance for version-level changes.

### IV. Testability

**Assessment**: N/A

No code changes. Validation is `npm run build` + visual review.
