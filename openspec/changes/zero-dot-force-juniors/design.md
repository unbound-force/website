## Context

Zero Dot Force is a separate GitHub org that incubates tools for Unbound Force. Two of its projects are Gaze analyzers, one is a standalone Divisor council, and one (vibe-check) is already on the roadmap.

## Goals / Non-Goals

### Goals

- Give Zero Dot Force its own page, reachable from the main nav
- Tie each junior project to the senior hero it trains with

### Non-Goals

- Hero names or trading cards for the junior projects
- Homepage layout changes
- Full per-project documentation (each repository's README stays the source)

## Decisions

- **Own docs section, not a Projects subpage**: `content/docs/projects/` holds Unbound Force's own projects. Zero Dot Force is a separate org with its own team identity, so it gets a sibling section (`/docs/zero-dot-force/`, docs menu weight 25, between Projects and The Team). The Projects index carries a short "Incubating" pointer to it.
- **Main nav entry**: the site's footer partial is overridden and renders no footer menu, so the link goes in `[[main]]` (weight 4, after Roadmap).
- **"Trains with" column**: matches the org profile README — reading-stone and snake-eyes with Gaze, review-council with The Divisor; vibe-check has none.

## Risks / Trade-offs

- External links to npm, PyPI and GitHub can drift; they point at stable package and repository URLs.
