## Context

Convention packs are versioned rule sets deployed via `uf init`. They use numbered rules with MUST/SHOULD/MAY classification. The -custom.md extension pattern allows project-specific overrides. Issue #93 calls for a blog post explaining the concept.

## Goals / Non-Goals

### Goals
- Explain the problem (prose standards in AGENTS.md that drift across projects)
- Show pack structure with real examples from go.md
- Cover the -custom.md extension pattern
- Connect to the ThoughtWorks "harness templates" concept

### Non-Goals
- Full pack reference documentation (that's for a dedicated docs page)
- Modifying existing pages

## Decisions

**File**: `content/blog/convention-packs.md` with slug `convention-packs`.

**Structure**: Problem (prose drifts) → What packs look like → The extension pattern → Why structure matters → CTA.

## Risks / Trade-offs

Minor overlap with developer.md convention packs section. The blog post is narrative-driven (why packs exist, industry context) while the docs page is practical (how to use them).
