## ADDED Requirements

### Requirement: Tutorials Section Infrastructure

The site MUST have a tutorials section at `content/docs/tutorials/` with a section index page (`_index.md`) and a navigation entry in `config/_default/menus/menus.en.toml` at weight 37.

#### Scenario: Tutorials section appears in docs sidebar
- **GIVEN** the tutorials `_index.md` exists with proper frontmatter
- **WHEN** a user navigates to `/docs/tutorials/`
- **THEN** the section index page MUST render and tutorials MUST appear in the docs sidebar navigation

#### Scenario: Navigation ordering is correct
- **GIVEN** the tutorials menu entry has weight 37
- **WHEN** the docs sidebar renders
- **THEN** "Tutorials" MUST appear after "Reference" (weight 35) and before "Changelog" (weight 38)

### Requirement: Tutorial Frontmatter

Each tutorial MUST include YAML frontmatter with: `title`, `description`, `lead`, `date`, `draft` (set to `false`), `weight`, and `toc` (set to `true`).

#### Scenario: Valid tutorial frontmatter
- **GIVEN** a new tutorial Markdown file in `content/docs/tutorials/`
- **WHEN** Hugo processes the file during build
- **THEN** the file MUST have all required frontmatter fields and `npm run build` MUST succeed

### Requirement: Tutorial Body Structure

Each tutorial MUST start body content with `##` (H2) headings. Tutorials SHOULD include step-by-step instructions with numbered sections, configuration examples in fenced code blocks, and prerequisite information.

#### Scenario: Step-by-step structure
- **GIVEN** a tutorial page
- **WHEN** reviewed for instructional structure
- **THEN** the content MUST contain sections for: (1) prerequisites, (2) installation or setup steps, and (3) usage or verification steps

### Requirement: Dewey Knowledge Stores Tutorial (#70)

The site MUST include a tutorial at `content/docs/tutorials/dewey-knowledge-stores.md` covering knowledge store configuration, curation workflow, and semantic search setup.

#### Scenario: Tutorial renders at correct URL
- **GIVEN** the tutorial file exists in the tutorials directory
- **WHEN** a user navigates to `/docs/tutorials/dewey-knowledge-stores/`
- **THEN** the page MUST render with the full tutorial content including prerequisites, configuration, and verification steps

### Requirement: Dewey Provider Configuration Tutorial (#114)

The site MUST include a tutorial at `content/docs/tutorials/dewey-provider-configuration.md` covering Ollama and Vertex AI provider setup for embedding and synthesis.

#### Scenario: Tutorial renders at correct URL
- **GIVEN** the tutorial file exists in the tutorials directory
- **WHEN** a user navigates to `/docs/tutorials/dewey-provider-configuration/`
- **THEN** the page MUST render with configuration examples for both Ollama and Vertex AI providers

### Requirement: Gaze CRAP Baseline CI Tutorial (#151)

The site MUST include a tutorial at `content/docs/tutorials/gaze-crap-baseline-ci.md` covering baseline file generation, CI integration with GitHub Actions, and regression detection.

#### Scenario: Tutorial renders at correct URL
- **GIVEN** the tutorial file exists in the tutorials directory
- **WHEN** a user navigates to `/docs/tutorials/gaze-crap-baseline-ci/`
- **THEN** the page MUST render with GitHub Actions workflow examples and baseline comparison setup

### Requirement: Pinkman OSS Scout Tutorial (#45)

The site MUST include a tutorial at `content/docs/tutorials/pinkman-oss-scout.md` covering the `/scout` command's four modes: discover, trend, audit, and report.

#### Scenario: Tutorial renders at correct URL
- **GIVEN** the tutorial file exists in the tutorials directory
- **WHEN** a user navigates to `/docs/tutorials/pinkman-oss-scout/`
- **THEN** the page MUST render with usage examples for all four scout modes

### Requirement: Onboard Customization Tutorial (#46)

The site MUST include a tutorial at `content/docs/tutorials/onboard-customization.md` covering the `/onboard` command's three interview phases and hero experience customization.

#### Scenario: Tutorial renders at correct URL
- **GIVEN** the tutorial file exists in the tutorials directory
- **WHEN** a user navigates to `/docs/tutorials/onboard-customization/`
- **THEN** the page MUST render with the three interview phases and customization options

### Requirement: Build Validation

After all content files are created, `npm run build` MUST succeed without errors. All new pages MUST appear in the site output.

#### Scenario: Clean build with all new content
- **GIVEN** all 9 blog posts, 5 tutorials, and 1 section index have been created
- **WHEN** `npm run build` is executed
- **THEN** the build MUST complete successfully with exit code 0 and all 15 new pages MUST be present in the `public/` output directory

## MODIFIED Requirements

None.

## REMOVED Requirements

None.
