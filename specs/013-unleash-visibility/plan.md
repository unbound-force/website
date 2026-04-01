# Implementation Plan: Unleash Visibility

**Branch**: `013-unleash-visibility` | **Date**: 2026-03-31 | **Spec**: [spec.md](spec.md)

## Summary

Surface `/unleash` as the flagship capability across the website. Create a blog post, add cross-references to 5 entry-point pages, restructure common-workflows.md, and update the homepage feature card. This takes `/unleash` from appearing on 1 of 22 pages to at least 7 pages plus a shareable blog post.

## Technical Context

**Language/Version**: Hugo (Go-based SSG) via `@thulite` npm packages
**Primary Dependencies**: `thulite ^2.6.3`, `@thulite/doks-core ^1.8.3`
**Testing**: `npm run build` must succeed; visual verification
**Constraints**: Markdown content + 1 HTML template edit; no new dependencies

## Constitution Check

### I. Content Accuracy -- PASS

All `/unleash` pipeline descriptions are sourced from the actual command file (`.opencode/command/unleash.md`). The blog post describes current behavior without version numbers.

### II. Minimal Footprint -- PASS

1 new blog post (Markdown), text edits to 5 existing pages, section reorder in 1 page, text edit in 1 HTML template. No new CSS, JS, or layouts.

### III. Visitor Clarity -- PASS

This feature exists specifically to fix a Visitor Clarity problem: the flagship capability is invisible on 21 of 22 pages. The restructuring places the most important content first.

## File Change Matrix

| File                                               | Priority | FRs                      | Change                                           |
| -------------------------------------------------- | -------- | ------------------------ | ------------------------------------------------ |
| `content/blog/unleash-in-practice.md`              | P1       | FR-001 to FR-006, FR-018 | New blog post                                    |
| `layouts/home.html`                                | P1+P2    | FR-007, FR-016, FR-017   | Rewrite Speckit card, CTA already done           |
| `content/docs/getting-started/quick-start.md`      | P1       | FR-008                   | Add /unleash to Start Working                    |
| `content/docs/getting-started/developer.md`        | P1       | FR-009                   | Add /unleash note before Speckit commands        |
| `content/docs/getting-started/_index.md`           | P1       | FR-010                   | Update Common Workflows description              |
| `content/docs/getting-started/product-owner.md`    | P1       | FR-011                   | Name /unleash alongside /workflow seed           |
| `content/docs/getting-started/common-workflows.md` | P2       | FR-012 to FR-015         | Reorder H2 sections, add note to manual workflow |

## Complexity Tracking

No constitution violations. No complexity tracking needed.
