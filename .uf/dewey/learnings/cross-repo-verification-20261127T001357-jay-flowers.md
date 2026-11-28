---
tag: cross-repo-verification
author: jay-flowers
category: pattern
created_at: 2026-11-27T00:13:57Z
identity: cross-repo-verification-20261127T001357-jay-flowers
tier: draft
as_of: "website@48e37b99af109e32646797fda0eb3eb7e01ed81c"
---

Dewey indexes the unbound-force sibling repo's openspec/ changes and proposals, docs/usage, docs/heroes, CHANGELOG, and scaffold command assets, but not its .opencode/ pack and persona files. In website review sessions where direct sibling-repo file reads are permission-blocked, those indexed pages verify website claims about uf command behavior (step numbering, thresholds, triage options, permission allowlists) down to verbatim detail; claims about persona frontmatter (bash deny lists, temperature) cannot be verified that way and must be reported as unverified. Workflow: semantic search finds the spec deltas, then dewey_get_page confirms exact wording.
