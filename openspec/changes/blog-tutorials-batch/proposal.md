## Why

The website has 14 open GitHub issues for content that has been planned but not yet written — 9 blog posts and 5 tutorials. Each issue contains a detailed outline with section structure, key points, and target audience. The blog section currently has 14 published posts, but the tutorials section does not exist yet. Publishing this batch will:

- Fill major content gaps in the website (tutorials section is entirely missing)
- Provide practical guides for Dewey, Gaze, Pinkman, and Unbound Force features
- Cover recent releases and capabilities (Dewey v3.1.0, Gaze baseline comparison, council-review-action, pluggable providers)
- Close 14 tracked issues in a single coordinated effort

## What Changes

### Blog Posts (9 new pages)

Each blog post follows the established pattern in `content/blog/` with standard Hugo/Doks frontmatter.

1. **CI failure classification** (#64) — How `/review-pr` separates pre-existing failures from PR-caused regressions
2. **AGENTS.md quality** (#65) — Why the quality of your AGENTS.md determines the quality of AI-generated code
3. **Dewey knowledge stores** (#69) — File-backed learnings that survive database deletion in Dewey v3.1.0
4. **Dewey slash command UX** (#72) — How a markdown file became Dewey's best feature via `/dewey-store`
5. **Pluggable LLM providers** (#116) — Dewey's cloud-optional architecture with Ollama and Vertex AI support
6. **Gaze baseline comparison** (#150) — Per-function CRAP regression detection for CI gates
7. **Council review action** (#191) — Bringing the Divisor Council to GitHub Actions for AI code review in CI
8. **Prompt hardening** (#198) — Engineering agent prompts that survive Dynamic Context Protocol compression
9. **Multi-platform support** (#51) — How `uf init` scaffolds for both OpenCode and Cursor

### Tutorials (5 new pages)

Tutorials require creating a new `content/docs/tutorials/` section with its own `_index.md` and navigation entry.

1. **Dewey knowledge stores setup** (#70) — From configuration to curated semantic search
2. **Dewey provider configuration** (#114) — Configuring embedding and synthesis providers (Ollama, Vertex AI)
3. **Gaze CRAP baseline in CI** (#151) — Setting up baseline comparison in GitHub Actions
4. **Pinkman OSS Scout** (#45) — Getting started with `/scout` discover/trend/audit/report modes
5. **Onboard customization** (#46) — Getting started with `/onboard` and the three interview phases

### Infrastructure Changes

- Create `content/docs/tutorials/_index.md` section index page
- Add tutorials navigation entry to `config/_default/menus/menus.en.toml`

## Capabilities

### New Capabilities
- `tutorials-section`: New documentation section for step-by-step guides, accessible from top-level navigation
- `blog-batch-content`: 9 new blog posts covering recent features across Gaze, Dewey, Unbound Force, and the Divisor Council

### Modified Capabilities
- `site-navigation`: Updated to include tutorials section in the docs sidebar and potentially in the top-level menu

### Removed Capabilities
- None

## Impact

- **Content files**: 15 new Markdown files (9 blog posts + 5 tutorials + 1 section index)
- **Navigation**: `config/_default/menus/menus.en.toml` updated for tutorials
- **No SCSS/layout changes**: All content uses standard Doks Markdown rendering
- **No dependency changes**: No new npm packages or Hugo modules
- **Issue tracking**: Closes GitHub issues #45, #46, #51, #64, #65, #69, #70, #72, #114, #116, #150, #151, #191, #198

## Constitution Alignment

Assessed against the Unbound Force org constitution.

### I. Autonomous Collaboration

**Assessment**: N/A

This change adds static website content (Markdown files). It does not modify any hero artifacts, agent communication protocols, or runtime coupling. Content is sourced from well-defined GitHub issue specifications.

### II. Composability First

**Assessment**: N/A

Blog posts and tutorials are standalone Markdown pages. Each can be published, updated, or removed independently. The tutorials section is a standard Hugo content directory with no custom dependencies.

### III. Observable Quality

**Assessment**: N/A

This change produces static HTML pages via Hugo's standard build pipeline. No machine-parseable output or provenance metadata is involved — these are human-readable articles.

### IV. Testability

**Assessment**: N/A

Static content changes are validated by `npm run build` (Hugo compilation succeeds) and visual verification. No runtime components are introduced that would require isolation testing.

### V. Security by Default

**Assessment**: N/A

This change adds static Markdown content files. No external inputs are processed, no dependencies are added, and no CI pipeline changes are made. Tutorial configuration examples MUST use placeholder values for secrets and MUST demonstrate SHA-pinned GitHub Actions where applicable.
