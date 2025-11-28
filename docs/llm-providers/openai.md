# OpenAI

[OpenAI](https://openai.com/) provides GPT-5 and the o-series reasoning models. OpenAI models are reliable, well-documented, and widely supported.

## Overview

**Why OpenAI?**
- GPT-5 offers excellent general-purpose performance
- o3/o4-mini provide enhanced reasoning for complex analysis
- Reliable infrastructure with high uptime
- Extensive documentation and community support
- Vision capabilities for browser screenshot analysis

## Getting Your API Key

1. Visit [platform.openai.com](https://platform.openai.com/)
2. Sign in or create an account
3. Navigate to [API Keys](https://platform.openai.com/api-keys)
4. Click "Create new secret key"
5. Copy your key (starts with `sk-`)

**Note**: You'll need to add credits to your account. New accounts may have spending limits.

## Configuration

### Environment Variables

```bash
export STRIX_LLM="openai/gpt-5"
export LLM_API_KEY="sk-your-openai-api-key"
```

### Optional: Organization ID

If you're part of multiple OpenAI organizations:

```bash
export OPENAI_ORGANIZATION="org-your-org-id"
```

## Recommended Models

| Model | Identifier | Context | Vision | Notes |
|-------|------------|---------|--------|-------|
| GPT-5 | `openai/gpt-5` | 128K | Yes | Recommended default |
| o3 | `openai/o3` | 200K | Yes | Advanced reasoning |
| o4-mini | `openai/o4-mini` | 128K | Yes | Fast reasoning model |
| o3-mini | `openai/o3-mini` | 128K | No | Cost-effective reasoning |
| GPT-4o | `openai/gpt-4o` | 128K | Yes | Previous generation |

### Model Selection Guide

- **General security testing**: Use `openai/gpt-5` - best balance of speed and capability
- **Complex vulnerability analysis**: Use `openai/o3` - enhanced reasoning for intricate logic
- **Budget-conscious**: Use `openai/o4-mini` - good reasoning at lower cost

## Special Features

### Reasoning Effort

Strix automatically sets `reasoning_effort: high` for o-series models (o1, o3, o3-mini, o4-mini) and GPT-5 to maximize analytical capability during security assessments.

### Stop Sequences

Strix uses custom stop sequences for tool calling. Note that o1 models don't support stop sequences - Strix handles this automatically.

## LiteLLM Proxy Configuration (Optional)

```yaml
# litellm_config.yaml
model_list:
  - model_name: strix-gpt5
    litellm_params:
      model: openai/gpt-5
      api_key: sk-your-key

  - model_name: strix-o3
    litellm_params:
      model: openai/o3
      api_key: sk-your-key
```

## Common Issues & Troubleshooting

### "Invalid API Key"
```
LLM request failed: Invalid API key
```
**Solutions**:
1. Verify your key at [platform.openai.com/api-keys](https://platform.openai.com/api-keys)
2. Ensure the key starts with `sk-`
3. Check if the key has been revoked or expired
4. Verify you have credits in your account

### "Rate limit exceeded"
```
LLM request failed: Rate limit exceeded
```
**Solutions**:
1. Check your [rate limits](https://platform.openai.com/account/limits)
2. Reduce concurrent requests:
   ```bash
   export LLM_RATE_LIMIT_CONCURRENT=3
   export LLM_RATE_LIMIT_DELAY=5
   ```
3. Request a rate limit increase from OpenAI

### "Model not found"
```
LLM request failed: Model not found
```
**Solutions**:
1. Verify the model name (e.g., `openai/gpt-5` not `gpt-5`)
2. Check if you have access to the model (some require approval)
3. Review available models at [platform.openai.com/docs/models](https://platform.openai.com/docs/models)

### "Context window exceeded"
```
LLM request failed: Context too long
```
**Solutions**:
1. Target smaller codebases or specific directories
2. Use a model with larger context (GPT-5 has 128K tokens)
3. Strix will automatically compress conversation history, but initial input must fit

### "Budget exceeded"
```
LLM request failed: Budget exceeded
```
**Solution**: Add credits to your OpenAI account or increase your spending limit in account settings.

### Slow Response Times
**Possible causes**:
- o3 models can take longer due to reasoning computation
- High API demand periods

**Solutions**:
1. Use `openai/o4-mini` for faster reasoning
2. Increase timeout: `export LLM_TIMEOUT=900`
