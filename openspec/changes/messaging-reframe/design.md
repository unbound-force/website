## Context

The homepage has a "Why Unbound Force?" section with one featured card and three supporting cards. The getting-started index has sections for the stack table, heroes, and a "Get Started" link list. Neither speaks to visitors familiar with harness engineering concepts. Issue #90 calls for adding a feature card and a design philosophy section.

The proposal (proposal.md) established constitution alignment: Content Accuracy (PASS), Minimal Footprint (PASS), Visitor Clarity (PASS).

## Goals / Non-Goals

### Goals
- Add one feature card to the homepage "Why Unbound Force?" section
- Add a "Design Philosophy" section to the getting-started index page
- Use harness engineering as secondary framing, superhero identity stays primary
- Attribute any Liu article data points to their source

### Non-Goals
- Rebranding the homepage or site around harness engineering
- Adding custom CSS or new visual elements
- Modifying the hero section, project cards, or other existing content
- Citing specific numbers from the Liu article (the card should convey the concept, not reproduce data)

## Decisions

**Homepage card placement**: Add as a fourth supporting card in the "Why Unbound Force?" grid, after "Open Source". This maintains the existing 3-column grid layout (the fourth card wraps naturally on large screens and stacks on mobile).

**Card content**: "Built as a Harness, Not a Wrapper" — feedforward controls guide agents before they write code, feedback controls catch what slips through. This conveys the core architectural concept without requiring knowledge of Liu's article.

**Design philosophy section**: Add after the "Get Started" section in _index.md. Three paragraphs: the Agent = Model + Harness equation, the three-tier context system, and layered feedback. Link to the architecture page only if it exists on this branch (it won't — #88 is a separate PR).

## Risks / Trade-offs

**Risk**: Fourth feature card may look unbalanced in the 3-column grid on large screens. Mitigation: Bootstrap handles this gracefully — the fourth card appears centered below, or can be part of a 2+2 grid.

**Trade-off**: Not citing specific numbers from Liu (52.8% → 66.5%, $9 → $125). The issue suggested this, but the numbers require significant context to be meaningful and might confuse visitors unfamiliar with the article. The card conveys the concept instead.
