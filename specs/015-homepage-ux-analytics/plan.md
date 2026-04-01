# Implementation Plan: Homepage UX Redesign and Analytics

**Branch**: `015-homepage-ux-analytics` | **Date**: 2026-04-01 | **Spec**: [spec.md](spec.md)

## Summary

Redesign the homepage based on a 15-point UX audit and add Google Analytics tracking. Key changes: swap hero CTAs (Get Started primary), rewrite lead text, add "How It Works" section, make /unleash card visually prominent, show 3 blog posts, remove sidebar footer duplicate, add benefit framing to project cards, and configure GA4 (G-969E28NTFN).

## Technical Context

**Language/Version**: Hugo (Go-based SSG) via `@thulite` npm packages
**Primary Dependencies**: `thulite ^2.6.3`, `@thulite/doks-core ^1.8.3`
**Testing**: `npm run build` must succeed; visual verification in dev server
**Target**: GitHub Pages (static site)

## Constitution Check

### I. Content Accuracy — PASS

All content changes describe actual current capabilities. The lead text and card headings are rewritten for clarity, not to overstate features.

### II. Minimal Footprint — PASS

Changes modify 2 existing files (home.html, \_index.md), add 1 config line (hugo.toml), and add minimal CSS for the "How It Works" section. No new dependencies, no new layouts, no JavaScript beyond Google's gtag.

### III. Visitor Clarity — PASS

This feature exists specifically to improve Visitor Clarity — the audit found the homepage undersells the flagship capability, sends visitors off-site, and has no "How It Works" explanation.

## File Change Matrix

| File                              | FRs                    | Change                                                                                                                             |
| --------------------------------- | ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| `layouts/home.html`               | FR-001 to FR-014       | Hero CTAs, badge, lead, "How It Works" section, feature card hierarchy, blog post count, project card descriptions, sidebar footer |
| `content/_index.md`               | FR-004                 | Lead text in frontmatter                                                                                                           |
| `config/_default/hugo.toml`       | FR-015                 | Add `googleAnalytics` config                                                                                                       |
| `assets/scss/common/_custom.scss` | FR-007, FR-009, FR-011 | Styles for "How It Works" section and featured card                                                                                |
