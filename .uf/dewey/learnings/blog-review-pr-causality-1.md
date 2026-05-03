---
tag: blog-review-pr-causality
category: reference
created_at: 2026-05-03T18:08:30Z
identity: blog-review-pr-causality-1
tier: draft
---

When writing blog posts about /review-pr for the Unbound Force website, the CI causality classification uses a 2x2 matrix based on base branch status and PR check status, with three classifications: PR-caused (base pass, PR fail), pre-existing (base fail, PR fail), and unknown (no base data, PR fail — treated as PR-caused conservatively). The fix branch naming pattern is fix/pr-NUMBER-sanitized-check-name. The in-line comment cap is exactly 15 comments, with CRITICAL prioritized over HIGH when the cap is exceeded. The dirty-tree guard checks git status --porcelain before creating fix branches. All of these details are verified from the upstream command file at .opencode/command/review-pr.md lines 132-139, 428-496, and 519-523.
