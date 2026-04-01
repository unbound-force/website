---
pack_id: markdown
language: Markdown
version: 1.0.0
---

# Convention Pack: Markdown (Documentation, Website Content, Blog Posts)

This is the Markdown-specific convention pack for The
Divisor PR reviewer framework. It contains rules for
writing documentation pages, website content, and blog
posts in Hugo/Doks static site projects. It complements
the language-agnostic `default.md` pack with
content-specific standards.

When this pack is active, personas load both packs --
this pack takes precedence on overlapping concerns
related to content structure, writing quality, and
Markdown formatting.

---

## Frontmatter

- **FM-001** [MUST] Every `.md` file in `content/` MUST
  have YAML frontmatter with at minimum: `title`,
  `description`, `date`, `draft`, and `weight`.

- **FM-002** [MUST] The `title` field MUST be concise
  (under 60 characters preferred) and describe the page's
  topic, not its section. The title generates the H1
  heading -- body content MUST NOT start with H1.

- **FM-003** [MUST] The `description` field MUST be a
  complete sentence of 110-160 characters that works as
  an SEO meta description and Open Graph preview. It MUST
  communicate the page's value proposition, not just its
  topic.

- **FM-004** [SHOULD] The `lead` field SHOULD provide a
  1-2 sentence hook that appears at the top of the page.
  It SHOULD be distinct from `description` -- the lead
  speaks to readers already on the page, while the
  description speaks to readers in search results.

- **FM-005** [MUST] The `weight` field MUST control
  sidebar ordering within the section. Lower weight =
  higher position. New pages MUST use weights consistent
  with sibling pages.

- **FM-006** [MUST] Blog posts MUST include additional
  frontmatter: `slug`, `categories`, `tags`, and
  `contributors`. The `slug` MUST be kebab-case and
  match the filename.

- **FM-007** [MUST] Section index pages MUST be named
  `_index.md` (with leading underscore).

---

## Content Structure

- **ST-001** [MUST] Body content MUST start with an H2
  (`##`) heading. The `title` frontmatter generates H1.
  Never use H1 (`#`) in body content.

- **ST-002** [MUST] Headings MUST use ATX style (`##`,
  `###`) not underline style (`===`, `---`). Headings
  MUST follow a strict hierarchy -- no skipping levels
  (e.g., H2 to H4 without H3).

- **ST-003** [MUST] The first paragraph after the opening
  H2 MUST establish what the page is about and why the
  reader should care. A reader who stops after this
  paragraph MUST understand the page's purpose.

- **ST-004** [SHOULD] Pages SHOULD progress from general
  to specific: concept overview, then practical usage,
  then reference details, then edge cases. The most
  common reader task SHOULD be addressable without
  scrolling past uncommon content.

- **ST-005** [SHOULD] Pages longer than 5 H2 sections
  SHOULD set `toc: true` in frontmatter to generate a
  table of contents for navigation.

- **ST-006** [MUST] Every documentation page MUST end
  with a "Next Steps" or "Learn More" section that gives
  the reader at least 2-3 cross-links to related pages.
  No page is a dead end.

- **ST-007** [SHOULD] Sections SHOULD be self-contained
  enough that a reader arriving via a direct link to a
  heading anchor can understand the content without
  reading preceding sections.

---

## Writing Quality

- **WQ-001** [MUST] Content MUST be task-oriented -- it
  answers "how do I do X?" before "how does X work
  internally?" Prefer imperative instructions ("Run
  `uf setup`") over passive descriptions ("The setup
  process can be initiated").

- **WQ-002** [MUST] Content MUST NOT contain placeholder
  text, TODO markers, "Coming Soon," "TBD," or empty
  sections. Only publish pages with real, complete
  content.

- **WQ-003** [MUST] All factual claims (metrics,
  capabilities, file counts, version numbers) MUST be
  sourced from the actual tool's current state. Never
  fabricate features, overstate maturity, or present
  aspirational capabilities as current functionality.

- **WQ-004** [MUST] Terminology MUST be consistent across
  all pages. The same concept MUST use the same term
  everywhere. If a concept has multiple common names,
  choose one canonical term and use it consistently.
  Introduce the canonical term on first use with any
  necessary context.

- **WQ-005** [SHOULD] Prose paragraphs SHOULD be 3-5
  sentences. Blocks of text longer than 5 sentences
  without a visual break (heading, list, code block,
  table) SHOULD be split for scannability.

- **WQ-006** [MUST] Content MUST NOT use weasel words
  that dismiss the reader's effort: "simply," "just,"
  "easily," "obviously," "of course." These words
  signal that the writer has not considered the reader's
  perspective.

- **WQ-007** [SHOULD] Active voice SHOULD be preferred
  over passive voice. "Gaze detects side effects" is
  stronger than "Side effects are detected by Gaze."
  Passive voice is acceptable when the actor is
  genuinely unknown or unimportant.

- **WQ-008** [SHOULD] Every feature description SHOULD
  pass the "so what" test: the benefit to the reader
  SHOULD be explicit, not implied. "Gaze detects side
  effects" is a feature. "Gaze tells you which tests
  are actually verifying behavior" is a benefit.

- **WQ-009** [MUST] Technical jargon, acronyms, and
  project-specific terms MUST be explained on first use
  or linked to a page that explains them. Assume the
  reader is a mid-level developer encountering the
  project for the first time.

- **WQ-010** [MUST] Numerical claims (counts, percentages,
  scores) MUST be consistent across all pages that
  reference them. If accuracy is "84.7%" on one page, it
  MUST be "84.7%" on every other page that mentions it.

---

## Markdown Formatting

- **MF-001** [MUST] Use fenced code blocks (triple
  backticks) with a language identifier for all code
  examples: ` ```bash `, ` ```yaml `, ` ```json `,
  ` ```text `, ` ```go `, etc. Never use bare fenced
  blocks without a language identifier.

- **MF-002** [MUST] Use inline code spans (single
  backticks) for commands (`uf setup`), filenames
  (`spec.md`), flags (`--verbose`), and identifiers.
  Do not use inline code for emphasis -- use bold or
  italics instead.

- **MF-003** [MUST] Markdown tables MUST have aligned
  pipes, header separators with at least 3 dashes per
  column (`|---|`), and spaces around cell content
  (`| Content |` not `|Content|`).

- **MF-004** [MUST] Use em-dashes (`---` rendered as
  `---`) for parenthetical clauses, consistent with the
  site's established style. Do not use double hyphens
  (`--`) where em-dashes are the convention.

- **MF-005** [SHOULD] Lists SHOULD use consistent markers
  within a section: either all `-` or all `*` or all
  numbered. Mixed markers within the same list are a
  formatting violation.

- **MF-006** [MUST] Internal links MUST use relative
  paths (`/docs/getting-started/quick-start/`) not
  absolute URLs (`https://unboundforce.dev/docs/...`).
  Links MUST point to existing pages. Anchor links MUST
  match actual heading slugs.

- **MF-007** [SHOULD] Images SHOULD have alt text that
  describes the content for accessibility. Decorative
  images SHOULD use an empty alt attribute.

---

## Blog Posts

- **BP-001** [MUST] Blog posts MUST have a narrative arc:
  problem statement, approach, evidence or walkthrough,
  and a conclusion with a call to action. Posts that are
  lists of features without context do not engage readers.

- **BP-002** [MUST] Blog post titles MUST be specific and
  descriptive. "Update on Progress" is too vague.
  "Why Contract Coverage: How Gaze Redefines What It
  Means to Have Good Tests" communicates the topic and
  value proposition.

- **BP-003** [SHOULD] Blog posts SHOULD include real-world
  examples, actual output, or concrete data rather than
  abstract descriptions. Show, then explain.

- **BP-004** [MUST] Blog posts MUST NOT contain
  time-sensitive language that becomes stale ("recently,"
  "new," "just launched," "this week") unless absolutely
  necessary. Use specific dates if temporality matters.

- **BP-005** [SHOULD] Blog posts SHOULD be self-contained
  -- a reader arriving from search or social media SHOULD
  understand the post without reading other site content.
  Link to detailed documentation for reference, but don't
  require it for comprehension.

- **BP-006** [MUST] The blog post's `description`
  frontmatter MUST work as a social media preview. It
  appears in Open Graph link cards. It MUST be a
  compelling 1-2 sentence summary, not a generic topic
  label.

---

## Cross-Referencing

- **XR-001** [MUST] When a concept is explained in depth
  on another page, the current page MUST link to it
  rather than duplicating the explanation. Brief context
  (1-2 sentences) is acceptable; full reproduction is
  not.

- **XR-002** [SHOULD] Significant features or capabilities
  SHOULD be reachable from at least 2 entry-point pages
  (homepage, Quick Start, getting-started landing, role
  guides). A feature mentioned on only 1 page has a
  discoverability problem.

- **XR-003** [MUST] When introducing a slash command
  (e.g., `/unleash`, `/finale`), the introduction MUST
  include the command's purpose in one sentence and a
  link to the full documentation. Do not assume the
  reader knows what the command does.

- **XR-004** [SHOULD] "Next Steps" links SHOULD lead to
  pages that logically follow the current page's topic.
  A getting-started guide's Next Steps SHOULD link to
  the relevant role guide, project page, or workflow
  documentation -- not to unrelated sections.

---

## Credibility and Trust

- **CT-001** [MUST] Content MUST NOT present aspirational
  capabilities as current features. Describing what a
  tool "will do" or "is designed to do" as what it "does"
  is a credibility violation. Use "Current Limitations"
  sections to honestly scope the tool's current state.

- **CT-002** [MUST] Content MUST NOT use loaded AI
  terminology ("AGI," "intelligent," "self-improving,"
  "sentient") without precise definition and
  justification. These terms invite skepticism from
  technical audiences.

- **CT-003** [SHOULD] Pages describing tool capabilities
  SHOULD include a "Current Limitations" or "What's Not
  Supported" section when applicable. Honesty about
  boundaries builds more trust than a polished feature
  list.

- **CT-004** [MUST] Content MUST accurately signal the
  project's maturity level. An early-stage project MUST
  NOT claim enterprise-grade reliability. Version numbers
  and limitation sections serve as maturity signals.

---

## Research and Source Verification

- **RS-001** [SHOULD] When researching upstream tool
  behavior, feature capabilities, or cross-repo context
  for documentation accuracy, agents SHOULD use Dewey
  MCP tools (`dewey_dewey_semantic_search`,
  `dewey_get_page`, `dewey_search`, `dewey_find_by_tag`)
  as the primary discovery mechanism. Dewey indexes all
  sibling repositories, the org workspace, and GitHub
  issues/PRs -- grep and glob only see the current repo.

- **RS-002** [SHOULD] After discovering relevant content
  via Dewey, agents SHOULD use the Read tool for targeted
  file access when exact line-level content is needed.
  Dewey finds the right document; Read gets the precise
  text.

- **RS-003** [MUST] All factual claims about upstream
  tools (feature descriptions, CLI flags, file counts,
  accuracy numbers) MUST be verified against the
  authoritative source before writing. Use Dewey to
  locate the source, then Read to confirm the exact
  content. Do not rely on memory or prior conversation
  context for specific numbers or capabilities.

- **RS-004** [SHOULD] When writing documentation about
  features that span multiple repositories (e.g., how
  `uf init` configures Dewey, how `/unleash` invokes
  Speckit commands), agents SHOULD use
  `dewey_dewey_semantic_search` to find the relevant
  specs, design documents, and PRs across repos before
  synthesizing the documentation. This ensures the
  documentation reflects the actual implementation, not
  an assumption about it.

---

## Custom Rules

<!-- This section is intentionally empty in the canonical
     pack. Project-specific custom rules belong in
     markdown-custom.md alongside this file. Custom rules
     use the CR-NNN identifier prefix. -->
