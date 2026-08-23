# Design: Gaze Multi-Language Documentation Update

## Context

Gaze has expanded from a Go-only tool to a multi-language analysis framework through 4 upstream PRs (gaze#178, gaze#180, gaze#184, gaze#194). The website needs to reflect this evolution across multiple pages while maintaining content accuracy and avoiding overstatement of maturity.

## Goals

- Accurately document Gaze's multi-language capabilities without overstating maturity
- Provide clear migration guidance for breaking changes
- Maintain cross-page consistency in the multi-language narrative
- All descriptions sourced from upstream PR descriptions and issue bodies

## Non-Goals

- Writing a full external analyzer development tutorial (that belongs in-repo at `docs/protocol.md`)
- Documenting the streaming protocol mode in detail (too implementation-specific for the website)
- Creating a separate "Multi-Language" landing page (not warranted by current maturity)
- Updating the blog posts (they describe Gaze at a point in time and remain accurate for that context)

## Decisions

### D1: In-page migration section (not separate page)

Migration notes for the JSON field rename and coverage behavior change will be added as a `## Migration Notes` section at the bottom of the Gaze project page, before "Learn More." A separate migration guide page would become stale after one release cycle and violate the Zero-Waste Mandate.

### D2: Taxonomy depth — list by tier, don't reproduce full table

The expanded taxonomy will list the 10 new universal types by tier (P0, P1, P2) with brief descriptions, then link to `docs/protocol.md` for the full 48-type reference table. Reproducing the full table on the website would create a maintenance burden and risk content drift (R3).

### D3: Homepage badge — "Go + Multi-Language"

The Gaze card badge on the homepage will change from "Go" to "Go + Multi-Language" to signal the expanded scope. The card description will not change — it already describes side effects and CRAP scores in language-agnostic terms. Protocol details do not belong on a homepage teaser.

### D4: `--test-short` in tester guide CI section

The `--test-short` flag will be documented in the tester guide's CI Integration Pattern section, where users encounter coverage configuration. A migration callout blockquote will alert users to the behavior change.

### D5: Constitution alignment confirmed

Observable Quality (Principle III) is the most relevant principle — documenting machine-parseable output changes supports users building integrations. Testability (Principle IV) is PASS per the project's manual validation model. Principles I, II are N/A (no agent changes). Principle V is N/A (no security impact).

## Risks

### R1: Upstream PRs not yet merged

Some upstream PRs (gaze#184, gaze#194) may not be merged when this website change is ready. **Mitigation**: The website change should be merged to `main` only after verifying all upstream PRs are merged. If any PR slips, defer the corresponding documentation sections or use `draft: true`.

### R2: JSON example accuracy

The migration notes document field renames and new fields. If the upstream implementation changes field names before merge, the website content becomes inaccurate on day one. **Mitigation**: Document field names and types rather than full JSON examples, reducing the accuracy surface area. Verify against merged code before website merge.

### R3: Side effect list staleness

The expanded taxonomy (48 types) will grow as new analyzers are added. **Mitigation**: D2 avoids reproducing the full reference table — the website lists the 10 new universal types and links to `docs/protocol.md` for the authoritative list. This bounds the maintenance obligation to the summary, not the exhaustive reference.
