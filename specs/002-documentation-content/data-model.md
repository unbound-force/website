# Data Model: Documentation Content Pages

**Branch**: `002-documentation-content` | **Date**: 2026-02-27 | **Spec**: [spec.md](spec.md) | **Plan**: [plan.md](plan.md)

## Entities

### Docs-Level Index (`content/docs/_index.md`)

| Field         | Type     | Required | Description                          |
| ------------- | -------- | -------- | ------------------------------------ |
| `title`       | string   | Yes      | Docs section title ("Documentation") |
| `description` | string   | Yes      | SEO meta description                 |
| `lead`        | string   | Yes      | Subtitle/intro text below title      |
| `date`        | datetime | Yes      | Creation date (ISO 8601)             |
| `draft`       | boolean  | Yes      | Must be `false`                      |
| `weight`      | integer  | Yes      | Top-level ordering                   |
| `toc`         | boolean  | Yes      | Table of contents toggle             |

### Section Index Pages (`content/docs/<section>/_index.md`)

Four section indexes with identical frontmatter schema:

| Section         | Path                                     | Weight |
| --------------- | ---------------------------------------- | ------ |
| Getting Started | `content/docs/getting-started/_index.md` | 10     |
| Projects        | `content/docs/projects/_index.md`        | 20     |
| Team            | `content/docs/team/_index.md`            | 30     |
| Contributing    | `content/docs/contributing/_index.md`    | 40     |

### Content Pages (`content/docs/<section>/<page>.md`)

| Field         | Type     | Required | Description                               |
| ------------- | -------- | -------- | ----------------------------------------- |
| `title`       | string   | Yes      | Page title (H1)                           |
| `description` | string   | Yes      | SEO meta description                      |
| `lead`        | string   | Yes      | Brief intro/subtitle                      |
| `date`        | datetime | Yes      | Creation date (ISO 8601)                  |
| `draft`       | boolean  | Yes      | Must be `false`                           |
| `weight`      | integer  | Yes      | Ordering within section (lower = higher)  |
| `toc`         | boolean  | Yes      | Table of contents (`true` for most pages) |

### Content Pages — Full Inventory

| #   | File Path                                     | Section         | Weight | Source Content                                 |
| --- | --------------------------------------------- | --------------- | ------ | ---------------------------------------------- |
| 1   | `content/docs/_index.md`                      | (root)          | 1      | Minimal — redirects to Getting Started         |
| 2   | `content/docs/getting-started/_index.md`      | Getting Started | 10     | `unbound-force.md` overview                    |
| 3   | `content/docs/getting-started/quick-start.md` | Getting Started | 20     | Synthesized from Specify, OpenCode, Swarm docs |
| 4   | `content/docs/projects/_index.md`             | Projects        | 10     | Brief project listing                          |
| 5   | `content/docs/projects/gaze.md`               | Projects        | 20     | Gaze README + OpenCode integration             |
| 6   | `content/docs/team/_index.md`                 | Team            | 10     | "The Heroes" intro + composite image           |
| 7   | `content/docs/team/muti-mind.md`              | Team            | 20     | `unbound-force.md` + `multi-mind.png`          |
| 8   | `content/docs/team/cobalt-crush.md`           | Team            | 30     | `unbound-force.md` + `cobalt-crush.png`        |
| 9   | `content/docs/team/gaze-tester.md`            | Team            | 40     | `unbound-force.md` + `gaze.png`                |
| 10  | `content/docs/team/the-divisor.md`            | Team            | 50     | `unbound-force.md` + `the-divisor.png`         |
| 11  | `content/docs/team/mx-f.md`                   | Team            | 60     | `unbound-force.md` + `mx-f.png`                |
| 12  | `content/docs/contributing/_index.md`         | Contributing    | 10     | AGENTS.md conventions                          |

### Image Assets (existing in `static/images/team/`)

| File                     | Used On                    | Alt Text                                       |
| ------------------------ | -------------------------- | ---------------------------------------------- |
| `the-unbound-force.jpeg` | Team index page            | The Unbound Force — A.G.I. Development Squad   |
| `multi-mind.png`         | Muti-Mind persona page     | Muti-Mind — Product Owner trading card         |
| `cobalt-crush.png`       | Cobalt-Crush persona page  | Cobalt-Crush — Developer trading card          |
| `gaze.png`               | Gaze (Tester) persona page | Gaze — Tester trading card                     |
| `the-divisor.png`        | The Divisor persona page   | The Divisor — PR Reviewer Council trading card |
| `mx-f.png`               | Mx F persona page          | Mx F — Manager trading card                    |

## Relationships

```
Docs Root (1) ──contains──▶ Section Index (4)
Section Index (1) ──contains──▶ Content Page (many)
Content Page ──sourced from──▶ Source Repository
Persona Page ──displays──▶ Trading Card Image (1:1)
Team Index ──displays──▶ Composite Image (1:1)
Gaze Project Page ──links to──▶ Blog Article (/blog/why-contract-coverage/)
Gaze Project Page ──links to──▶ GitHub Repository (external)
Navigation Menu ──references──▶ Section Index (via [[docs]] entries)
```

## Validation Rules

1. **Frontmatter completeness**: All 7 required fields present on every page (FR-011)
2. **No H1 in body**: Body content starts with `##` (FR-012)
3. **Section index naming**: `_index.md` with leading underscore (FR-013)
4. **Unique weights**: No duplicate `weight` values within a section
5. **Image existence**: Referenced images exist in `static/images/team/` (verified above)
6. **Link validity**: All internal links resolve to existing pages (FR-015)
7. **No placeholders**: No "Coming Soon", "TODO", "TBD" content (SC-006)
8. **Content accuracy**: Facts verifiable against source repositories (FR-005, FR-008)
9. **Build success**: `npm run build` completes with zero errors (SC-003)
10. **Draft state**: All pages MUST have `draft: false` (FR-011)

## Configuration State

| File            | Field              | Current Value          | Change Needed                                                |
| --------------- | ------------------ | ---------------------- | ------------------------------------------------------------ |
| `menus.en.toml` | `[[docs]]` entries | All 4 sections defined | None — already configured                                    |
| `params.toml`   | `mainSections`     | `["docs", "blog"]`     | None                                                         |
| `params.toml`   | `navBarButton`     | `false`                | Change to `true` — button points to `/docs/getting-started/` |

## File Operations Summary

| File                                          | Operation | Content Source                    |
| --------------------------------------------- | --------- | --------------------------------- |
| `content/docs/_index.md`                      | Create    | Minimal frontmatter               |
| `content/docs/getting-started/_index.md`      | Create    | `unbound-force.md` overview       |
| `content/docs/getting-started/quick-start.md` | Create    | Synthesized from tool docs        |
| `content/docs/projects/_index.md`             | Create    | Brief project listing             |
| `content/docs/projects/gaze.md`               | Create    | Gaze README + OpenCode research   |
| `content/docs/team/_index.md`                 | Create    | Heroes intro + composite image    |
| `content/docs/team/muti-mind.md`              | Create    | `unbound-force.md` + trading card |
| `content/docs/team/cobalt-crush.md`           | Create    | `unbound-force.md` + trading card |
| `content/docs/team/gaze-tester.md`            | Create    | `unbound-force.md` + trading card |
| `content/docs/team/the-divisor.md`            | Create    | `unbound-force.md` + trading card |
| `content/docs/team/mx-f.md`                   | Create    | `unbound-force.md` + trading card |
| `content/docs/contributing/_index.md`         | Create    | AGENTS.md conventions             |
