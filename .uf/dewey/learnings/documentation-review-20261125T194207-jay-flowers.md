---
tag: documentation-review
author: jay-flowers
category: pattern
created_at: 2026-11-25T19:42:07Z
identity: documentation-review-20261125T194207-jay-flowers
tier: draft
as_of: "website@e481e6bbbe0d05dcd3a609769e355f32b66e0329"
evidence:
  - "dewey:openspec/changes/zero-dot-force-juniors/proposal.md"
  - "dewey:content/docs/vision/_index.md"
  - "session:ses_debce5275ffe6Bi7EVQmWlYvxQ"
---

When an OpenSpec proposal's Why cites existing unlinked mentions as motivation (e.g., "the website mentions X in the vision and roadmap"), grep the whole content tree for those terms and enumerate every carrying page in What Changes. In website PR #453, the Why named both the vision and roadmap pages but What Changes scoped links to the roadmap only, leaving three of four tool mentions on the Vision page — a top-nav page — without a path to the new page. The fix at review time is three links; the fix at proposal time is enumerating all carrying pages up front.
