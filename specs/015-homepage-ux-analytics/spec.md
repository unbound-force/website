# Feature Specification: Homepage UX Redesign and Analytics

**Feature Branch**: `015-homepage-ux-analytics`
**Created**: 2026-04-01
**Status**: Draft
**Input**: Redesign homepage based on 15-point UX audit (hero CTAs, lead text, feature card hierarchy, blog post count, narrative structure) and add Google Analytics tracking (G-969E28NTFN).

## User Scenarios & Testing _(mandatory)_

### User Story 1 — Hero Section: Clear Primary Action and Compelling Lead (Priority: P1)

A first-time visitor lands on the homepage. Currently, the primary CTA ("Explore on GitHub") sends them off-site to a code repository, and the secondary CTA ("View Gaze") references a sub-project they've never heard of. The lead text is generic ("purpose-built personas work together through structured workflows to ship quality software") and doesn't differentiate the project from any other AI coding tool. The hero badge says "Open Source AI Agent Swarm" — two generic terms that don't earn attention.

After this change, the hero leads with a compelling, differentiating statement, the primary CTA keeps visitors on-site ("Get Started"), and the badge communicates the unique capability.

**Why this priority**: The hero is what 80%+ of visitors see first. Every second a visitor spends confused about what to do or unimpressed by the lead text is lost conversion. These are the highest-impact, lowest-effort changes on the page.

**Independent Test**: Load the homepage and verify: (a) the primary CTA says "Get Started" and links to the Quick Start page, (b) the secondary CTA says "Explore on GitHub" and links to the org, (c) the lead text communicates the unique autonomous pipeline capability, (d) the badge text is specific and arresting.

**Acceptance Scenarios**:

1. **Given** a visitor on the homepage, **When** they look at the hero CTAs, **Then** the primary (visually prominent) button says "Get Started" and links to `/docs/getting-started/quick-start/`, and the secondary button says "Explore on GitHub" and links to the GitHub org.
2. **Given** a visitor reading the hero lead text, **When** they finish the first sentence, **Then** they understand what makes Unbound Force different from other AI coding tools (the autonomous spec-to-demo pipeline).
3. **Given** a visitor seeing the hero badge, **When** they read it, **Then** it communicates a specific capability (not generic terms like "AI Agent Swarm").

---

### User Story 2 — Feature Cards: Visual Hierarchy and Action-Oriented Framing (Priority: P1)

A visitor scrolls to the "Why Unbound Force?" section and sees 4 visually identical cards. The flagship capability (`/unleash` — spec to demo in one command) has the same visual weight as "Open Source" (a license fact). The card headings describe properties ("Agent Personas", "Quality-First") rather than actions or benefits the visitor can relate to.

After this change, the `/unleash` card is visually prominent, and card headings communicate actions or benefits rather than abstract properties.

**Why this priority**: The feature cards are the second thing visitors see. If the flagship capability doesn't stand out visually and the cards don't communicate benefits, the visitor leaves with a vague impression instead of a clear value proposition.

**Independent Test**: Load the homepage and verify: (a) the `/unleash` card is visually distinct from the other 3, (b) card headings communicate what the visitor can do or gain, not just what the system has.

**Acceptance Scenarios**:

1. **Given** a visitor looking at the feature cards, **When** they scan the section, **Then** the `/unleash` card is visually more prominent than the other 3 cards (wider, different background, larger, or positioned as a standalone callout above the grid).
2. **Given** a visitor reading the card headings, **When** they scan all 4 headings, **Then** at least 3 of the 4 communicate an action or benefit (not just a property label).

---

### User Story 3 — Page Narrative: "How It Works" Section and Blog Post Count (Priority: P2)

A visitor reads the homepage top to bottom. Currently, the page structure is: hero → "Why Unbound Force?" → Projects → 1 Blog Post → CTA. The "Why" section comes before the reader understands what the product does. There's no visual summary of how the product works. Only 1 of 4 blog posts is shown, wasting space that could showcase the site's strongest content.

After this change, the page includes a brief "How It Works" visual (specify → unleash → finale) between the hero and the feature cards, and the blog section shows more posts.

**Why this priority**: The narrative structure affects comprehension for visitors who scroll the full page. Adding "How It Works" answers "what do I actually do with this?" before "why is it good?" — the natural reader question order. Showing more blog posts increases engagement with the site's most credible content.

**Independent Test**: Load the homepage and verify: (a) a "How It Works" or equivalent section exists between the hero and the feature cards, (b) the blog section shows at least 2 posts.

**Acceptance Scenarios**:

1. **Given** a visitor scrolling past the hero, **When** they reach the section below the hero, **Then** they find a brief visual or text summary of the 3-step workflow (specify → unleash → finale) before the feature cards.
2. **Given** a visitor scrolling to the blog section, **When** they look at the articles, **Then** they see at least 2 (and up to 3) recent blog posts, not just 1.
3. **Given** a visitor reading the "How It Works" section, **When** they finish it, **Then** they understand the product's workflow well enough that the "Why Unbound Force?" feature cards make sense in context.

---

### User Story 4 — Cleanup: Sidebar Footer, Project Card Benefits, Code Styling (Priority: P3)

The sidebar footer section near-duplicates the CTA section. The project card descriptions explain what the tools do technically but not why a visitor should care. A `<code>` tag appears in a marketing card, and the hero CTAs use complex SVG icons that add visual weight without clarity.

After this change, the sidebar footer is removed or differentiated, project descriptions include benefit framing, and the hero buttons are simplified.

**Why this priority**: These are polish items that improve overall quality but don't affect the core conversion flow (hero → features → CTA). Individually low impact, collectively they improve the page's professional feel.

**Independent Test**: Load the homepage and verify: (a) no near-duplicate CTA sections exist, (b) project descriptions include a benefit phrase, (c) hero buttons have clean, simple styling.

**Acceptance Scenarios**:

1. **Given** a visitor scrolling the entire page, **When** they reach the bottom, **Then** they do not encounter two near-identical CTA sections.
2. **Given** a visitor reading the project cards, **When** they read a project description, **Then** they understand the benefit to their development workflow, not just the technical capability.
3. **Given** a visitor looking at the hero buttons, **When** they see the CTAs, **Then** the buttons use simple text or minimal icons (not multi-line SVG paths).

---

### User Story 5 — Google Analytics Tracking (Priority: P1)

The site has no visitor analytics. The team cannot measure which pages are visited, which CTAs are clicked, or whether the homepage changes improve engagement. After this change, Google Analytics (measurement ID G-969E28NTFN) tracks page views across the entire site.

**Why this priority**: Without analytics, all UX changes in this spec are unmeasurable. Adding analytics first ensures the team can validate whether the homepage redesign actually improves engagement. This is a prerequisite for data-driven iteration.

**Independent Test**: Load any page on the site, open browser developer tools, and verify that the Google Analytics script is loaded with the correct measurement ID.

**Acceptance Scenarios**:

1. **Given** a visitor loading any page on the site, **When** the page renders, **Then** the Google Analytics tracking script is present in the page source with measurement ID G-969E28NTFN.
2. **Given** a visitor navigating between pages, **When** they click a link, **Then** each page view is tracked by Google Analytics.

---

### Edge Cases

- What happens when a visitor has an ad blocker that blocks Google Analytics? The site must render and function normally regardless. Analytics is enhancement-only — it must not affect page load, layout, or functionality.
- What happens when the blog section has fewer than 2 published posts? The template should gracefully show whatever posts exist (1 if only 1, 0 if none) without broken layout.
- What happens when the "How It Works" section is viewed on mobile? The 3-step visual must be readable on narrow screens (vertical stacking rather than horizontal).
- What happens when a visitor arrives with JavaScript disabled? The page must render all content. Analytics tracking will not function but no visual breakage should occur.

## Requirements _(mandatory)_

### Functional Requirements

**Hero Section (US1):**

- **FR-001**: The hero primary CTA MUST say "Get Started" and link to `/docs/getting-started/quick-start/`.
- **FR-002**: The hero secondary CTA MUST say "Explore on GitHub" and link to `https://github.com/unbound-force`.
- **FR-003**: The "View Gaze" button MUST be removed from the hero section.
- **FR-004**: The hero lead text MUST communicate the autonomous pipeline capability and differentiate from generic AI coding tools.
- **FR-005**: The hero badge text MUST be replaced with a specific, capability-focused phrase.
- **FR-006**: The hero CTA buttons MUST use simple text or minimal icons (remove the multi-line SVG path icons).

**Feature Cards (US2):**

- **FR-007**: The `/unleash` ("Spec to Demo in One Command") feature card MUST be visually more prominent than the other 3 cards.
- **FR-008**: At least 3 of the 4 feature card headings MUST communicate an action or benefit, not a property label.

**Page Narrative (US3):**

- **FR-009**: A "How It Works" section MUST exist between the hero and the "Why Unbound Force?" feature cards section, summarizing the 3-step workflow (specify → unleash → finale).
- **FR-010**: The blog section MUST display at least 2 recent posts (up to 3), changed from the current limit of 1.
- **FR-011**: The "How It Works" section MUST be responsive and readable on mobile screens (vertical stacking for narrow viewports).

**Cleanup (US4):**

- **FR-012**: The sidebar footer section (lines 335-354) MUST be removed or differentiated from the main CTA section to eliminate duplicate calls-to-action.
- **FR-013**: The Gaze project card description MUST include a benefit phrase (why it matters to the visitor's workflow, not just what it does technically).
- **FR-014**: The Dewey project card description MUST include a benefit phrase.

**Analytics (US5):**

- **FR-015**: Google Analytics MUST be configured site-wide with measurement ID G-969E28NTFN.
- **FR-016**: Analytics MUST NOT affect page load performance, layout, or functionality when blocked or when JavaScript is disabled.

**Cross-cutting:**

- **FR-017**: All changes MUST pass `npm run build` without errors.
- **FR-018**: All changes MUST render correctly in both light and dark mode.

## Success Criteria _(mandatory)_

### Measurable Outcomes

- **SC-001**: The hero primary CTA links to an on-site page (Quick Start), not an external site — verified by inspecting the link target.
- **SC-002**: The `/unleash` feature card is visually distinguishable from the other 3 cards without reading the text — verified by screenshot comparison.
- **SC-003**: A "How It Works" section is visible between the hero and features when scrolling — verified by page inspection.
- **SC-004**: The blog section shows 2+ posts on a site with 4 published posts — verified by counting rendered cards.
- **SC-005**: Google Analytics script with measurement ID G-969E28NTFN is present in the rendered page source — verified by browser developer tools.
- **SC-006**: `npm run build` succeeds with zero errors.
- **SC-007**: No duplicate CTA sections appear on the page — verified by scrolling the full page.

## Assumptions

- Google Analytics will use Hugo's built-in `googleAnalytics` configuration parameter in `hugo.toml`, which is the standard method for Hugo sites. If Hugo's built-in template is not compatible with the Doks/Thulite theme, a custom partial may be needed.
- The "How It Works" section will use the existing card/section styling patterns (Bootstrap grid, feature-card classes) rather than requiring new custom CSS, per the Minimal Footprint principle.
- The feature card visual hierarchy for `/unleash` will be achieved through layout changes (wider card spanning 2 columns, or a standalone callout above the grid) rather than adding heavy custom styling.
- The sidebar footer section (`sectionFooter` in `params.toml`) will be disabled via configuration rather than removing the template code, in case it's needed in the future.
- The blog section change from `first 1` to `first 3` requires adjusting the grid layout (1-column to 3-column) to accommodate multiple posts.
- The hero lead text and badge will be set in `content/_index.md` frontmatter (lead field) and `layouts/home.html` (badge text), matching the existing pattern.
- All hero button SVG simplification will use Tabler Icons (already a project dependency) or plain text buttons, not custom SVGs.
