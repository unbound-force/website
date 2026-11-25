## ADDED Requirements

### Requirement: Zero Dot Force Page

The site MUST have a page at `/docs/zero-dot-force/` that introduces Zero Dot Force as Unbound Force's junior hero team and describes vibe-check, reading-stone, snake-eyes and review-council, each linked to its repository.

The page MUST NOT assign hero names to the junior projects.

#### Scenario: Page renders

- **GIVEN** the site is built
- **WHEN** a visitor opens `/docs/zero-dot-force/`
- **THEN** the page lists all four projects with repository and install links

#### Scenario: Reachable from navigation

- **GIVEN** a visitor is on any page
- **WHEN** they look at the main nav or the docs sidebar
- **THEN** a "Zero Dot Force" entry links to `/docs/zero-dot-force/`

## MODIFIED Requirements

### Requirement: Junior Project Cross-References

The Gaze project and hero pages MUST name reading-stone and snake-eyes as Gaze's TypeScript and Python analyzers. The Divisor page MUST link review-council.

#### Scenario: Cross-links resolve

- **GIVEN** the cross-links are added
- **WHEN** a visitor follows any new internal link
- **THEN** the link resolves to an existing page

#### Scenario: Build succeeds

- **GIVEN** all content changes are in place
- **WHEN** `npm run build` is executed
- **THEN** the build completes without errors

## REMOVED Requirements

None.
