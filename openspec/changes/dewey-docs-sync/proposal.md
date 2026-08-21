## Why

The Dewey documentation on the Unbound Force website is significantly out of date. Twelve upstream changes spanning v3.1.0 and v3.2.0 have shipped in the `unbound-force/dewey` repository without corresponding website documentation updates. Users visiting the website encounter stale tool counts (48 instead of 50), missing installation methods (RPM, fixed Homebrew), undocumented environment variables (`DEWEY_CHUNK_MAX_CHARS`, `DEWEY_SYNTHESIS_ENDPOINT`, `DEWEY_AUTHOR`), missing provider configuration (Vertex AI alongside Ollama), and no mention of major features like curated knowledge stores, content sanitization, or the `dewey doctor` synthesis diagnostics.

This creates a trust gap: the website claims to be the authoritative documentation, but users who install the latest release discover capabilities and configuration options that aren't documented anywhere on the site.

Tracked by: #207, #208, #209, #213, #240, #243, #249, #186, #132, #131, #41, #113.

## What Changes

Synchronize three existing documentation pages to reflect the current state of Dewey (through v3.2.0):

1. **`content/docs/projects/dewey.md`** — Update tool count from 48 to 50, add curated knowledge stores and pluggable providers to feature list, add RPM to installation options, mention content sanitization.

2. **`content/docs/getting-started/knowledge.md`** — Add RPM installation section, document `DEWEY_CHUNK_MAX_CHARS` and `DEWEY_SYNTHESIS_ENDPOINT` environment variables, add Vertex AI provider configuration alongside Ollama, document `region: global` endpoint behavior, update `dewey doctor` output to include synthesis layer diagnostics, add content sanitization configuration (`sanitize_mode`, `trust_tier`), document curated knowledge stores (`dewey curate`, `knowledge-stores.yaml`), update learning identity format to v3.2.0 timestamped format, add `DEWEY_AUTHOR` env var, note Homebrew cask fix.

3. **`content/docs/team/dewey.md`** — Update tool count from 48 to 50, add `curate` and `store_compiled` tools to the tool catalog, update `max_chunk_chars` documentation with the `DEWEY_CHUNK_MAX_CHARS` env var, mention pluggable providers.

## Capabilities

### New Capabilities
- `RPM installation docs`: Fedora/RHEL/CentOS installation instructions with `dnf install` workflow
- `Vertex AI provider configuration`: Complete setup guide for Vertex AI as an alternative to Ollama, including `gcloud` auth, project/region config, and `region: global` endpoint behavior
- `Content sanitization reference`: Documentation of the 4-layer sanitization pipeline, pattern catalog, severity classifications, and per-source configuration
- `Curated knowledge stores reference`: Documentation of `dewey curate`, `knowledge-stores.yaml`, the `curated` trust tier, and background curation
- `Synthesis endpoint configuration`: `DEWEY_SYNTHESIS_ENDPOINT` env var documentation with precedence chain explanation
- `Doctor synthesis diagnostics`: Updated `dewey doctor` output reference showing the synthesis layer section

### Modified Capabilities
- `Tool count`: Updated from 48 to 50 across all pages that reference it
- `Installation section`: Homebrew cask fix noted, RPM added between Homebrew and `go install`
- `Environment variables reference`: Added `DEWEY_CHUNK_MAX_CHARS`, `DEWEY_SYNTHESIS_ENDPOINT`, `DEWEY_AUTHOR`
- `Learning identity format`: Updated examples to v3.2.0 timestamped format with migration note
- `Embedding configuration`: Added `DEWEY_CHUNK_MAX_CHARS` and `embedding.max_chunk_chars` documentation

### Removed Capabilities
- None. All changes are additive or corrective.

## Impact

**Affected files:**
- `content/docs/projects/dewey.md` — Feature list, tool count, installation options
- `content/docs/getting-started/knowledge.md` — Installation, env vars, provider config, doctor output, sanitization, curation, learning format
- `content/docs/team/dewey.md` — Tool catalog, tool count, embedding config

**No structural changes:**
- No new pages created (all content fits within existing page structure)
- No navigation changes needed
- No layout or template changes
- No SCSS/CSS changes

**Cross-page consistency:** Tool count must be updated consistently across all three pages (48 → 50). The `curate` tool (from #41) brings it to 49, and `store_compiled` (from #113) brings it to 50.

## Constitution Alignment

Assessed against the Unbound Force org constitution.

### I. Autonomous Collaboration

**Assessment**: N/A

This change updates static documentation content on the website. It does not modify artifact-based communication protocols, tool interfaces, or agent interaction patterns. Documentation pages are self-contained Markdown files that do not introduce runtime coupling.

### II. Composability First

**Assessment**: N/A

No new dependencies are introduced. The website remains a standalone Hugo static site. The documentation updates describe Dewey features but do not create mandatory dependencies between the website and Dewey's runtime.

### III. Observable Quality

**Assessment**: PASS

The documentation updates improve observable quality by accurately documenting Dewey's current capabilities, configuration options, and diagnostic outputs. Users can verify the documentation against their installed version. Content is sourced from upstream GitHub issues with traceable provenance (#207, #208, #209, #213, #240, #243, #249, #186, #132, #131, #41, #113).

### IV. Testability

**Assessment**: PASS

This change modifies documentation content only. The project has no test suite (AGENTS.md). Verification strategy is defined in tasks group 4: `npm run build` (zero errors), visual inspection of all three pages, cross-page consistency checks for shared facts (tool count, provider terminology, `DEWEY_CHUNK_MAX_CHARS`), and constitution alignment verification. All acceptance scenarios use GIVEN/WHEN/THEN format verifiable through manual inspection of rendered pages.

### V. Security by Default

**Assessment**: N/A

This change documents existing security features (content sanitization) but does not modify any security-sensitive code, dependencies, or CI configuration. The documentation accurately describes sanitization capabilities without introducing new attack surface.
