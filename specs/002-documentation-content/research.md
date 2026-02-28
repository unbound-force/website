# Research: Documentation Content Pages

**Branch**: `002-documentation-content` | **Date**: 2026-02-27 | **Spec**: [spec.md](spec.md) | **Plan**: [plan.md](plan.md)

## R1: Getting Started Section — Source Content

**Decision**: Adapt content from `unbound-force/unbound-force.md` overview section for the "What is Unbound Force?" index page. The Quick Start page will synthesize information across the three tools (Specify, OpenCode, Swarm) from their respective sources.

**Rationale**: The `unbound-force.md` file contains two sections: "Overview" and "The Heroes." The overview describes Unbound Force as "a set of personas and roles for a software agent swarm" themed as a superhero team. Each hero is not just a markdown file — they can include LSP servers, MCP servers, tooling, tasks, commands, plugins, and other technologies. The three tools are:

| Tool               | Source                       | Role                                                                                           |
| ------------------ | ---------------------------- | ---------------------------------------------------------------------------------------------- |
| Specify (spec-kit) | `github.com/github/spec-kit` | Specification pipeline — structured workflow for specs, plans, and tasks before implementation |
| OpenCode           | `opencode.ai`                | AI coding environment the personas run inside                                                  |
| Swarm              | `swarmtools.ai`              | OpenCode plugin enabling learning amongst team members                                         |

The Quick Start page must cover all three tools equally (per clarification decision). Research into each tool's documentation reveals a layered stack model:

| Layer            | Tool     | Install                                                                                                                     | Role                                                                                |
| ---------------- | -------- | --------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| **Agent**        | OpenCode | `curl -fsSL https://opencode.ai/install \| bash`                                                                            | AI coding agent runtime — where you interact, write code, run commands              |
| **Planning**     | Specify  | `uv tool install specify-cli --from git+https://github.com/github/spec-kit.git` then `specify init <project> --ai opencode` | Spec-driven development — turns ideas into specs, plans, and tasks                  |
| **Coordination** | Swarm    | `npm install -g opencode-swarm-plugin@latest` then `swarm setup`                                                            | Multi-agent parallelism — spawns workers, coordinates via file reservations, learns |

The Quick Start page will present these as complementary layers with a "why should I care" framing rather than raw installation commands. Each tool is independently useful but they compose into the full Unbound Force workflow.

**Alternatives considered**:

- Copying the overview verbatim: Rejected — Constitution Principle III requires audience adaptation, and Principle II says not to copy READMEs verbatim.
- Deferring Quick Start until each tool has a dedicated page: Rejected — FR-002 requires the Quick Start page in this spec.

## R2: Gaze Project Page — `/gaze` OpenCode Command Focus

**Decision**: Present Gaze as an OpenCode-integrated tool. The primary workflow is: install Gaze binary -> run `gaze init` in your Go project -> use `/gaze` within OpenCode. No CLI commands documented on the page.

**Rationale**: Research into the Gaze repository reveals the following OpenCode integration:

**Setup flow:**

1. Install Gaze: `brew install unbound-force/tap/gaze` or `go install github.com/unbound-force/gaze/cmd/gaze@latest`
2. Run `gaze init` in any Go project — scaffolds 4 files into `.opencode/`:
   - `.opencode/agents/gaze-reporter.md` — quality report agent
   - `.opencode/agents/doc-classifier.md` — document-enhanced classifier agent
   - `.opencode/command/gaze.md` — `/gaze` slash command
   - `.opencode/command/classify-docs.md` — `/classify-docs` slash command

**Usage (inside OpenCode):**

```
/gaze                           # Full report for entire module
/gaze crap ./internal/store     # CRAP scores only
/gaze quality ./pkg/api         # Contract Coverage metrics only
```

**Three modes:**

| Mode           | What You Get                                                                |
| -------------- | --------------------------------------------------------------------------- |
| Full (default) | CRAP Summary + Quality Summary + Classification Summary + Health Assessment |
| `crap`         | CRAPload count, top 5 worst scores, GazeCRAP quadrant distribution          |
| `quality`      | Contract coverage, gaps, over-specification, worst tests                    |

**How it works**: The `/gaze` command routes to the `gaze-reporter` agent, which resolves the binary, runs CLI commands with `--format=json`, interprets the output, and produces human-readable markdown summaries with tables, key metrics, and actionable recommendations. Graceful degradation if any command fails.

**Also available**: `/classify-docs` — enhances classification by combining mechanical signals with project documentation analysis.

**Key metrics to highlight on the page:**

- Contract Coverage (% of contractual side effects asserted on)
- Over-Specification Score (count of incidental assertions)
- GazeCRAP (composite risk score using contract coverage instead of line coverage)

**Cross-link**: Page will link to `/blog/why-contract-coverage/` for the deep technical explanation.

**Alternatives considered**:

- Documenting CLI commands directly: Rejected — user explicitly chose to focus on the OpenCode `/gaze` command only.
- Documenting both OpenCode and CLI: Rejected — same reason.

## R3: Team Persona Pages — Source Content and Images

**Decision**: Adapt each persona description from `unbound-force/unbound-force.md` "The Heroes" section. Each persona page displays its trading card image. Team index displays the composite image.

**Rationale**: The source file contains detailed descriptions for all 5 personas:

| Persona         | Role          | Tagline                                                    | Image File         |
| --------------- | ------------- | ---------------------------------------------------------- | ------------------ |
| Muti-Mind       | Product Owner | "The Vision Keeper and Prioritization Engine"              | `multi-mind.png`   |
| Cobalt-Crush    | Developer     | "The Engineering Core and Adaptive Implementation Engine"  | `cobalt-crush.png` |
| Gaze            | Tester        | "The Quality Sentinel and Predictive Validation Engine"    | `gaze.png`         |
| The Divisor     | PR Reviewer   | "The Architectural Conscience and Code Integrity Guardian" | `the-divisor.png`  |
| Mx F (Mx Found) | Manager       | "The Flow Facilitator and Continuous Improvement Coach"    | `mx-f.png`         |

Each persona has well-defined key responsibilities, interaction patterns, and a "source of truth for X" framing. Content will be rewritten for a website audience — less raw role specification, more "what does this agent do for you."

The Divisor is unique: it operates as a **council of three sub-personas** (The Guard, The Architect, The Adversary) with collective approval required for merges. This must be highlighted.

**Image usage**: Trading card images are already in `static/images/team/`. In Markdown, they'll be referenced as `![Alt text](/images/team/filename.png)`. The team composite image (`the-unbound-force.jpeg`) goes on the index page, possibly also `the-unbound-force-small.jpeg` for responsive sizing.

**Alternatives considered**:

- Using Doks image shortcode instead of standard Markdown images: Worth exploring during implementation if the shortcode provides responsive/optimized images. Defer to implementation.
- Creating custom page templates for persona cards: Rejected — Constitution Principle II (Minimal Footprint). Standard Markdown with images is sufficient.

## R4: Contributing Guide — Content Sources

**Decision**: Synthesize contributing guidelines from the project's actual conventions documented in AGENTS.md and the Speckit workflow.

**Rationale**: There is no standalone `CONTRIBUTING.md` in the website repo. The contributing page must cover:

1. **Reporting issues** — link to GitHub issues
2. **Submitting pull requests** — standard GitHub PR flow
3. **Conventional Commits** — `feat:`, `fix:`, `docs:`, `chore:`, `style:`, `refactor:`, `ci:`
4. **Speckit workflow** — all non-trivial work goes through the pipeline: constitution -> specify -> clarify -> plan -> tasks -> implement
5. **Links** — to the GitHub org and individual repositories

This content is straightforward and doesn't require upstream source verification beyond confirming the Conventional Commits types and Speckit pipeline stages already documented in AGENTS.md.

**Alternatives considered**:

- Linking to a CONTRIBUTING.md in each repo: No such files exist currently. The website page serves as the canonical contributing guide.

## R5: Navigation and Sidebar Configuration

**Decision**: The `[[docs]]` menu entries already exist in `menus.en.toml` from spec 001. No navigation configuration changes are needed.

**Rationale**: The current `menus.en.toml` already defines all 4 docs sections:

- Getting Started (weight 10)
- Projects (weight 20)
- The Team (weight 30)
- Contributing (weight 40)

These were added during the site scaffold (spec 001). FR-014 is already satisfied. The only remaining work is creating the content pages themselves with correct `weight` values in their frontmatter for intra-section ordering.

**Alternatives considered**:

- Restructuring the navigation weights: Unnecessary — the current ordering matches the spec requirements.

## R6: Image Display in Hugo/Doks

**Decision**: Use standard Markdown image syntax (`![alt](/images/team/filename.png)`) for trading card images in persona pages.

**Rationale**: Research confirms that Doks has a **render hook** for standard Markdown images. The `@thulite/images` integration automatically processes `![alt](src)` images — converting to WebP, generating responsive `sizes`, adding LQIP blur-up placeholders, and setting `loading`/`decoding`/`fetchpriority` attributes based on config defaults. This means standard Markdown syntax gets full image optimization without any shortcodes.

Images in `static/` are served at their path (e.g., `static/images/team/multi-mind.png` → `/images/team/multi-mind.png`). Doks also provides `{{< picture >}}`, `{{< figure >}}`, and `{{< img >}}` shortcodes for more control (captions, explicit sizing), but these add complexity without benefit since the spec does not require captions.

Constitution Principle II (Minimal Footprint) confirms: standard Markdown images are the correct choice.

**Alternatives considered**:

- `{{< figure >}}` shortcode for captions: Rejected — spec doesn't require captions on trading card images.
- Page bundles with co-located images: Rejected — images are shared resources in `static/`, not page-specific.
- Hugo image processing (`assets/` resources): Rejected — render hook already provides optimization for `static/` images.

## R7: Gaze Project Page — Known Limitations

**Decision**: Include an honest limitations section on the Gaze project page, sourced from the Gaze README.

**Rationale**: Per FR-005, the page must not fabricate features or omit known limitations. The current limitations are:

1. **Direct function body only** — no transitive side effects (effects from called functions) in v1
2. **P3-P4 side effects not yet detected** — taxonomy defined but detection not implemented
3. **Assertion mapping accuracy ~78.8%** — target is 90%, tracked as GitHub Issue #6
4. **No CGo or unsafe analysis** — functions using cgo or unsafe.Pointer are skipped
5. **Single package loading** — `analyze` processes one package at a time

These should be framed honestly but constructively — "current scope" rather than "failures." The P0-P2 tiers cover the most common and impactful side effects; P3-P4 are lower-priority edge cases.

**Alternatives considered**:

- Omitting limitations: Rejected — violates Constitution Principle I (Content Accuracy) and FR-005.
- Listing all limitations with full technical detail: Rejected — website audience framing. Brief, honest, constructive.

## R8: "Get Started" Button and Docs-Level Index

**Decision**: Re-enable the "Get Started" navbar button in `params.toml` and create `content/docs/_index.md` as the docs section root.

**Rationale**: The `params.toml` navbar button is commented out with `"until docs content exists (spec 002)"`. This feature creates that content, so the button should be re-enabled to point to `/docs/getting-started/`.

Research confirms that Doks requires `content/docs/_index.md` for Hugo to treat `docs/` as a section root. Without it, the sidebar auto-generation and section navigation will not work. This is a standard Hugo content-as-structure requirement, not Doks-specific.

The docs-level `_index.md` counts as one of the 12 pages in SC-001: "2 Getting Started (index + Quick Start), 2 Projects (index + Gaze), 6 Team (index + 5 personas), 1 Contributing (index), plus the docs-level `_index.md`."

**Alternatives considered**:

- Leave button disabled until merge: Rejected — the button is needed for the two-click navigation requirement (SC-002) and must be testable during development.
- Skip docs-level `_index.md`: Not an option — Hugo requires it for section recognition.
