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

- Do not change Speckit command behavior beyond what `speckit/` branch-prefix handling requires, and do not change OpenSpec tooling.
- Do not rename existing Git branches.
- Do not modify CI, templates, styles, or navigation unless a stale branch example is found in one of those files.
- Do not introduce a new website page solely for this convention correction.
- Do not perform a wholesale synchronization of the vendored `.specify/scripts/bash` scripts; only the `speckit/` prefix handling is backported.

## Decisions

1. **Use repository-wide search before editing.** Search for the old `NNN-<name>` and related branch wording, then classify each match as current guidance, historical context, or unrelated numbering. This avoids both stale user guidance and accidental changes to legitimate spec paths such as `specs/NNN-name/`.

2. **Update guidance in place.** The existing contributor and workflow pages already own this information, so corrections should be made in those pages rather than duplicating the convention in a new location.

3. **Clarify compatibility explicitly.** Documentation will state that only newly created Speckit branches use the `speckit/` prefix; existing branches are not renamed. This prevents readers from interpreting the convention update as a migration requirement.

4. **Preserve namespace distinction.** References to OpenSpec will continue to use `opsx/<name>` and will be checked after editing so the two workflows remain distinguishable.

5. **Use documentation validation as the quality gate.** The implementation will verify no stale user-facing branch examples remain, confirm required new examples exist, and run `npm run build`. This supports the proposal's constitution assessment: artifact-based collaboration remains intact, no coupling is added, and quality is observable through deterministic checks.

6. **Backport the branch gate instead of documenting around it.** The vendored `.specify/scripts/bash` scripts predate the `speckit/` prefix: `common.sh` rejects branches lacking a three-digit prefix and `create-new-feature.sh` emits unprefixed names. Publishing the new convention while the repository's own hard gate rejects it would make the guidance false. Issue #205 is documentation-focused, so the backport is authorized by the PR #286 review feedback requiring the guidance to match a working gate. The prefix handling is applied to `common.sh` and `create-new-feature.sh`, with only the `--paths-only` help text clarified in `check-prerequisites.sh`; the remaining behavior of the other vendored scripts is left untouched to avoid pulling unrelated upstream refactoring into this change. The scripts report and emit the real branch ref (`speckit/NNN-<name>`), while feature-directory resolution strips the prefix.

## Risks / Trade-offs

- Some historical articles may intentionally describe an older convention. Those references should be updated when presented as current guidance, while historical context may be retained only if clearly labeled.
- `AGENTS.md` contains repository governance rather than end-user site content, but leaving its branch instructions stale would create conflicting contributor guidance; it should therefore be updated alongside published docs.
- A documentation-and-scaffolding change has no automated content test suite, so the build, targeted searches, and script smoke tests are the practical regression checks.
- The backported scripts are vendored scaffolding. A later wholesale synchronization from the canonical repository may supersede these edits; the backport keeps behavior aligned with the canonical prefix handling in the meantime.
