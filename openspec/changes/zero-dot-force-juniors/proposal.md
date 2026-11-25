## Why

Zero Dot Force (https://github.com/zero-dot-force) is Unbound Force's incubator, framed as its junior hero team. The website mentions vibe-check, reading-stone and snake-eyes in the vision and roadmap but never introduces the org, and the Gaze and Divisor pages do not point at the junior projects that extend them. The Zero Dot Force org profile rewrite ([zero-dot-force/.github#41](https://github.com/zero-dot-force/.github/pull/41)) calls the org "The junior hero team of Unbound Force" and links back to unboundforce.dev; the website should link the other way with matching wording.

## What Changes

- New docs section `content/docs/zero-dot-force/_index.md` introducing Zero Dot Force and its four projects: vibe-check, reading-stone, snake-eyes, review-council
- "Zero Dot Force" entries in the main nav and the docs sidebar menu
- Cross-links: Gaze project and hero pages → reading-stone and snake-eyes; The Divisor → review-council; Projects index → Incubating pointer
- Roadmap: link the Zero Dot Force repos, and correct the vibe-check TypeScript bullet (TypeScript analysis already ships via ripwire)

## Capabilities

### New Capabilities

- `zero-dot-force-page`: a page describing the junior team and its projects

### Modified Capabilities

- Navigation and cross-references on gaze.md, gaze-tester.md, the-divisor.md, projects/\_index.md, roadmap/\_index.md

### Removed Capabilities

- None

## Impact

- One new content page, small text additions to five pages, two menu entries
- No layout, style or CI changes

## Constitution Alignment

Assessed against the **Unbound Force Website** constitution (.specify/memory/constitution.md).

### I. Content Accuracy

**Assessment**: PASS

Project descriptions and install commands come from each repository's README (vibe-check, reading-stone, snake-eyes, review-council) and match the Zero Dot Force org profile. No hero names are invented for the junior projects.

### II. Minimal Footprint

**Assessment**: PASS

Plain Markdown and menu entries only; no custom layouts, CSS or JavaScript.

### III. Visitor Clarity

**Assessment**: PASS

The junior team is reachable from the main nav and from the pages of the heroes each project extends.
