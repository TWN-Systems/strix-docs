# Environment Variables

This guide documents all environment variables used by Strix.

## Required Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `STRIX_LLM` | LLM model identifier | `openrouter/x-ai/grok-4.1-fast:free` |
| `LLM_API_KEY` | API key for your LLM provider | `sk-or-xxxxx` |

## Optional Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `LLM_API_BASE` | Provider default | Custom API endpoint (for local models) |
| `LLM_TIMEOUT` | `600` | Request timeout in seconds |
| `LLM_RATE_LIMIT_CONCURRENT` | `5` | Max concurrent requests |
| `LLM_RATE_LIMIT_DELAY` | `0` | Delay between requests (seconds) |
| `PERPLEXITY_API_KEY` | - | Enables web search capabilities |

## Example .env File

```bash
# Required
STRIX_LLM="openrouter/x-ai/grok-4.1-fast:free"
LLM_API_KEY="xxxxxx"

# Optional - for local models (Ollama, LMStudio, etc.)
# LLM_API_BASE="http://localhost:11434"

# Optional - enables web search
# PERPLEXITY_API_KEY=""

# Optional - request timeout in seconds (default: 600)
# LLM_TIMEOUT=600
```

## Loading Environment Variables

### Option 1: Export directly

```bash
export STRIX_LLM="openrouter/x-ai/grok-4.1-fast:free"
export LLM_API_KEY="your-key"
strix --target ./your-app
```

### Option 2: Use a .env file

Create a `.env` file in your project directory, then source it:

```bash
source .env
strix --target ./your-app
```

Or use a tool like [direnv](https://direnv.net/) to automatically load `.env` files.

## Provider-Specific Variables

Some providers require their own environment variables:

| Provider | Variable | Notes |
|----------|----------|-------|
| AWS Bedrock | `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_REGION_NAME` | Required for Bedrock |
| Google Vertex AI | `GOOGLE_APPLICATION_CREDENTIALS`, `VERTEXAI_PROJECT`, `VERTEXAI_LOCATION` | Required for Vertex AI |
| Ollama | `LLM_API_BASE`, `LLM_API_KEY` | API key can be any placeholder value |

See provider-specific guides in [LLM Providers](llm-providers/index.md) for details.
