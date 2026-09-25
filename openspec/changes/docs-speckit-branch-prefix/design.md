## Context

The website documents two specification workflows: Speckit uses numbered feature branches and OpenSpec uses the `opsx/` namespace. The current documentation still describes Speckit branches as `NNN-<short-name>`, although the current convention is `speckit/NNN-<short-name>`. Issue #205 requests a documentation correction based on the upstream tooling change.

The affected references are primarily `AGENTS.md`, `content/docs/contributing/_index.md`, and `content/blog/the-8-phase-pipeline.md`; implementation should search the complete repository for additional user-facing occurrences before editing.

## Goals / Non-Goals

### Goals

- Make every current Speckit branch example use `speckit/NNN-<short-name>`.
- Explain that the prefix applies to new branches and that existing branches are not renamed.
- Preserve the separate `opsx/<change-name>` OpenSpec convention.
- Keep documentation links, frontmatter, navigation, and page structure intact.
- Validate the documentation with exact-reference searches and the normal Hugo production build.

### Non-Goals

- Do not change Speckit or OpenSpec command implementations.
- Do not rename existing Git branches.
- Do not modify CI, templates, styles, or navigation unless a stale branch example is found in one of those files.
- Do not introduce a new website page solely for this convention correction.

## Decisions

1. **Use repository-wide search before editing.** Search for the old `NNN-<name>` and related branch wording, then classify each match as current guidance, historical context, or unrelated numbering. This avoids both stale user guidance and accidental changes to legitimate spec paths such as `specs/NNN-name/`.

2. **Update guidance in place.** The existing contributor and workflow pages already own this information, so corrections should be made in those pages rather than duplicating the convention in a new location.

3. **Clarify compatibility explicitly.** Documentation will state that only newly created Speckit branches use the `speckit/` prefix; existing branches are not renamed. This prevents readers from interpreting the convention update as a migration requirement.

4. **Preserve namespace distinction.** References to OpenSpec will continue to use `opsx/<name>` and will be checked after editing so the two workflows remain distinguishable.

5. **Use documentation validation as the quality gate.** The implementation will verify no stale user-facing branch examples remain, confirm required new examples exist, and run `npm run build`. This supports the proposal's constitution assessment: artifact-based collaboration remains intact, no coupling is added, and quality is observable through deterministic checks.

## Risks / Trade-offs

- Some historical articles may intentionally describe an older convention. Those references should be updated when presented as current guidance, while historical context may be retained only if clearly labeled.
- `AGENTS.md` contains repository governance rather than end-user site content, but leaving its branch instructions stale would create conflicting contributor guidance; it should therefore be updated alongside published docs.
- A documentation-only change has no automated content test suite, so the build and targeted searches are the practical regression checks.
