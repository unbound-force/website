---
tag: website-content-review
author: jay-flowers
category: gotcha
created_at: 2026-11-25T19:42:09Z
identity: website-content-review-20261125T194209-jay-flowers
tier: draft
as_of: "website@e481e6bbbe0d05dcd3a609769e355f32b66e0329"
evidence:
  - "session:ses_debce1aceffevMGcFdQGwEQK3t"
---

GitHub silently redirects repository URLs across org transfers: `gh repo view unbound-force/review-council` succeeds and resolves to zero-dot-force/review-council. A green existence check on a URL does not prove it is canonical — when a site publishes install commands containing repo URLs (e.g. `lola mod add https://github.com/.../review-council.git`), confirm the URL matches the repo's current owner rather than a redirecting predecessor. Website PR #453 published the canonical zero-dot-force URL while the upstream review-council README still carried the pre-transfer unbound-force URL (worth an upstream fix so the two don't diverge).
