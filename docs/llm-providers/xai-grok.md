# xAI Grok

[xAI](https://x.ai/) provides Grok models, known for strong reasoning and fast inference capabilities.

## Overview

**Why xAI Grok?**
- **Strong reasoning**: Grok-4 offers excellent analytical capability
- **Fast inference**: Quick response times
- **Large context**: 128K token context window
- **Vision support**: Multimodal capabilities for screenshot analysis

## Getting Your API Key

1. Visit [console.x.ai](https://console.x.ai/)
2. Create an account or sign in
3. Navigate to API Keys
4. Generate a new API key

## Configuration

### Environment Variables

```bash
export STRIX_LLM="xai/grok-4.1-fast"
export LLM_API_KEY="your-xai-api-key"
```

## Recommended Models

| Model | Identifier | Context | Vision | Notes |
|-------|------------|---------|--------|-------|
| Grok 4.1 Fast | `xai/grok-4.1-fast` | 128K | Yes | Recommended for speed |
| Grok 4 | `xai/grok-4-0709` | 128K | Yes | Maximum capability |
| Grok Code Fast | `xai/grok-code-fast-1` | 128K | No | Code-focused |

### Model Selection Guide

- **General security testing**: `xai/grok-4.1-fast` - good balance
- **Complex analysis**: `xai/grok-4-0709` - maximum reasoning
- **Code review**: `xai/grok-code-fast-1` - optimized for code

## Special Features

### Stop Word Handling

Strix automatically handles stop word compatibility for Grok models. Some Grok models don't support custom stop sequences - this is managed internally.

## LiteLLM Proxy Configuration (Optional)

```yaml
# litellm_config.yaml
model_list:
  - model_name: strix-grok
    litellm_params:
      model: xai/grok-4.1-fast
      api_key: your-xai-key
```

## Common Issues & Troubleshooting

### "Invalid API Key"
**Solutions**:
1. Verify your key at [console.x.ai](https://console.x.ai/)
2. Ensure billing is set up
3. Check if the key is active

### Rate Limits
xAI may have rate limits. If encountering issues:
```bash
export LLM_RATE_LIMIT_CONCURRENT=3
export LLM_RATE_LIMIT_DELAY=5
```

### Model Availability
Check [xAI status](https://status.x.ai/) if models are unavailable.
