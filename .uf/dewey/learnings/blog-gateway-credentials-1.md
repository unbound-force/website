---
tag: blog-gateway-credentials
category: gotcha
created_at: 2026-05-03T17:29:17Z
identity: blog-gateway-credentials-1
tier: draft
---

When writing blog posts about the Unbound Force gateway for the website, the environment variable injected into sandbox containers is ANTHROPIC_API_KEY (not ANTHROPIC_AUTH_TOKEN), and the placeholder value is "gateway" (not "gateway-proxy"). This was caught as a HIGH finding during code review for the blog-gateway-credentials change. The actual source is internal/sandbox/config.go line 79 which sets ANTHROPIC_API_KEY=gateway. The ANTHROPIC_AUTH_TOKEN name does not exist in the codebase. Always verify environment variable names against the actual source code, even when they seem obvious from the naming convention — the Anthropic SDK uses ANTHROPIC_API_KEY while the concept of an "auth token" is a gateway abstraction that doesn't appear in the implementation.
