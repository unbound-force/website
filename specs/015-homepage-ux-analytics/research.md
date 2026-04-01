# Research: Homepage UX Redesign and Analytics

**Date**: 2026-04-01

## R1: Hugo Google Analytics Configuration

**Decision**: Use Hugo's built-in `googleAnalytics` config parameter.

Hugo supports Google Analytics 4 natively. Add to `config/_default/hugo.toml`:

```toml
googleAnalytics = "G-969E28NTFN"
```

Hugo's internal template (`google_analytics.html`) generates the gtag.js snippet. The Thulite/Doks theme inherits this — no custom partial needed.

The gtag script loads asynchronously and does not block page rendering. If JavaScript is disabled or the script is blocked, the page renders normally.

## R2: Hero Section Redesign

**Current state**:

- Badge: "Open Source AI Agent Swarm"
- Lead: "An open source AI agent swarm for software engineering. Purpose-built personas work together through structured workflows to ship quality software."
- Primary CTA: "Explore on GitHub" (links off-site)
- Secondary CTA: "View Gaze" (links to a sub-project GitHub repo)

**New design**:

- Badge: "Autonomous Development Pipeline" (specific, capability-focused)
- Lead: "AI coding agents are powerful but not self-directing. Unbound Force gives them structure — one command to go from a feature description to tested, reviewed, demo-ready code."
- Primary CTA: "Get Started" (links to /docs/getting-started/quick-start/)
- Secondary CTA: "Explore on GitHub" (links to github.com/unbound-force)
- Remove "View Gaze" entirely
- Simplify button SVGs to minimal arrow or no icon

## R3: Feature Card Hierarchy

**Current cards** (all visually identical):

1. Agent Personas
2. Spec to Demo in One Command
3. Quality-First
4. Open Source

**New approach**: Make card #2 (/unleash) a full-width featured card above the other 3. This creates a clear visual hierarchy without adding heavy CSS. The featured card spans the full container width with a larger description, then the remaining 3 cards sit in a 3-column grid below.

**Revised headings** (action/benefit-oriented):

1. "A Team That Knows Your Codebase" (was "Agent Personas") — describes the benefit of specialized personas
2. "Spec to Demo in One Command" (keep — already action-oriented)
3. "Every Change Tested and Reviewed" (was "Quality-First") — describes the outcome
4. "Open Source" (keep — value statement, not a property)

## R4: "How It Works" Section

**Placement**: Between hero and "Why Unbound Force?" features section.

**Content**: 3-step visual showing the developer workflow:

1. **Specify** — Describe what you want to build
2. **Unleash** — The swarm handles the rest
3. **Finale** — Ship it

Use a 3-column grid with step numbers, headings, and 1-line descriptions. On mobile, stack vertically. Use existing feature-card styling with numbered badges.

## R5: Blog Section Expansion

**Current**: `first 1` — shows only the latest post.
**New**: `first 3` — shows up to 3 posts in a 3-column grid (or 2 + 1 on narrower screens).

The site currently has 4 blog posts. Showing 3 gives better coverage of the site's strongest content while keeping the section compact.

## R6: Sidebar Footer

**Current**: Lines 335-354 in home.html, controlled by `site.Params.doks.sectionFooter` in params.toml.

Checking params.toml: `sectionFooter` is set to `true` (line 84). This means the duplicate CTA renders.

**Decision**: Set `sectionFooter = false` in params.toml to disable the duplicate. The main CTA section (lines 307-334) already handles this role with better messaging.
