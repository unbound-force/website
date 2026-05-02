---
tag: config-docs
category: gotcha
created_at: 2026-05-02T21:32:17Z
identity: config-docs-2
tier: draft
---

The uf config layered loading precedence order is CLI flags > env vars > repo config > user config > compiled defaults. This follows the standard convention where more-specific sources win. The original OpenSpec proposal for config-docs had the order inverted (user > repo > env), which was caught during spec review by verifying against the actual source code comment in internal/config/config.go. Always verify precedence orders against the actual implementation source code — spec artifacts may contain assumptions that are wrong.
