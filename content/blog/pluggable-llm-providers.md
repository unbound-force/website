---
title: "Dewey Goes Cloud-Optional — Pluggable LLM Providers for Embedding and Synthesis"
description: "Dewey's new pluggable provider architecture lets teams choose between local Ollama and cloud Vertex AI for both embedding and synthesis — or mix them. Local-first, cloud-optional."
lead: "Local LLMs produce lower-quality synthesis and require GPU hardware. Dewey's pluggable providers let you mix local Ollama embeddings with cloud Vertex AI synthesis — or go fully local. Your choice."
slug: "pluggable-llm-providers"
date: 2026-08-23T00:00:00+00:00
draft: false
weight: 105
toc: true
categories: ["Engineering"]
tags: ["dewey", "LLM", "providers", "vertex-ai", "ollama"]
contributors: ["Unbound Force"]
---

## The Problem with Local-Only

Dewey started as a local-first knowledge system. Ollama handled both embedding and synthesis, which meant zero cloud dependencies and full data sovereignty. That design worked — until it didn't.

Local LLMs produce measurably lower-quality synthesis than cloud models. Knowledge compilation — where Dewey groups learnings by topic, resolves contradictions, and produces current-state articles — demands reasoning that smaller local models struggle with. The output reads like a summary, not a synthesis. Teams without dedicated GPU hardware hit a second wall: Ollama's embedding models run slowly on CPU, making semantic search across large vaults impractical.

The original architecture forced an all-or-nothing choice. You ran everything locally, or you didn't run Dewey at all. That constraint needed to break.

## Pluggable Provider Interfaces

The `016-pluggable-providers` branch introduces two provider interfaces — `Embedder` and `Synthesizer` — that decouple Dewey's intelligence layer from any specific LLM backend. Each interface has exactly one job: `Embedder` converts text into vector embeddings for semantic search, and `Synthesizer` generates natural language output for knowledge compilation.

Factory functions `NewEmbedderFromConfig()` and `NewSynthesizerFromConfig()` centralize provider construction. They read the configuration, select the correct backend, and return a ready-to-use provider. Callers never import provider-specific packages directly. Adding a new backend — Anthropic, OpenAI, a future local model — means implementing the interface and registering it in the factory. No call sites change.

```go
// Factory selects the provider based on config.
embedder, err := providers.NewEmbedderFromConfig(cfg)
synthesizer, err := providers.NewSynthesizerFromConfig(cfg)
```

Ollama remains the zero-config default. If you install Dewey and do nothing else, it behaves exactly as before — local Ollama for both embedding and synthesis. No API keys, no cloud accounts, no configuration files required.

## Configuration: Two New Fields

Two new fields in Dewey's configuration control provider selection: `embedding.provider` and `synthesis.provider`. Each accepts `ollama` or `vertex-ai` as values.

```yaml
# Per-vault config: .dewey/config.yaml
embedding:
  provider: ollama
  model: nomic-embed-text

synthesis:
  provider: vertex-ai
  model: gemini-2.5-flash
  project: my-gcp-project
  location: us-central1
```

For teams running multiple vaults, a global configuration file at `~/.config/dewey/config.yaml` sets defaults that individual vaults can override. This prevents duplicating Vertex AI credentials across every project. The vault-level config takes precedence when both exist.

```yaml
# Global config: ~/.config/dewey/config.yaml
synthesis:
  provider: vertex-ai
  model: gemini-2.5-flash
  project: my-gcp-project
  location: us-central1
```

### Config Precedence: A Deliberate Asymmetry

Embedding and synthesis handle environment variables differently, and this is intentional. For embedding, environment variables like `DEWEY_EMBEDDING_PROVIDER` override the config file. This preserves backward compatibility — existing CI pipelines and scripts that set environment variables continue to work without modification.

For synthesis, the relationship is inverted: the config file takes precedence, and environment variables serve as a fallback. Synthesis configuration is a deliberate architectural choice that teams make once and commit to version control. Letting a stray environment variable silently swap your synthesis backend would undermine that intentionality. The asymmetry reflects different usage patterns: embedding config is often set dynamically in automation, while synthesis config is a stable team decision.

## Vertex AI: Pure Go, No CGO

The Vertex AI provider authenticates through `golang.org/x/oauth2/google`, which uses Application Default Credentials (ADC). Run `gcloud auth application-default login` once, and Dewey picks up the credentials automatically. No API keys to manage, no secrets to rotate, no CGO dependencies to cross-compile around.

This matters for distribution. Dewey ships as a single static binary. CGO dependencies would force platform-specific builds, complicate `go install`, and break the "download and run" experience. The pure Go constraint was non-negotiable during design.

```bash
# One-time setup for Vertex AI
gcloud auth application-default login
```

## The Recommended Setup: Hybrid Providers

The architecture supports mixing providers — and the hybrid configuration is the recommended setup for most teams. Run Ollama locally for embeddings and route synthesis to Vertex AI.

```yaml
embedding:
  provider: ollama
  model: nomic-embed-text

synthesis:
  provider: vertex-ai
  model: gemini-2.5-flash
  project: my-gcp-project
  location: us-central1
```

This combination plays to each provider's strengths. Ollama's `nomic-embed-text` produces high-quality embeddings with low latency on commodity hardware — no GPU required for the embedding model. Vertex AI's Gemini models handle the heavy reasoning that knowledge compilation demands. Your raw data stays local for indexing and search; only the synthesis prompts (which contain aggregated, anonymized learnings) leave the machine.

Teams that need full data sovereignty can set both providers to `ollama` and accept the synthesis quality trade-off. Teams that want maximum quality can set both to `vertex-ai`. The pluggable architecture makes this a configuration decision, not a code change.

## Closing the Compilation Loop: `store_compiled`

The new `store_compiled` MCP tool completes the agent-driven compilation workflow. Previously, Dewey's `compile` tool returned synthesis prompts that an agent would process, but there was no way to persist the result back into the knowledge graph. The compiled article existed only in the conversation context.

With `store_compiled`, the workflow becomes a closed loop:

1. Agent calls `dewey_compile` — Dewey groups learnings by topic and returns synthesis prompts
2. Agent performs synthesis (using whatever LLM powers the agent)
3. Agent calls `dewey_store_compiled` with the synthesized article, source learnings, and topic tag
4. Dewey persists the compiled article with full provenance metadata

```
compile → synthesize → store_compiled → searchable knowledge
```

This means compiled knowledge articles are now first-class citizens in the graph. They appear in semantic search results, carry provenance tracking (which learnings were compiled, which model performed synthesis), and can be promoted from draft to validated status through the existing `dewey_promote` workflow.

## Constitution Amendment: Local by Default, Cloud Opt-In

This change required amending Dewey's constitution. The original "Local-Only Processing" principle stated that all data processing must happen on the user's machine. That principle served its purpose — it forced the architecture to work without cloud dependencies — but it also prevented teams from opting into cloud services when the trade-off made sense.

The amended principle reads "Local by Default, Cloud Opt-In." Dewey works out of the box with no cloud services. Cloud providers are available for teams that choose them, with explicit configuration required. No data leaves the machine unless the user configures a cloud provider. The amendment preserves the original intent (privacy, zero-config startup) while removing the artificial ceiling on quality.

Constitutional amendments in the Unbound Force ecosystem are not taken lightly. They require explicit justification, documented trade-offs, and alignment with the broader governance model. This amendment passed because it expanded user choice without reducing the default privacy guarantees.

## Get Started

Upgrade to the latest Dewey build from the `016-pluggable-providers` branch:

```bash
go install github.com/unbound-force/dewey/cmd/dewey@latest
```

To add Vertex AI synthesis to an existing vault:

```bash
# Authenticate with GCP
gcloud auth application-default login

# Add synthesis config to your vault
cat >> .dewey/config.yaml << 'EOF'
synthesis:
  provider: vertex-ai
  model: gemini-2.5-flash
  project: YOUR_PROJECT
  location: us-central1
EOF
```

Run `dewey_compile` through your MCP client to test the synthesis quality difference. Compare the output against a pure Ollama compilation of the same learnings. The difference in reasoning depth is the reason this architecture exists.

For questions, issues, or provider requests, open an issue on the [Dewey repository](https://github.com/unbound-force/dewey).
