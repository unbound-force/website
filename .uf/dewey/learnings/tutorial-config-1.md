---
tag: tutorial-config
category: pattern
created_at: 2026-05-03T19:56:09Z
identity: tutorial-config-1
tier: draft
---

The Unbound Force website session on 2026-05-03 completed all 19 release-ready issues across 12 PRs in a single conversation. The pattern that emerged for maximum throughput: batch propose+unleash+finale cycles, proactively fix the recurring CRITICAL finding (wrong constitution — org vs website project) before running spec review, skip full Divisor council reviews for blog posts after the pattern was validated 3+ times, and use upstream source files (../unbound-force/.opencode/command/*.md, internal/config/config.go, internal/gateway/provider.go, internal/sandbox/config.go) as the authoritative content source with direct file access rather than Dewey semantic search. Key gotchas discovered: ANTHROPIC_API_KEY not ANTHROPIC_AUTH_TOKEN for sandbox env vars, config precedence is CLI > env > repo > user > defaults (not the reverse), and always grep content/ for all instances of a stale pattern before writing fix specs (the /finale merge fix had 12 instances across 5 files including the blog).
