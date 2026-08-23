---
title: "Configuring Dewey Embedding and Synthesis Providers"
description: "Step-by-step guide for configuring Dewey's pluggable embedding and synthesis providers — covering both Ollama (default, local) and Vertex AI (cloud)."
lead: "Switch between local Ollama and cloud Vertex AI for embeddings and synthesis. Mix providers for the best of both worlds."
date: 2026-08-23T00:00:00+00:00
draft: false
weight: 20
toc: true
---

## Prerequisites

Before configuring providers, make sure you have Dewey installed and the provider-specific dependencies ready.

**Dewey installation** — install via Homebrew or Go:

```bash
# Homebrew
brew install unbound-force/tap/dewey

# Or from source
go install github.com/unbound-force/dewey/cmd/dewey@latest
```

**For Ollama (local embeddings and synthesis):**

1. Install Ollama from [ollama.com](https://ollama.com).
2. Pull an embedding model:

```bash
ollama pull nomic-embed-text
```

3. Verify Ollama is running:

```bash
ollama list
```

**For Vertex AI (cloud embeddings and synthesis):**

1. Create or select a GCP project with the Vertex AI API enabled.
2. Install the `gcloud` CLI from [cloud.google.com/sdk](https://cloud.google.com/sdk).
3. Authenticate with application default credentials:

```bash
gcloud auth application-default login
gcloud config set project your-project-id
```

## Default Setup (Ollama)

Dewey works out of the box with Ollama — no configuration file required. When you run Dewey without a `config.yaml`, it connects to Ollama on `localhost:11434` and uses `nomic-embed-text` for embeddings.

Start Ollama, then run Dewey:

```bash
ollama serve &
dewey serve
```

Dewey automatically detects the local Ollama instance and begins generating embeddings for your vault content. No API keys, no cloud credentials, no YAML files. This is the fastest path from install to working semantic search.

## Configuring Vertex AI Embeddings

Vertex AI embeddings use Google's `text-embedding-005` model, which produces higher-quality vectors than local models for most workloads. The trade-off is network latency and GCP billing.

### Step 1: Authenticate with GCP

Ensure your application default credentials are set:

```bash
gcloud auth application-default login
```

### Step 2: Create or edit your vault-level config

Create a `config.yaml` in your Dewey vault directory (the directory containing your Logseq graph):

```yaml
embedding:
  provider: vertexai
  model: text-embedding-005
  project: your-project-id
  location: us-central1
```

### Step 3: Reindex your vault

Switching embedding providers changes the vector space. Existing embeddings are incompatible with the new model's dimensions. You must reindex:

```bash
dewey reindex
```

This regenerates all embeddings using the Vertex AI model. Depending on vault size, this may take several minutes and incur GCP API costs.

### Step 4: Verify

Run a semantic search to confirm the new embeddings are working:

```bash
dewey search "your test query"
```

You should see results ranked by the Vertex AI embedding model's similarity scores.

## Configuring Vertex AI Synthesis

Synthesis is the provider Dewey uses for compiling learnings into knowledge articles. Vertex AI synthesis uses Claude models via Google's `rawPredict` endpoint, giving you access to Anthropic's models through your GCP project.

### Step 1: Add synthesis configuration

Add the `synthesis` block to your `config.yaml`:

```yaml
synthesis:
  provider: vertexai
  model: claude-sonnet-4-20250514
  project: your-project-id
  location: us-east5
```

The `location` for synthesis may differ from your embedding location. Claude models on Vertex AI are available in specific regions — check [Google's model availability docs](https://cloud.google.com/vertex-ai/docs/general/locations) for current region support.

### Step 2: Verify credentials

Synthesis uses the same application default credentials as embeddings. If you already authenticated for embedding configuration, no additional credential setup is needed.

### Step 3: Test compilation

Trigger a compile to verify synthesis works:

```bash
dewey compile
```

Dewey will use the configured Claude model to synthesize stored learnings into compiled knowledge articles.

## Mixing Providers

The recommended production setup uses Ollama for embeddings and Vertex AI for synthesis. This combination keeps embedding generation fast and local (no network round-trips, no per-query costs) while using Claude's superior language capabilities for knowledge synthesis.

### Recommended hybrid config

```yaml
embedding:
  provider: ollama
  model: nomic-embed-text

synthesis:
  provider: vertexai
  model: claude-sonnet-4-20250514
  project: your-project-id
  location: us-east5
```

This setup means:

- **Embeddings** are generated locally by Ollama. Indexing and semantic search stay fast and free.
- **Synthesis** uses Claude via Vertex AI. Compilation produces higher-quality knowledge articles.
- **No reindex required** if you were already using Ollama for embeddings — the vector space hasn't changed.

You can also reverse the mix (Vertex AI embeddings + Ollama synthesis), though this is less common. The key constraint is that embedding and synthesis providers are fully independent — changing one does not affect the other.

## Global Config

For developers working across multiple Dewey vaults, a global configuration file avoids duplicating provider settings in every vault.

### Step 1: Create the global config directory

```bash
mkdir -p ~/.config/dewey
```

### Step 2: Add your global config

Create `~/.config/dewey/config.yaml` with your shared provider settings:

```yaml
embedding:
  provider: ollama
  model: nomic-embed-text

synthesis:
  provider: vertexai
  model: claude-sonnet-4-20250514
  project: your-project-id
  location: us-east5
```

### Step 3: Override per vault (optional)

Any vault-level `config.yaml` takes precedence over the global config. Place a `config.yaml` in a specific vault directory to override global settings for that vault only.

This is useful when one vault needs a different embedding model or a different GCP project for billing isolation.

## Config Precedence

Dewey resolves configuration from three sources, but the precedence rules differ between embedding and synthesis. This asymmetry is a deliberate backward-compatibility decision.

### Embedding precedence (env vars win)

For embedding configuration, environment variables override the config file:

1. **Environment variables** (highest priority) — `DEWEY_EMBEDDING_PROVIDER`, `DEWEY_EMBEDDING_MODEL`, `DEWEY_VERTEX_PROJECT`, `DEWEY_VERTEX_LOCATION`
2. **Vault-level config** — `config.yaml` in the vault directory
3. **Global config** (lowest priority) — `~/.config/dewey/config.yaml`

```bash
# Force Ollama embeddings regardless of config file
DEWEY_EMBEDDING_PROVIDER=ollama dewey serve
```

### Synthesis precedence (config file wins)

For synthesis configuration, the config file takes priority and environment variables serve as fallback only:

1. **Vault-level config** (highest priority) — `config.yaml` in the vault directory
2. **Global config** — `~/.config/dewey/config.yaml`
3. **Environment variables** (lowest priority, fallback only) — `DEWEY_SYNTHESIS_PROVIDER`, `DEWEY_SYNTHESIS_MODEL`

This means setting `DEWEY_SYNTHESIS_PROVIDER=ollama` in your environment has no effect if your `config.yaml` specifies `synthesis.provider: vertexai`. The config file wins.

### Why the asymmetry?

Embedding provider selection was originally controlled exclusively through environment variables. When config file support was added, environment variables retained their override behavior to avoid breaking existing setups. Synthesis support was added later with config-file-first semantics from the start. The result is an intentional asymmetry that preserves backward compatibility for embedding users while giving synthesis users the cleaner config-file-first model.

## Troubleshooting

### Credential errors with Vertex AI

**Symptom:** `could not find default credentials` or `permission denied` errors when using Vertex AI.

**Fix:**

1. Re-authenticate with application default credentials:

```bash
gcloud auth application-default login
```

2. Verify your project is set correctly:

```bash
gcloud config get-value project
```

3. Confirm the Vertex AI API is enabled in your GCP project:

```bash
gcloud services list --enabled | grep aiplatform
```

If the API is not listed, enable it:

```bash
gcloud services enable aiplatform.googleapis.com
```

### Model not found

**Symptom:** `model not found` errors during embedding or synthesis.

**Fix for Ollama:** Pull the model explicitly:

```bash
ollama pull nomic-embed-text
```

**Fix for Vertex AI:** Verify the model name matches a supported model in your configured region. Model availability varies by region — `text-embedding-005` and Claude models are not available in every Vertex AI location.

### Dimension mismatch after provider switch

**Symptom:** Semantic search returns no results or garbage results after switching embedding providers.

**Cause:** Different embedding models produce vectors with different dimensions. Ollama's `nomic-embed-text` produces 768-dimensional vectors. Vertex AI's `text-embedding-005` produces 768-dimensional vectors by default but can be configured for other dimensions. If you switch between models with different output dimensions, the existing vector index becomes incompatible.

**Fix:** Reindex your entire vault after any embedding provider change:

```bash
dewey reindex
```

This drops all existing embeddings and regenerates them with the new model. The operation is safe — it does not modify your vault content, only the derived vector index.

## Further Reading

- [Pluggable LLM Providers](/blog/pluggable-llm-providers/) — the design rationale behind the Embedder and Synthesizer interfaces
- [Setting Up Dewey Knowledge Stores](/docs/tutorials/dewey-knowledge-stores/) — configure automated knowledge extraction from indexed sources
- [Dewey](/docs/projects/dewey/) — project overview, installation, and architecture
