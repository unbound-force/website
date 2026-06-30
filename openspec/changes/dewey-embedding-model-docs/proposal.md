## Why

Dewey's `upgrade-embedding-model-r2` change (tracked in [dewey#53](https://github.com/unbound-force/dewey/issues/53)) introduces new configuration options and diagnostic behavior that must be reflected in the website documentation. Without this sync, the website will show outdated information -- users configuring `DEWEY_CHUNK_MAX_CHARS` or `max_chunk_chars` will find no documentation, and users running `dewey doctor` will see an advisory about legacy models that the docs don't explain.

This is a documentation-only change driven by [website#166](https://github.com/unbound-force/website/issues/166).

## What Changes

Update three Dewey documentation pages to reflect the embedding model upgrade:

1. **Environment variables** -- add `DEWEY_CHUNK_MAX_CHARS` (default: 12288) to the appropriate documentation section
2. **Provider configuration** -- add `max_chunk_chars` field to the `embedding` config YAML example
3. **Doctor diagnostics** -- document the new legacy model advisory that appears when `granite-embedding:30m` is configured
4. **Semantic search setup** -- note that the default model remains `granite-embedding:30m` for now, with R2 as a future upgrade path

## Capabilities

### New Capabilities
- `DEWEY_CHUNK_MAX_CHARS documentation`: Environment variable reference for the new chunk size override
- `max_chunk_chars config documentation`: Configuration field reference for embedding chunk size limits
- `Legacy model advisory documentation`: Explanation of the `dewey doctor` informational note about Granite Embedding R2

### Modified Capabilities
- `Embedding Model section`: Updated to mention chunk size configurability and the R2 upgrade path
- `Doctor section`: Updated to include the legacy model advisory diagnostic

### Removed Capabilities
- None

## Impact

Three content files are affected:

- `content/docs/projects/dewey.md` -- Embedding Model section, Architecture section
- `content/docs/getting-started/knowledge.md` -- Installation section, Embedding Model Alignment section, Diagnostic Commands section
- `content/docs/team/dewey.md` -- Embedding Model section

No layout, navigation, or styling changes. No new pages created. All changes are additive content updates to existing sections.

## Constitution Alignment

Assessed against the Unbound Force website project constitution (`.specify/memory/constitution.md`).

### I. Content Accuracy

**Assessment**: PASS

All documented values (`DEWEY_CHUNK_MAX_CHARS`, `max_chunk_chars`, default `12288`, legacy model advisory behavior) are derived from the upstream Dewey change ([dewey#53](https://github.com/unbound-force/dewey/issues/53)). Design decision D5 ensures R2 is not presented as currently available, preventing feature fabrication.

### II. Minimal Footprint

**Assessment**: PASS

All changes are additive edits to existing Markdown sections. No new pages, layouts, CSS, or dependencies are introduced. Design decision D1 explicitly chooses to update existing sections rather than creating new structures.

### III. Visitor Clarity

**Assessment**: PASS

New configuration options are documented in the sections where users would naturally look for them -- env vars alongside existing env vars, config fields alongside existing config examples, doctor advisories in the doctor section. Forward-looking R2 language (D5) avoids confusing users about unavailable features.
