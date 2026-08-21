## ADDED Requirements

### Requirement: Council Review Action Reference Page

A new reference page MUST be created at `content/docs/reference/council-review-action.md` that documents the council-review-action composite GitHub Action. The page MUST cover:

- What the action does (AI-powered code review as a GitHub Action)
- The three-workflow chain architecture (trigger → review → comment)
- Persona discovery mechanism (how review personas are found and invoked)
- Configuration options (inputs, secrets, environment variables). The Configuration section MUST include a security note explaining that secrets should be scoped to the narrowest context needed (repository-level, not organization-level) and that workflow permissions should follow the principle of least privilege
- Integration with the review lifecycle (how it relates to `/review-council` and `/review-pr`)
- Cross-reference to the code review tutorial for workflow context

The page MUST use standard Hugo frontmatter with `weight: 40` in the reference section.

#### Scenario: User understands the three-workflow chain

- **GIVEN** a repository maintainer wants to understand how the council-review-action works
- **WHEN** they read the Council Review Action reference page
- **THEN** the page explains the trigger workflow, review workflow, and comment workflow, including how they chain together

#### Scenario: User finds configuration options

- **GIVEN** a user is configuring the council-review-action for their repository
- **WHEN** they read the Configuration section of the reference page
- **THEN** they find all required and optional inputs, secrets, and environment variables with descriptions

### Requirement: Council Review Action Tutorial

A new tutorial page MUST be created at `content/docs/getting-started/council-review-action-tutorial.md` that provides a step-by-step adoption guide for adding the council-review-action to a downstream repository.

The tutorial MUST include:

- Prerequisites (GitHub repository, required permissions, secrets setup)
- Step-by-step instructions for adding the workflow files
- Example workflow YAML configuration using `${{ secrets.SECRET_NAME }}` syntax for all credential references (MUST NOT include placeholder strings resembling real tokens). A callout SHOULD warn users to never hardcode API keys or tokens in workflow files
- How to verify the action is working (creating a test PR)
- Troubleshooting common setup issues
- Link to the reference page for advanced configuration

The tutorial MUST follow the pattern established by the existing code-review-tutorial.md. The tutorial MUST use Hugo frontmatter with `weight: 75` (after core workflow tutorials). The introduction paragraph SHOULD frame the action as assisting human reviewers, not replacing them.

#### Scenario: User adds council-review-action to their repo

- **GIVEN** a repository maintainer wants automated AI code review on pull requests
- **WHEN** they follow the council-review-action tutorial from start to finish
- **THEN** they have a working GitHub Action that runs AI code review on new PRs

#### Scenario: User troubleshoots a failed setup

- **GIVEN** a user followed the tutorial but the action is not running
- **WHEN** they read the troubleshooting section
- **THEN** they find guidance for common issues (missing secrets, incorrect permissions, workflow trigger configuration)

## MODIFIED Requirements

None.

## REMOVED Requirements

None.
