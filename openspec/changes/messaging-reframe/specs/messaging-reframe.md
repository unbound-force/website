## ADDED Requirements

### Requirement: Homepage Harness Engineering Feature Card

The homepage MUST include a feature card in the "Why Unbound Force?" section that speaks to the harness engineering audience.

The card MUST convey the feedforward/feedback control concept without requiring knowledge of the Liu article.

The card MUST NOT rebrand Unbound Force around harness engineering.

The card MUST follow the existing feature card HTML pattern (no new CSS or JavaScript).

#### Scenario: Visitor sees harness engineering card

- **GIVEN** a visitor navigates to the homepage
- **WHEN** they scroll to the "Why Unbound Force?" section
- **THEN** they see a feature card describing the harness architecture (feedforward + feedback controls)

### Requirement: Getting-Started Design Philosophy Section

The getting-started index page MUST include a "Design Philosophy" section explaining the architectural principles behind Unbound Force.

The section SHOULD cover: the Agent = Model + Harness concept, the three-tier context system, and layered feedback.

The section MUST be pure Markdown (no custom HTML).

#### Scenario: Visitor reads design philosophy

- **GIVEN** a visitor navigates to the "What is Unbound Force?" page
- **WHEN** they scroll past the heroes and get started sections
- **THEN** they see a design philosophy section explaining the architectural principles

#### Scenario: Build succeeds with changes

- **GIVEN** the homepage and getting-started index are modified
- **WHEN** `npm run build` is executed
- **THEN** the build completes successfully

## MODIFIED Requirements

None.

## REMOVED Requirements

None.
