---
tag: critical-doc-fixes
category: gotcha
created_at: 2026-05-02T22:08:02Z
identity: critical-doc-fixes-1
tier: draft
---

When fixing factual errors in the Unbound Force website documentation, always grep all files under content/ for every stale pattern being corrected before writing the spec. The critical-doc-fixes change initially scoped /finale merge corrections to 3 docs pages but the Adversary spec reviewer found a 4th instance in blog/unleash-in-practice.md and 3 additional instances in developer.md (Session Lifecycle table, Session Ritual prose, Session Ritual code block) beyond the single reference originally targeted. The blog post was entirely missed from the original scope. Always run a site-wide search for stale patterns before writing the spec, not just as a verification task after implementation.
