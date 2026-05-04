## ADDED Requirements

None (modifications only).

## MODIFIED Requirements

### Requirement: Cross-Reference Links

Seven page pairs MUST have cross-reference links added as specified in issue #96.

All links MUST point to pages that exist on the current branch.

#### Scenario: All cross-references resolve

- **GIVEN** the cross-reference links are added
- **WHEN** a visitor clicks any of the new links
- **THEN** the link resolves to an existing page

#### Scenario: Build succeeds

- **GIVEN** all pages are modified with cross-reference additions
- **WHEN** `npm run build` is executed
- **THEN** the build completes successfully

## REMOVED Requirements

None.
