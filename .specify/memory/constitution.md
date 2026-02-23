<!--
  SYNC IMPACT REPORT
  ==================
  Version change: (none) → 1.0.0 (initial ratification)

  Modified principles:
    - [PRINCIPLE_1_NAME] → I. Content Accuracy
    - [PRINCIPLE_2_NAME] → II. Minimal Footprint
    - [PRINCIPLE_3_NAME] → III. Visitor Clarity
    - [PRINCIPLE_4_NAME] → (removed, not needed)
    - [PRINCIPLE_5_NAME] → (removed, not needed)

  Added sections:
    - Governance (filled from template)

  Removed sections:
    - [SECTION_2_NAME] (behavioral constraints kept in AGENTS.md)
    - [SECTION_3_NAME] (development workflow kept in AGENTS.md)
    - Principles 4 and 5 (user chose 3 principles)

  Templates requiring updates:
    ✅ .specify/templates/plan-template.md — no changes needed;
       Constitution Check section is generic and will align at plan time.
    ✅ .specify/templates/spec-template.md — no changes needed;
       requirements format already uses MUST/SHOULD language.
    ✅ .specify/templates/tasks-template.md — no changes needed;
       task phases are feature-driven, not principle-specific.
    ✅ .specify/templates/checklist-template.md — no changes needed.
    ✅ .specify/templates/agent-file-template.md — no changes needed.
    ✅ README.md — single-line placeholder; no principle refs to update.

  Deferred TODOs:
    - RATIFICATION_DATE set to today (first adoption). Update if a
      different ceremonial date is preferred.
-->

# Unbound Force Website Constitution

## Core Principles

### I. Content Accuracy

Every piece of content on the Unbound Force website MUST accurately
reflect the current state of the source repositories it describes.

- Project descriptions, capabilities, and statuses MUST be derived
  from actual repository content (README.md, AGENTS.md, specs). Never
  fabricate features or overstate project maturity.
- Claims about what a project does MUST be verifiable by inspecting
  the project's repository. If a feature is not yet implemented, it
  MUST NOT appear on the website as if it exists.
- No placeholder or "Coming Soon" content. Pages MUST only be
  published when they contain real, substantive content.
- When upstream repository content changes, the corresponding website
  pages MUST be reviewed and updated to stay in sync.

**Rationale**: The website is the public face of Unbound Force. If it
misrepresents the projects — overstating capabilities, listing
unimplemented features, or showing stale information — it erodes
trust with the exact audience the organization is trying to reach.
Accuracy is the foundation of credibility.

### II. Minimal Footprint

The website MUST use the simplest implementation that achieves the
goal. Every custom element MUST justify its existence over the
built-in alternative.

- Prefer Markdown content over custom HTML. Use Doks theme built-in
  features (shortcodes, partials, components) before writing custom
  layouts or CSS.
- Check the [Doks documentation](https://getdoks.org/) before adding
  any custom SCSS, JavaScript, or template overrides. If Doks already
  provides the feature, use it.
- Do not add dependencies to `package.json` unless strictly required.
  Every dependency is a maintenance burden.
- Do not over-engineer. The site SHOULD be functional and accurate,
  not feature-complete. Content is more important than custom design.
- Do not add a blog, analytics, newsletter signup, or other ancillary
  features unless explicitly requested and justified.

**Rationale**: A documentation-focused project site should be low
maintenance. Every custom element — a template override, a CSS rule,
a JavaScript snippet — is a future maintenance task. The Doks theme
already provides search, dark mode, sidebar navigation, mobile
responsiveness, and SEO. Using these built-ins keeps the site lean
and lets contributors focus on content rather than infrastructure.

### III. Visitor Clarity

The website MUST clearly communicate what Unbound Force is, what each
project does, and how to get involved. Every page MUST serve a
discoverable purpose.

- The homepage MUST provide a clear value proposition: what Unbound
  Force is, what it offers, and how to get started — within seconds
  of landing.
- Project pages MUST be adapted for a website audience. Do not copy
  READMEs verbatim. Frame content around "why should I care" rather
  than raw technical setup details.
- Navigation MUST be intuitive. A visitor with no prior knowledge of
  the project MUST be able to find any published page within two
  clicks from the homepage.
- Information hierarchy MUST flow logically: homepage → projects →
  detailed docs. Do not bury important content in deep navigation
  structures.
- Every page MUST have a clear purpose. If you cannot articulate what
  value a page provides to a visitor, it SHOULD NOT exist.

**Rationale**: The website exists to serve visitors — developers and
teams evaluating whether Unbound Force is relevant to them. If the
site is confusing, poorly organized, or requires insider knowledge to
navigate, it fails its core purpose regardless of how accurate or
minimal the implementation is.

## Governance

This constitution is the highest-authority document for the Unbound
Force website project. All development practices, pull request
reviews, and content decisions MUST be consistent with the principles
defined above.

- **Amendments**: Any change to this constitution MUST be proposed via
  pull request, reviewed, and approved before merge. The amendment
  MUST include a migration plan if it alters or removes existing
  principles.
- **Versioning**: The constitution follows semantic versioning:
  - MAJOR: Principle removal or incompatible redefinition.
  - MINOR: New principle or materially expanded guidance.
  - PATCH: Clarifications, wording, or non-semantic refinements.
- **Compliance Review**: At each planning phase (spec, plan, tasks),
  the Constitution Check gate MUST verify that the proposed work
  aligns with all active principles. Constitution violations are
  automatically CRITICAL severity and non-negotiable.

**Version**: 1.0.0 | **Ratified**: 2026-02-23 | **Last Amended**: 2026-02-23
