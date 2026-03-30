# Dewey Documentation Gap Analysis

**Date**: 2026-03-30
**Source**: Automated analysis via Dewey semantic search + manual cross-reference
**Dewey version**: v1.4.2 (current release) + unreleased 006-unified-ignore (merged to main)
**Website docs written**: 2026-03-25/26

## Pages Analyzed

| Page | Path | Purpose |
|------|------|---------|
| Project page | `content/docs/projects/dewey.md` | Feature overview, installation, architecture |
| Getting started | `content/docs/getting-started/knowledge.md` | Installation, source config, OpenCode integration |
| Team/tool page | `content/docs/team/dewey.md` | Query capabilities, embedding model, hero usage |

## Out of Date

### HIGH Severity

**1. No mention of .gitignore support or sources.yaml ignore/recursive fields**
- **Affects**: All three pages
- **What changed**: Spec 006-unified-ignore (merged 2026-03-30) added automatic `.gitignore` respect across all filesystem walkers, plus `ignore` (pattern list) and `recursive` (boolean) fields for disk sources in `sources.yaml`.
- **Impact**: Users with `node_modules/`, `vendor/`, or other gitignored directories won't know dewey automatically skips them. Users won't know they can add custom ignore patterns or disable recursive indexing.
- **Fix**: Update Local Disk source section in getting-started. Add `ignore` and `recursive` fields to YAML examples. Note that `.gitignore` is respected automatically.

**2. Source config YAML examples missing new fields**
- **Affects**: `content/docs/getting-started/knowledge.md` (Configure Content Sources section)
- **What changed**: Disk sources now support `ignore: [pattern1, pattern2]` and `recursive: true|false` in `sources.yaml`.
- **Current text**: "indexes all Markdown files in your repository, including hidden directories" — this is no longer accurate when `.gitignore` exists.
- **Fix**: Update the Local Disk section and add a complete YAML reference showing all disk source fields.

### MEDIUM Severity

**3. Missing `dewey doctor` command**
- **Affects**: `content/docs/getting-started/knowledge.md`
- **What changed**: `dewey doctor` was added in v1.1.0 and significantly enhanced in v1.3.0 and v1.4.2 (emoji markers, summary box, uf-doctor-style output, verbose ignore reporting).
- **Impact**: Users troubleshooting dewey don't know the primary diagnostic tool exists.
- **Fix**: Add `dewey doctor` to the Initialize Your Repository section alongside `dewey status`.

**4. Missing `dewey reindex` command**
- **Affects**: `content/docs/getting-started/knowledge.md`
- **What changed**: `dewey reindex` was added in v1.2.0 for clean index rebuilds (deletes graph.db and re-indexes from scratch).
- **Impact**: Users with corrupted or stale indexes don't know how to rebuild.
- **Fix**: Add `dewey reindex` to the Updating Sources section with a note about when to use it.

**5. Missing `--verbose`, `--log-file`, and global CLI flags**
- **Affects**: `content/docs/getting-started/knowledge.md`
- **What changed**: v1.3.0 added `--verbose` (`-v`), `--log-file PATH`, `--no-embeddings`, and `--vault PATH` global flags. Auto-logging to `.dewey/dewey.log` was also added.
- **Impact**: Users troubleshooting startup issues or MCP timeouts can't find the debugging tools.
- **Fix**: Add a "Troubleshooting" or "Debugging" section documenting these flags.

### LOW Severity

**6. No version numbers anywhere**
- **Affects**: All three pages
- **What changed**: Dewey has had 10 releases (v0.5.0 through v1.4.2) since initial docs.
- **Fix**: Add a "Current Version" note or link to the GitHub releases page.

**7. Embedding chunk size reduction not documented**
- **Affects**: `content/docs/team/dewey.md` (Embedding Model table)
- **What changed**: v1.4.1 reduced `maxChunkChars` from 2000 to 1000 to prevent Ollama context overflow with granite-embedding:30m's 512-token window.
- **Fix**: Minor — could add a note about chunk sizing, but this is an internal detail.

**8. "Startup is near-instant" claim now accurate but unexplained**
- **Affects**: `content/docs/projects/dewey.md` (Persistent SQLite Index section)
- **What changed**: This was aspirational until the ignore fix. With `.gitignore` respect, startup IS fast — but only because junk directories are skipped.
- **Fix**: Could note that `.gitignore` respect enables fast startup for projects with large dependency directories.

## Missing Documentation

### HIGH Severity

**9. No CLI reference page**
- **What's needed**: A dedicated page documenting all dewey commands and their flags:
  - `dewey serve` — start the MCP server
  - `dewey index` — index external sources
  - `dewey reindex` — clean rebuild
  - `dewey status` — show index health
  - `dewey doctor` — comprehensive diagnostics
  - `dewey init` — initialize `.dewey/` directory
  - `dewey search` — CLI keyword search
  - `dewey source add` — add a content source
  - `dewey journal` — create/query journal entries
  - `dewey add` — add content to pages
- Each command has flags (e.g., `--source`, `--force`, `--vault`) that users need.

**10. No .gitignore / ignore behavior documentation**
- **What's needed**: Explain how dewey decides what to index and what to skip:
  - Hidden directories (`.git/`, `.obsidian/`, `.dewey/`) always skipped
  - `.gitignore` at source root is automatically respected
  - `sources.yaml` `ignore` field adds additional patterns (union merge)
  - `recursive: false` restricts indexing to top-level files only
  - Pattern syntax matches `.gitignore` (directory names, globs, negation, comments)

### MEDIUM Severity

**11. No sources.yaml field reference**
- **What's needed**: Complete reference for all `sources.yaml` fields:
  - `id`, `type`, `name`, `config`, `refresh_interval`
  - Disk-specific: `path`, `ignore`, `recursive`
  - GitHub-specific: `org`, `repos`
  - Web-specific: `urls`, `depth`
  - Default values and validation rules

**12. No dewey doctor guide**
- **What's needed**: What each doctor section checks:
  - Environment (vault path, dewey binary)
  - Workspace (.dewey/ directory, config files, sources.yaml)
  - Database (graph.db health, page/block/embedding counts)
  - Sources in Database (per-source page counts)
  - Embedding Layer (Ollama availability, model status)
  - MCP Server (lock file, opencode.json)
  - Summary box interpretation

**13. No troubleshooting page**
- **What's needed**: Common issues and fixes:
  - MCP server timeout → check `.gitignore`, run `dewey reindex`
  - Ollama not running → `ollama serve` or install Ollama
  - Lock file conflicts → only one dewey serve per vault
  - Low embedding coverage → run `dewey index` to generate embeddings
  - Debugging flags: `--verbose`, `--log-file`, `dewey doctor`

**14. Global CLI flags not on website**
- **What's needed**: Document flags available on all commands:
  - `--verbose` / `-v` — enable debug logging
  - `--log-file PATH` — write logs to file
  - `--no-embeddings` — skip embedding generation
  - `--vault PATH` — specify vault directory

## Accurate (No Changes Needed)

- Architecture section (project page)
- graphthulhu relationship (team page)
- Embedding model details and table (team page)
- Graceful degradation table (getting-started)
- Hero usage table (team page)
- OpenCode integration JSON config (getting-started)
- Three content source type descriptions (all pages)
- Query capabilities tables (team page)

## Recommended Priority

1. **Update getting-started page** — `.gitignore` behavior, `ignore`/`recursive` source config, `dewey doctor`, `dewey reindex`, global flags, troubleshooting section
2. **Add CLI reference page** — all commands and flags in one place
3. **Update project page** — current version, ignore support mention
4. **Update team page** — minor (chunk size note, version)

## Dewey Releases Since Docs Were Written

| Version | Date | Key Changes |
|---------|------|-------------|
| v1.0.0 | 2026-03-22 | Unified content serve, external source loading |
| v1.0.1 | 2026-03-22 | CLI search rewrite, filepath.Abs fix |
| v1.1.0 | 2026-03-23 | `dewey doctor`, `--verbose`, `--no-embeddings`, UUID collision fix |
| v1.2.0 | 2026-03-23 | `dewey reindex`, hard error on persistence failures |
| v1.3.0 | 2026-03-24 | Auto-logging, `--log-file`, enhanced doctor diagnostics |
| v1.3.1 | 2026-03-24 | Fix external page deletion on serve startup |
| v1.4.0 | 2026-03-24 | `resolveVaultPath()`, regression tests, `--vault` flag, log truncation |
| v1.4.1 | 2026-03-25 | Embedding chunk size 2000→1000 |
| v1.4.2 | 2026-03-30 | Doctor emoji markers, lipgloss bold headings, fix hint correction |
| *main* | 2026-03-30 | Spec 006: unified .gitignore + sources.yaml ignore support |
