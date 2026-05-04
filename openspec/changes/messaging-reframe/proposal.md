## Why

The website's messaging doesn't speak to the harness engineering audience — engineers who have read Liu's article and are looking for concrete implementations. The homepage feature cards describe tools but don't explain the architectural principles that make the system work. The getting-started index describes what the heroes do but not the design philosophy behind the system. With Liu's article actively circulating (1.7K claps), there is a time-sensitive positioning opportunity.

GitHub Issue: #90

## What Changes

- Add a new feature card to the homepage (`layouts/home.html`) addressing the harness engineering audience — "Built as a Harness, Not a Wrapper" with feedforward/feedback control framing
- Add a "Design Philosophy" section to the getting-started index (`content/docs/getting-started/_index.md`) after the existing content, framing in terms the harness engineering community recognizes
- Harness engineering is a secondary framing lens, not a rebrand — superhero team identity remains primary

## Capabilities

### New Capabilities
- `homepage-harness-card`: A feature card on the homepage that speaks to the harness engineering audience
- `getting-started-design-philosophy`: A section on the getting-started index page explaining the design philosophy

### Modified Capabilities
- None

### Removed Capabilities
- None

## Impact

- Modified file: `layouts/home.html` (add one feature card)
- Modified file: `content/docs/getting-started/_index.md` (add design philosophy section)
- No new files, no changes to navigation or styles

## Constitution Alignment

Assessed against the **Unbound Force Website** constitution (.specify/memory/constitution.md).

### I. Content Accuracy

**Assessment**: PASS

The homepage card describes real architectural features (feedforward/feedback controls). Any cited data points from the Liu article are attributed to their source. The design philosophy section describes the three-tier context system and layered feedback — both verifiable against the upstream repository.

### II. Minimal Footprint

**Assessment**: PASS

The homepage card follows the existing feature card HTML pattern — no new CSS, templates, or JavaScript. The getting-started section is pure Markdown. No new dependencies.

### III. Visitor Clarity

**Assessment**: PASS

Both changes improve visitor clarity by connecting Unbound Force to a recognized industry framework. The homepage card gives visitors from the harness engineering community an immediate recognition point. The design philosophy section provides the "why" context missing from the current page.
