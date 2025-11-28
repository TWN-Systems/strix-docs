# Anthropic Claude

[Anthropic](https://www.anthropic.com/) provides Claude models, known for strong reasoning capabilities, excellent instruction following, and superior performance on complex analytical tasks. Claude is particularly well-suited for security analysis.

## Overview

**Why Anthropic Claude?**
- **Excellent reasoning**: Top-tier performance on complex security analysis
- **Large context window**: 200K tokens for comprehensive codebase review
- **Prompt caching**: Strix automatically uses Anthropic's caching for cost savings
- **Strong instruction following**: Precise, thorough vulnerability analysis
- **Vision support**: Excellent screenshot analysis for browser-based testing

## Getting Your API Key

1. Visit [console.anthropic.com](https://console.anthropic.com/)
2. Sign in or create an account
3. Navigate to [API Keys](https://console.anthropic.com/settings/keys)
4. Click "Create Key"
5. Copy your key (starts with `sk-ant-`)

**Note**: New accounts may require adding payment information before API access is enabled.

## Configuration

### Environment Variables

```bash
export STRIX_LLM="anthropic/claude-sonnet-4-5"
export LLM_API_KEY="sk-ant-your-anthropic-key"
```

## Recommended Models

| Model | Identifier | Context | Vision | Notes |
|-------|------------|---------|--------|-------|
| Claude Sonnet 4.5 | `anthropic/claude-sonnet-4-5` | 200K | Yes | Best balance of speed/capability |
| Claude Opus 4.5 | `anthropic/claude-opus-4-5` | 200K | Yes | Maximum capability |
| Claude Haiku 4.5 | `anthropic/claude-haiku-4-5` | 200K | Yes | Fastest, most cost-effective |
| Claude Sonnet 4 | `anthropic/claude-sonnet-4` | 200K | Yes | Previous generation |

### Model Selection Guide

- **Recommended default**: `anthropic/claude-sonnet-4-5` - excellent reasoning at reasonable cost
- **Maximum capability**: `anthropic/claude-opus-4-5` - for the most complex security assessments
- **Fast scanning**: `anthropic/claude-haiku-4-5` - quick initial reconnaissance

## Special Features

### Automatic Prompt Caching

Strix automatically enables [prompt caching](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching) for Anthropic models. This provides:

- **Cost savings**: Up to 90% reduction on cached tokens
- **Faster responses**: Cached prompts are processed more quickly
- **Automatic management**: Strix handles cache headers automatically

You'll see cache statistics in the usage output:
```
Cache hit: 50000 cached tokens, 5000 new tokens
```

### Reasoning Effort

Strix automatically sets `reasoning_effort: high` for Claude Sonnet 4.5 and Haiku 4.5 models, enabling extended thinking for complex security analysis.

### Extended Thinking

For complex vulnerability chains, Claude models can engage in extended reasoning. This happens automatically when the model needs more computation time.

## LiteLLM Proxy Configuration (Optional)

```yaml
# litellm_config.yaml
model_list:
  - model_name: strix-claude
    litellm_params:
      model: anthropic/claude-sonnet-4-5
      api_key: sk-ant-your-key

  - model_name: strix-claude-opus
    litellm_params:
      model: anthropic/claude-opus-4-5
      api_key: sk-ant-your-key
```

## Common Issues & Troubleshooting

### "Invalid API Key"
```
LLM request failed: Invalid API key
```
**Solutions**:
1. Verify your key at [console.anthropic.com/settings/keys](https://console.anthropic.com/settings/keys)
2. Ensure the key starts with `sk-ant-`
3. Check if the key has been revoked
4. Verify payment information is added to your account

### "Rate limit exceeded"
```
LLM request failed: Rate limit exceeded
```
**Solutions**:
1. Anthropic has tiered rate limits based on usage
2. Reduce concurrent requests:
   ```bash
   export LLM_RATE_LIMIT_CONCURRENT=3
   export LLM_RATE_LIMIT_DELAY=5
   ```
3. Check your [usage tier](https://console.anthropic.com/settings/limits)
4. Request a limit increase for production use

### "Model not found"
```
LLM request failed: Model not found
```
**Solutions**:
1. Use the correct prefix: `anthropic/claude-sonnet-4-5`
2. Check for typos in the model name
3. Verify the model is available (some may be in limited access)

### "Context window exceeded"
```
LLM request failed: Context too long
```
**Solutions**:
1. Claude has 200K context, but very large codebases may exceed this
2. Target specific directories rather than entire projects
3. Strix automatically compresses conversation history

### "Budget exceeded"
```
LLM request failed: Budget exceeded
```
**Solution**:
1. Add credits to your Anthropic account
2. Check spending limits in account settings
3. Consider using Claude Haiku for initial scans to reduce costs

### Prompt Caching Not Working

**Symptoms**: High token costs, no cache hit messages

**Solutions**:
1. Prompt caching requires minimum content length
2. Ensure `enable_prompt_caching` is not disabled in your config
3. Check that you're using a caching-supported model

### "Overloaded" Errors
```
LLM request failed: Service unavailable
```
**Solution**: Anthropic may be experiencing high demand. Strix will automatically retry with backoff. If persistent:
1. Wait a few minutes
2. Check [Anthropic status](https://status.anthropic.com/)
3. Try a different model temporarily

## Cost Optimization

1. **Use prompt caching**: Strix enables this automatically
2. **Choose appropriate models**: Use Haiku for simple tasks, Sonnet for standard analysis, Opus only for complex assessments
3. **Target specific code**: Focus scans on relevant directories rather than entire repositories
4. **Monitor usage**: Track costs in the [Anthropic console](https://console.anthropic.com/settings/cost)
