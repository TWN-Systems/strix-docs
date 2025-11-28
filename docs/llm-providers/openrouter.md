# OpenRouter

[OpenRouter](https://openrouter.ai/) is a unified API that provides access to multiple LLM providers through a single interface. It's an excellent choice for Strix users who want flexibility to switch between models without managing multiple API keys.

## Overview

**Why OpenRouter?**
- Access to 100+ models from OpenAI, Anthropic, Google, Meta, and more
- Single API key for all providers
- Often cheaper than direct provider access
- Automatic fallback between providers
- Usage-based billing with no minimums

## Getting Your API Key

1. Visit [openrouter.ai](https://openrouter.ai/)
2. Sign in with Google, GitHub, or email
3. Navigate to [API Keys](https://openrouter.ai/keys)
4. Click "Create Key"
5. Copy your key (starts with `sk-or-`)

## Configuration

### Environment Variables

```bash
export STRIX_LLM="openrouter/anthropic/claude-3.5-sonnet"
export LLM_API_KEY="sk-or-your-openrouter-key"
```

### Model Identifier Format

OpenRouter uses the format: `openrouter/<model-name>`

Add `:free` suffix for free tier models where available.

**Examples:**
```bash
# Paid models
export STRIX_LLM="openrouter/anthropic/claude-3.5-sonnet"
export STRIX_LLM="openrouter/openai/gpt-4o"
export STRIX_LLM="openrouter/google/gemini-2.0-flash-exp"

# Free tier models (add :free suffix)
export STRIX_LLM="openrouter/google/gemini-2.0-flash-exp:free"
export STRIX_LLM="openrouter/meta-llama/llama-3.3-70b-instruct:free"
export STRIX_LLM="openrouter/qwen/qwen-2.5-72b-instruct:free"
```

> **Tip**: Browse available models and their pricing at [openrouter.ai/models](https://openrouter.ai/models). Models with a free tier show `:free` in their identifier.

## Recommended Models

| Model | Identifier | Context | Vision | Notes |
|-------|------------|---------|--------|-------|
| Claude 3.5 Sonnet | `openrouter/anthropic/claude-3.5-sonnet` | 200K | Yes | Best for security analysis |
| GPT-4o | `openrouter/openai/gpt-4o` | 128K | Yes | Reliable, well-rounded |
| Gemini 2.0 Flash | `openrouter/google/gemini-2.0-flash-exp:free` | 1M | Yes | Free tier available |
| Llama 3.3 70B | `openrouter/meta-llama/llama-3.3-70b-instruct:free` | 128K | No | Free tier available |
| Qwen 2.5 72B | `openrouter/qwen/qwen-2.5-72b-instruct:free` | 128K | No | Free, strong coding |

## LiteLLM Proxy Configuration (Optional)

For advanced users running a LiteLLM proxy:

```yaml
# litellm_config.yaml
model_list:
  - model_name: strix-model
    litellm_params:
      model: openrouter/anthropic/claude-3.5-sonnet
      api_key: sk-or-your-key
```

Then configure Strix to use your proxy:

```bash
export STRIX_LLM="strix-model"
export LLM_API_BASE="http://localhost:4000"
```

## Cost Optimization Tips

1. **Use model routing**: OpenRouter can automatically select the cheapest model that meets your requirements
2. **Set spend limits**: Configure daily/monthly limits in the OpenRouter dashboard
3. **Monitor usage**: Check the usage page to track costs per model
4. **Consider off-peak**: Some models have lower rates during off-peak hours

## Common Issues & Troubleshooting

### "Invalid API Key"
```
LLM request failed: Invalid API key
```
**Solution**: Verify your key at [openrouter.ai/keys](https://openrouter.ai/keys). Ensure it starts with `sk-or-`.

### "Model not found"
```
LLM request failed: Model not found
```
**Solution**: Check the model identifier format. Use `openrouter/<model-name>` format (e.g., `openrouter/anthropic/claude-3.5-sonnet`). Add `:free` suffix for free tier models. Browse available models at [openrouter.ai/models](https://openrouter.ai/models).

### "Rate limit exceeded"
```
LLM request failed: Rate limit exceeded
```
**Solution**: OpenRouter rate limits vary by model. Reduce concurrent requests:
```bash
export LLM_RATE_LIMIT_CONCURRENT=2
export LLM_RATE_LIMIT_DELAY=10
```

### "Insufficient credits"
**Solution**: Add credits to your OpenRouter account. OpenRouter is pay-as-you-go.

### Slow Responses
**Solution**: Some models are queued during high demand. Consider using models with faster availability or enabling fallback in OpenRouter settings.
