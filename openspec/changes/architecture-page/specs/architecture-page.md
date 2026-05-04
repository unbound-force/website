## ADDED Requirements

### Requirement: Architecture Documentation Page

The website MUST include a documentation page at `content/docs/getting-started/architecture.md` that presents the overall system architecture and design philosophy of Unbound Force.

The page MUST cover the following architectural concepts:
1. Three-tier context system (static docs, versioned rules, dynamic semantic memory)
2. Layered governance model (constitution > convention packs > agent personas > commands > CI)
3. Control matrix (feedforward/feedback x computational/inferential)
4. Doer/judge separation (tool access restrictions on review agents)
5. Plan/execute separation (8-phase Speckit pipeline with hard gates)
6. Composability (independently useful components, graceful degradation)

The page MUST use standard Hugo/Doks Markdown with YAML frontmatter. No custom HTML, CSS, or template overrides.

The page SHOULD frame content for a website audience ("why does this matter") rather than reproducing raw technical implementation details.

The page MUST attribute any claims derived from the Yanli Liu "Harness Engineering" article to their source.

The page MUST NOT present "harness engineering" as Unbound Force's primary identity or branding.

#### Scenario: Visitor reads architecture page

- **GIVEN** a visitor navigates to the Getting Started section
- **WHEN** they click on the Architecture & Design Philosophy page
- **THEN** they see a page covering all six architectural concepts with cross-references to existing pages for deeper detail

#### Scenario: Architecture page builds without errors

- **GIVEN** the architecture page exists at `content/docs/getting-started/architecture.md`
- **WHEN** `npm run build` is executed
- **THEN** the build completes successfully with no errors or warnings related to the new page

### Requirement: Frontmatter Convention

The page MUST include YAML frontmatter with: title, description, lead, date, draft (false), weight (82), and toc (true).

#### Scenario: Page appears in correct sidebar position

- **GIVEN** the architecture page has weight 82
- **WHEN** the getting-started section renders in the sidebar
- **THEN** the page appears after knowledge.md (weight 80) and before constitution.md (weight 85)

### Requirement: Cross-Reference Accuracy

The page MUST only link to pages that exist on the current branch. Cross-references SHOULD use relative paths.

#### Scenario: All cross-references resolve

- **GIVEN** the architecture page contains links to other documentation pages
- **WHEN** a visitor clicks any cross-reference link
- **THEN** the link resolves to an existing page (no 404 errors)

## MODIFIED Requirements

None.

## REMOVED Requirements

None.
