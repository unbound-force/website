# Research: Dewey v3.0.0 Documentation Sync

**Date**: 2026-04-11  
**Feature**: [spec.md](spec.md) | [plan.md](plan.md)

## R1: Dewey MCP Tool Count (v3.0.0)

**Decision**: Use "48 MCP tools across 12 categories" as the canonical count.

**Source**: Upstream Dewey PR #42 (spec 013-knowledge-compile). The tool count increased from 40 to 48 with the addition of 8 new tools across 2 new categories.

**Previous baseline** (established in spec 010 research R3): "40 tools across 10 categories."

**New categories and tools added in v3.0.0**:

| New Category         | Tools                                          | Purpose                                                                                                           |
| -------------------- | ---------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| Knowledge Management | `compile`, `lint`, `promote`                   | Knowledge lifecycle: compile learnings into articles, lint for quality issues, promote draft content to validated |
| Learning             | `store_learning`, `index`, `reindex`, `reload` | Store learnings, manage indexes                                                                                   |

The exact category breakdown needs to be verified against the Dewey v3.0.0 `server.go` at implementation time. The table above lists 7 unique new tools (3 + 4), but the delta from 40 to 48 implies 8 new tools — the discrepancy may be due to tools not listed here or category reclassification. The spec states 48 tools; the website should use the number stated in the spec and verified against the upstream README or server.go.

**Pattern from spec 010**: Spec 010 research R3 established the pattern of using the README header as the authoritative human-facing statement. The same pattern applies here — use the number from the spec/README, not a manual count of tool registrations.

**Current website mentions to update**:

- `content/docs/projects/dewey.md` line 17: "40 MCP tools across 10 categories"
- `content/docs/projects/dewey.md` line 38: heading "### 40 MCP Tools Across 10 Categories"
- `content/docs/projects/dewey.md` line 40: body text "40 MCP tools across 10 categories"
- `content/docs/team/dewey.md` line 35: "40 MCP tools across 10 categories"
- `content/blog/dewey-vs-karpathy.md` line 52: "40 tools over MCP's stdio JSON-RPC protocol" — **out of scope** per spec (blog content excluded)

**Rationale**: The spec explicitly states the tool count changed from 40 to 48. All website pages must be updated to reflect this. The blog post is out of scope per the spec's "Out of Scope" section (blog content is covered by issue #30 as a separate spec).

**Alternatives considered**: Leaving the blog post count at 40 was accepted because the spec explicitly excludes blog content changes.

## R2: Content Source Types (v3.0.0)

**Decision**: Update from "three" to "four" content source types. The fourth type is "code" (Go AST parsing).

**Source**: Upstream Dewey PR #32 (code source indexing) and PR #42 (v3.0.0 release). The code source type was added in PR #32 and is part of the v3.0.0 release.

**The four source types are**:

1. **Local disk** — indexes Markdown files in your repository
2. **GitHub API** — fetches issues, PRs, READMEs from whitelisted repositories
3. **Web crawl** — indexes documentation from toolstack websites
4. **Code** — Go AST parsing to extract function signatures, CLI commands, MCP tool registrations, and package documentation

**Current website mentions to update**:

- `content/docs/projects/dewey.md` line 46: heading "### Three Content Source Types" → "### Four Content Source Types"
- `content/docs/projects/dewey.md` lines 48-51: three bullet points → add fourth for code
- `content/docs/projects/dewey.md` line 75: "disk, GitHub, and web crawl" → add code
- `content/docs/team/dewey.md` line 72: "three pluggable source types" → "four pluggable source types"
- `content/docs/team/dewey.md` lines 76-84: three subsections → add Code subsection
- `content/docs/getting-started/knowledge.md` line 147: "three pluggable source types" → "four pluggable source types"
- `content/docs/getting-started/knowledge.md`: needs a Code source configuration section

**Note**: The blog post (`dewey-vs-karpathy.md` line 43) already says "Four source types" and includes the code source — it was written after PR #32 landed. No blog changes needed.

**Rationale**: The code source type is a significant capability addition. Visitors evaluating Dewey need to know it can index source code, not just Markdown.

## R3: Knowledge Compilation, Linting, and Promotion

**Decision**: Document compile, lint, and promote as three new CLI commands and MCP tools that form the knowledge lifecycle.

**Source**: Upstream Dewey PR #42 (spec 013-knowledge-compile).

**Compile** (`dewey compile`):

- Clusters stored learnings by topic using semantic similarity
- Synthesizes clusters into current-state articles via LLM
- Raw learnings are an append-only event log; compiled articles are materialized views rebuilt from learnings
- MCP tool: `dewey_compile`

**Lint** (`dewey lint`):

- Automated quality checks that detect stale decisions, uncompiled learnings, embedding gaps, and contradictions
- Some checks are mechanically fixable; others require compilation
- MCP tool: `dewey_lint`

**Promote** (`dewey promote`):

- Moves content between trust tiers (draft → validated → authored)
- Requires human review for promotion to authored tier
- MCP tool: `dewey_promote`

**Documentation approach**:

- **Project page** (`dewey.md`): Add a "Knowledge Lifecycle" section describing compile, lint, and promote at evaluator level (what they do, why they matter)
- **Knowledge guide** (`knowledge.md`): Add CLI command documentation with usage descriptions
- **Team page** (`dewey.md`): Add compile, lint, and promote to the MCP tools table

## R4: Trust Tiers

**Decision**: Document the three trust tiers as a quality classification system for stored knowledge.

**Source**: Upstream Dewey PR #42 (spec 013-knowledge-compile), spec 020 Key Entities section.

**Three trust tiers**:

| Tier          | Meaning                             | How Content Gets This Tier                                                    |
| ------------- | ----------------------------------- | ----------------------------------------------------------------------------- |
| **Authored**  | Human-written content               | Content created directly by humans (specs, READMEs, design docs)              |
| **Validated** | Machine-generated, human-reviewed   | Content generated by agents and approved by a human via `dewey promote`       |
| **Draft**     | Machine-generated, not yet reviewed | Content stored by agents via `store_learning` or generated by `dewey compile` |

**Impact on search**: Trust tiers affect search result ranking and filtering. The `dewey_semantic_search_filtered` tool accepts a `tier` parameter for trust-tier filtering. Higher-tier content ranks above lower-tier content in search results.

**Documentation approach**: Add a "Trust Tiers" section to the knowledge guide (`knowledge.md`) explaining the three tiers, their meaning, and how they affect search results.

## R5: `store_learning` API Changes (v3.0.0)

**Decision**: Document the updated `store_learning` tool parameters.

**Source**: Upstream Dewey PR #42 (spec 013-knowledge-compile). The API changed from the original implementation (PR #27, issue #25).

**Parameter changes**:

| Parameter     | Before (v2.x)                      | After (v3.0.0)                                                     |
| ------------- | ---------------------------------- | ------------------------------------------------------------------ |
| `information` | Required (string)                  | Required (string) — unchanged                                      |
| `tags`        | Optional (string, comma-separated) | **Removed**                                                        |
| `tag`         | N/A                                | **Required** (string, singular)                                    |
| `category`    | N/A                                | **Optional** (enum: decision, pattern, gotcha, context, reference) |

**Response format**: Returns a `{tag}-{sequence}` identity string (e.g., `gotcha-003`).

**Semantic search results metadata**: Results now include `created_at`, `category`, and `tier` fields.

**Documentation approach**: Add `store_learning` parameter documentation to the knowledge guide. Include the response format and the new metadata fields in search results.

**Note**: The `store_learning` tool is currently mentioned conceptually in the blog post but not documented with parameters anywhere on the site. This is the first time its API will be documented on the website.

## R6: Web Crawler Bug Fix

**Decision**: Add a single sentence noting that complex documentation sites (e.g., pkg.go.dev) are now supported.

**Source**: Issue #29 (web crawler reliability improvements).

**Documentation approach**: Add a note to the web source section of the knowledge guide. This is a minor addition — one sentence confirming that complex sites are supported.

**Current state**: The web source section already describes web crawling as working correctly. The bug was in the code, not the docs. The note is a confidence-building addition, not a correction.
