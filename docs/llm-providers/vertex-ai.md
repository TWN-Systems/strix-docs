# Google Vertex AI

> **⚠️ Experimental - Not Fully Tested**
>
> Vertex AI integration has not been fully tested with Strix. You may encounter compatibility issues.
>
> Strix is under active development and not all providers are fully supported yet.
>
> For help, join [#support in the Strix Discord](https://discord.gg/YjKFvEZSdZ).

[Vertex AI](https://cloud.google.com/vertex-ai) is Google Cloud's enterprise AI platform, providing access to Gemini models with additional compliance, security, and enterprise features.

## Overview

**When to use Vertex AI instead of Google AI Studio:**
- Enterprise compliance requirements
- Need for VPC controls and private endpoints
- Google Cloud organization policies
- Audit logging and access controls
- Existing GCP infrastructure

**For most users**: [Google AI Studio](google-gemini.md) is simpler and recommended.

## Prerequisites

1. Google Cloud Platform account
2. A GCP project with billing enabled
3. Vertex AI API enabled
4. Service account with appropriate permissions

## Getting Started

### 1. Enable Vertex AI API

```bash
gcloud services enable aiplatform.googleapis.com
```

### 2. Create Service Account

```bash
# Create service account
gcloud iam service-accounts create strix-vertex \
    --display-name="Strix Vertex AI"

# Grant Vertex AI User role
gcloud projects add-iam-policy-binding YOUR_PROJECT_ID \
    --member="serviceAccount:strix-vertex@YOUR_PROJECT_ID.iam.gserviceaccount.com" \
    --role="roles/aiplatform.user"

# Create and download key
gcloud iam service-accounts keys create vertex-key.json \
    --iam-account=strix-vertex@YOUR_PROJECT_ID.iam.gserviceaccount.com
```

### 3. Configuration

```bash
export STRIX_LLM="vertex_ai/gemini-2.5-pro"
export GOOGLE_APPLICATION_CREDENTIALS="/path/to/vertex-key.json"
export VERTEXAI_PROJECT="your-gcp-project-id"
export VERTEXAI_LOCATION="us-central1"
```

## Recommended Models

| Model | Identifier | Context | Vision | Notes |
|-------|------------|---------|--------|-------|
| Gemini 2.5 Pro | `vertex_ai/gemini-2.5-pro` | 1M | Yes | Best capability |
| Gemini 2.5 Flash | `vertex_ai/gemini-2.5-flash` | 1M | Yes | Fast, cost-effective |
| Gemini 2.0 Flash | `vertex_ai/gemini-2.0-flash` | 1M | Yes | Previous generation |

## Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `STRIX_LLM` | Yes | Model identifier (e.g., `vertex_ai/gemini-2.5-pro`) |
| `GOOGLE_APPLICATION_CREDENTIALS` | Yes | Path to service account JSON key |
| `VERTEXAI_PROJECT` | Yes | GCP project ID |
| `VERTEXAI_LOCATION` | Yes | Region (e.g., `us-central1`) |

## LiteLLM Proxy Configuration (Optional)

```yaml
# litellm_config.yaml
model_list:
  - model_name: strix-vertex
    litellm_params:
      model: vertex_ai/gemini-2.5-pro
      vertex_project: your-gcp-project
      vertex_location: us-central1
```

## Common Issues & Troubleshooting

### "Permission denied"

```
LLM request failed: Permission denied
```

**Solutions**:
1. Verify Vertex AI API is enabled in your project
2. Check service account has `Vertex AI User` role
3. Verify `GOOGLE_APPLICATION_CREDENTIALS` path is correct
4. Confirm project ID and location are set

### "Model not found"

```
LLM request failed: Model not found
```

**Solutions**:
1. Use `vertex_ai/` prefix (not `gemini/`)
2. Check model availability in your region
3. Verify model name spelling

### "Invalid credentials"

**Solutions**:
1. Regenerate service account key
2. Ensure key file hasn't expired
3. Check file permissions on key JSON

## Getting Help

- Discord: [#support in strix-ai](https://discord.gg/YjKFvEZSdZ)
- GitHub: [usestrix/strix/issues](https://github.com/usestrix/strix/issues)
