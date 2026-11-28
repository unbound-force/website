---
tag: ci-gating
author: jay-flowers
category: pattern
created_at: 2026-11-28T00:22:08Z
identity: ci-gating-20261128T002208-jay-flowers
tier: draft
as_of: "website@1987bb7484c6fb63b570ce25c4584c95a9ca9dfa"
evidence:
  - "unbound-force/website#467"
  - "unbound-force/gaze#362"
  - "unbound-force/replicator#189"
---

When gating a repo's required check through a SHA-pinned reusable workflow, the effective input contract (paths, author, branch patterns that trigger the skip) lives in the external repo at the pin — accurate but non-local. Prefer explicit `with:` inputs (learnings_dir, toolkit_paths, toolkit_author) or a comment naming the effective defaults at the call site so the contract is self-documenting and immune to default drift on pin bumps. Pair the always-run aggregate job with a want-mapping that verifies the gate wiring itself (build ran iff code==true, skipped iff code==false) rather than just job outcomes, so gate drift fails loudly; and word required-check header comments as pending with a dependency marker until the branch-protection change actually lands (replicator#189 precedent: headers claiming an end state that doesn't exist yet mislead maintainers in the interim).
