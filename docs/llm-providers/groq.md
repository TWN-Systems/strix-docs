# Groq

[Groq](https://groq.com/) provides ultra-fast LLM inference through custom hardware (LPU). Known for extremely low latency responses.

## Overview

**Why Groq?**
- **Fastest inference**: Sub-second responses
- **Low latency**: Ideal for interactive testing
- **Free tier**: Generous free quota for testing
- **Growing model selection**: Llama, Mixtral, and more

**Limitations**:
- Limited model selection compared to other providers
- No vision/multimodal support for most models
- May not be suitable for complex security analysis

## Getting Your API Key

1. Visit [console.groq.com](https://console.groq.com/)
2. Create an account
3. Navigate to API Keys
4. Create a new key

## Configuration

### Environment Variables

```bash
export STRIX_LLM="groq/llama-3.3-70b-versatile"
export LLM_API_KEY="gsk_your-groq-key"
```

## Recommended Models

| Model | Identifier | Context | Notes |
|-------|------------|---------|-------|
| Llama 3.3 70B | `groq/llama-3.3-70b-versatile` | 128K | Best capability |
| Llama 3.1 70B | `groq/llama-3.1-70b-versatile` | 128K | Reliable option |
| Mixtral 8x7B | `groq/mixtral-8x7b-32768` | 32K | Fast, smaller context |

## LiteLLM Proxy Configuration (Optional)

```yaml
# litellm_config.yaml
model_list:
  - model_name: strix-groq
    litellm_params:
      model: groq/llama-3.3-70b-versatile
      api_key: gsk_your-groq-key
```

## Common Issues & Troubleshooting

### Rate Limits
Groq has per-minute token limits on free tier:
```bash
export LLM_RATE_LIMIT_CONCURRENT=1
export LLM_RATE_LIMIT_DELAY=15
```

### No Vision Support
Groq models don't support vision/images. Browser screenshot analysis won't work. Consider pairing with a vision-capable provider for comprehensive testing.

### Model Availability
Some models may be temporarily unavailable. Check [Groq status](https://status.groq.com/) if you encounter issues.

### Context Limits
Some Groq models have smaller context windows (32K). For large codebases, consider a provider with larger context.
