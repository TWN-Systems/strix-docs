# Google AI Studio (Gemini)

[Google AI Studio](https://aistudio.google.com/) provides access to Gemini models, featuring the largest context windows available (up to 1M tokens) and competitive pricing with a generous free tier.

> **Looking for enterprise/GCP?** See [Vertex AI](vertex-ai.md) for Google Cloud deployments.

## Overview

**Why Google Gemini?**
- **Massive context window**: Up to 1M tokens - analyze entire codebases
- **Free tier**: Substantial free usage for experimentation
- **Vision support**: Excellent multimodal capabilities for browser screenshots
- **Fast inference**: Gemini Flash models offer quick responses
- **Competitive pricing**: Often cheaper than OpenAI/Anthropic

## Getting Your API Key

### Google AI Studio (Recommended for most users)

1. Visit [aistudio.google.com](https://aistudio.google.com/)
2. Sign in with your Google account
3. Click "Get API key" in the left sidebar
4. Click "Create API key"
5. Select or create a Google Cloud project
6. Copy your API key

## Configuration

```bash
export STRIX_LLM="gemini/gemini-2.5-pro"
export LLM_API_KEY="your-google-ai-studio-key"
```

## Recommended Models

| Model | Identifier | Context | Vision | Notes |
|-------|------------|---------|--------|-------|
| Gemini 2.5 Pro | `gemini/gemini-2.5-pro` | 1M | Yes | Best capability, large context |
| Gemini 2.5 Flash | `gemini/gemini-2.5-flash` | 1M | Yes | Fast, reasoning-capable |
| Gemini 2.0 Flash | `gemini/gemini-2.0-flash` | 1M | Yes | Good free tier option |
| Gemini 3.0 | `gemini/gemini-3.0` | 1M | Yes | Latest generation |

### Model Selection Guide

- **Large codebase analysis**: Use `gemini/gemini-2.5-pro` or `gemini/gemini-3.0` - 1M token context handles massive projects
- **Fast scanning**: Use `gemini/gemini-2.5-flash` - quick responses with reasoning
- **Budget/free tier**: Use `gemini/gemini-2.0-flash` - generous free quota

## Special Features

### Reasoning Effort

Strix automatically enables `reasoning_effort: high` for Gemini 2.5 Flash and Pro models to maximize analytical capability.

### Large Context Window

Gemini's 1M token context window means Strix can analyze larger codebases without truncation. This is particularly valuable for:
- Monorepos
- Complex applications with many files
- Full project security assessments

## Free Tier Details

Google AI Studio offers a free tier with:
- Rate limits: 15 RPM (requests per minute) for Gemini 2.0 Flash
- Daily quotas vary by model
- Sufficient for testing and light usage

For production use, consider upgrading to paid tier for higher limits.

## LiteLLM Proxy Configuration (Optional)

```yaml
# litellm_config.yaml
model_list:
  - model_name: strix-gemini
    litellm_params:
      model: gemini/gemini-2.5-pro
      api_key: your-google-ai-key
```

## Common Issues & Troubleshooting

### "Invalid API Key"
```
LLM request failed: Invalid API key
```
**Solutions**:
1. Verify your key at [aistudio.google.com](https://aistudio.google.com/)
2. Ensure the key is associated with an active Google Cloud project
3. Check if billing is enabled (required for some models)

### "Rate limit exceeded"
```
LLM request failed: Rate limit exceeded
```
**Solutions**:
1. Free tier has strict rate limits (15 RPM for Flash)
2. Reduce concurrent requests:
   ```bash
   export LLM_RATE_LIMIT_CONCURRENT=2
   export LLM_RATE_LIMIT_DELAY=10
   ```
3. Upgrade to paid tier for higher limits
4. Wait for quota reset (daily for free tier)

### "Model not found"
```
LLM request failed: Model not found
```
**Solutions**:
1. Use the `gemini/` prefix (e.g., `gemini/gemini-2.5-pro`)
2. Check model availability in your region
3. Verify the model name spelling

### "Content policy violation"
```
LLM request failed: Content policy violation
```
**Solution**: Gemini has content filters. Security testing prompts may occasionally trigger these. Consider:
1. Using a different model for that specific test
2. Adjusting your testing approach
3. Trying a different provider (Anthropic is more flexible for security research)

### Slow Initial Response
**Cause**: Gemini models may have cold start latency.

**Solution**: First request may be slower. Subsequent requests should be faster.
