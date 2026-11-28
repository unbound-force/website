---
tag: dewey-learnings
author: jay-flowers
category: gotcha
created_at: 2026-11-27T00:14:04Z
identity: dewey-learnings-20261127T001404-jay-flowers
tier: draft
as_of: "website@48e37b99af109e32646797fda0eb3eb7e01ed81c"
---

Learnings-only PRs (all changed files under .uf/dewey/learnings/) skip build/test/lint CI entirely, and gh pr create --fill bodies come from commit messages — so chore/dewey-learnings PRs have no automated content review unless the publish flow's screening runs.
