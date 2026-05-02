---
tag: gateway-docs
category: context
created_at: 2026-05-02T22:31:32Z
identity: gateway-docs-1
tier: draft
---

When writing gateway documentation for the Unbound Force website, the Bedrock credential acquisition mechanism is more nuanced than simply reading AWS environment variables. The upstream gateway code at internal/gateway/refresh.go acquires credentials by executing `aws configure export-credentials --format env`, not by reading env vars directly. The env vars (AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_SESSION_TOKEN) are what the CLI outputs, not what the gateway reads as input. For website documentation, describing the credential values used is a reasonable simplification, but a note like "acquired via the AWS CLI" would improve accuracy without adding complexity.
