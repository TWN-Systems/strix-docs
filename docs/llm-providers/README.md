# LLM Provider Configuration

Strix uses [LiteLLM](https://docs.litellm.ai/) as its LLM abstraction layer, enabling support for 100+ providers through a unified interface. This guide covers configuration for the most popular providers.

## How It Works

Strix requires two environment variables for most cloud providers:

```bash
export STRIX_LLM="provider/model-name"    # Model identifier
export LLM_API_KEY="your-api-key"         # Provider API key
```

The model identifier follows LiteLLM's format: `provider/model-name`.

## Environment Variables Reference

| Variable | Required | Description | Default |
|----------|----------|-------------|---------|
| `STRIX_LLM` | Yes | Model identifier (e.g., `openai/gpt-5`) | `openai/gpt-5` |
| `LLM_API_KEY` | Yes* | API key for the provider | - |
| `LLM_API_BASE` | No | Custom API endpoint URL | - |
| `LLM_TIMEOUT` | No | Request timeout in seconds | `600` |
| `LLM_RATE_LIMIT_DELAY` | No | Delay between requests (seconds) | `5.0` |
| `LLM_RATE_LIMIT_CONCURRENT` | No | Max concurrent requests | `6` |
| `PERPLEXITY_API_KEY` | No | Enables web search capabilities | - |

*For Ollama, use a placeholder value like `LLM_API_KEY="1234"`

## Recommended Models

Strix works best with models that have:
- **Large context windows** (128K+ tokens) for analyzing codebases
- **Strong reasoning capabilities** for security analysis
- **Vision/multimodal support** for Playwright browser screenshots

### Model Tiers

#### Maximum Capability
Best performance for complex security assessments.

| Model | Provider | Context | Vision | Notes |
|-------|----------|---------|--------|-------|
| `anthropic/claude-sonnet-4-5` | Anthropic | 200K | Yes | Excellent reasoning, prompt caching |
| `anthropic/claude-opus-4-5` | Anthropic | 200K | Yes | Top-tier capability |
| `openai/gpt-5` | OpenAI | 128K | Yes | Latest GPT model |
| `gemini/gemini-2.5-pro` | Google | 1M | Yes | Largest context window |

#### Balanced Performance
Good balance of capability and cost.

| Model | Provider | Context | Vision | Notes |
|-------|----------|---------|--------|-------|
| `xai/grok-4.1-fast` | xAI | 128K | Yes | Fast inference |
| `anthropic/claude-sonnet-4` | Anthropic | 200K | Yes | Reliable performer |
| `gemini/gemini-2.5-flash` | Google | 1M | Yes | Fast, large context |

#### Budget-Friendly
Lower cost options via aggregators. Use `:free` suffix for free tier models on OpenRouter.

| Model | Provider | Context | Vision | Notes |
|-------|----------|---------|--------|-------|
| `openrouter/google/gemini-2.0-flash-exp:free` | OpenRouter | 1M | Yes | Free tier |
| `openrouter/meta-llama/llama-3.3-70b-instruct:free` | OpenRouter | 128K | No | Free tier |
| `openrouter/qwen/qwen-2.5-72b-instruct:free` | OpenRouter | 128K | No | Free, strong coding |
| `gemini/gemini-2.0-flash` | Google | 1M | Yes | Free tier available |

#### Privacy-Focused (Local)

> **Warning**: Local models have severe limitations for Strix:
> - Limited context windows (8K-32K vs 128K-200K for cloud)
> - Reduced reasoning capability
> - Slower inference without dedicated GPU
> - Most lack vision/multimodal support
>
> Cloud providers are strongly recommended. See [Local Models](../local-models/index.md) for details.

## Provider Comparison

| Provider | Best For | Pricing | Vision | Prompt Caching |
|----------|----------|---------|--------|----------------|
| [OpenRouter](openrouter.md) | Multi-model access | Varies | Yes | Depends on model |
| [OpenAI](openai.md) | Reliability, GPT-5 | $$$ | Yes | No |
| [Google Gemini](google-gemini.md) | Large context, free tier | $-$$ | Yes | No |
| [Anthropic](anthropic.md) | Reasoning, security analysis | $$$ | Yes | Yes (auto) |
| [Azure OpenAI](azure-openai.md) | Enterprise, compliance | $$$ | Yes | No |
| [DeepSeek](deepseek.md) | Cost-effective reasoning | $ | Limited | No |
| [Groq](groq.md) | Fast inference | $$ | No | No |
| [xAI](xai-grok.md) | Grok models | $$ | Yes | No |
| [z.ai](zhipu-glm.md) | GLM-4.6 | $ | Yes | No |

## Provider Guides

### Primary Providers (Recommended)
- [OpenRouter](openrouter.md) - Access multiple providers through one API
- [OpenAI](openai.md) - GPT-5, o3, o4-mini
- [Google Gemini](google-gemini.md) - Gemini 2.5 Pro/Flash
- [Anthropic](anthropic.md) - Claude Sonnet/Opus 4.5

### Additional Providers
- [Azure OpenAI](azure-openai.md) - Enterprise deployment
- [DeepSeek](deepseek.md) - DeepSeek-R1 reasoning
- [Groq](groq.md) - Ultra-fast inference
- [xAI Grok](xai-grok.md) - Grok-4 models
- [z.ai GLM](zhipu-glm.md) - GLM-4.6
- [Other Providers](other-providers.md) - LiteLLM-compatible providers

### Local Models
- [Local Models Overview](../local-models/index.md) - Self-hosted options
- [Ollama](../local-models/ollama.md) - Easy local deployment
- [LM Studio](../local-models/lmstudio.md) - Desktop application
