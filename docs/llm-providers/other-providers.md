# Other LiteLLM-Compatible Providers

Strix supports any provider compatible with [LiteLLM](https://docs.litellm.ai/docs/providers). This guide covers additional providers and custom configurations.

## Known Incompatible Providers

The following providers are **not compatible** with Strix:

| Provider | Reason |
|----------|--------|
| AgentRouter | Not compatible with Strix |
| DeepSeek (direct) | Authentication and JSON parsing issues - use [OpenRouter](openrouter.md) instead |
| Zhipu.ai / z.ai | Uses JWT token format, not OpenAI-compatible ([PR in progress](https://github.com/BerriAI/litellm/pull/16359)) |

## Supported Providers

LiteLLM supports 100+ providers. Here are some additional options:

| Provider | Prefix | Documentation |
|----------|--------|---------------|
| Cohere | `cohere/` | [Docs](https://docs.litellm.ai/docs/providers/cohere) |
| AI21 | `ai21/` | [Docs](https://docs.litellm.ai/docs/providers/ai21) |
| Together AI | `together_ai/` | [Docs](https://docs.litellm.ai/docs/providers/togetherai) |
| Anyscale | `anyscale/` | [Docs](https://docs.litellm.ai/docs/providers/anyscale) |
| Replicate | `replicate/` | [Docs](https://docs.litellm.ai/docs/providers/replicate) |
| Perplexity | `perplexity/` | [Docs](https://docs.litellm.ai/docs/providers/perplexity) |
| Fireworks | `fireworks_ai/` | [Docs](https://docs.litellm.ai/docs/providers/fireworks_ai) |
| Mistral | `mistral/` | [Docs](https://docs.litellm.ai/docs/providers/mistral) |
| Bedrock | `bedrock/` | [Docs](https://docs.litellm.ai/docs/providers/bedrock) |
| Sagemaker | `sagemaker/` | [Docs](https://docs.litellm.ai/docs/providers/aws_sagemaker) |

## General Configuration Pattern

Most providers follow this pattern:

```bash
export STRIX_LLM="provider-prefix/model-name"
export LLM_API_KEY="your-provider-api-key"
```

## Provider-Specific Examples

### Cohere

```bash
export STRIX_LLM="cohere/command-r-plus"
export LLM_API_KEY="your-cohere-key"
```

### Together AI

```bash
export STRIX_LLM="together_ai/meta-llama/Llama-3.3-70B-Instruct-Turbo"
export LLM_API_KEY="your-together-key"
```

### Mistral

```bash
export STRIX_LLM="mistral/mistral-large-latest"
export LLM_API_KEY="your-mistral-key"
```

### AWS Bedrock

```bash
export STRIX_LLM="bedrock/anthropic.claude-3-5-sonnet-20241022-v2:0"
export AWS_ACCESS_KEY_ID="your-access-key"
export AWS_SECRET_ACCESS_KEY="your-secret-key"
export AWS_REGION_NAME="us-east-1"
```

### Fireworks AI

```bash
export STRIX_LLM="fireworks_ai/accounts/fireworks/models/llama-v3p3-70b-instruct"
export LLM_API_KEY="your-fireworks-key"
```

## Custom OpenAI-Compatible Endpoints

For any OpenAI-compatible API:

```bash
export STRIX_LLM="openai/model-name"
export LLM_API_KEY="your-api-key"
export LLM_API_BASE="https://your-custom-endpoint.com/v1"
```

This works with:
- Self-hosted vLLM
- Text Generation Inference (TGI)
- LocalAI
- Any OpenAI-compatible server

## LiteLLM Proxy for Advanced Configurations

For complex setups with multiple providers, model routing, or fallbacks, consider running a [LiteLLM proxy](https://docs.litellm.ai/docs/proxy/quick_start):

```yaml
# litellm_config.yaml
model_list:
  - model_name: strix-primary
    litellm_params:
      model: anthropic/claude-sonnet-4-5
      api_key: your-anthropic-key

  - model_name: strix-primary
    litellm_params:
      model: openai/gpt-5
      api_key: your-openai-key
    # Fallback to OpenAI if Anthropic fails

  - model_name: strix-budget
    litellm_params:
      model: groq/llama-3.3-70b-versatile
      api_key: your-groq-key
```

Then configure Strix:

```bash
export STRIX_LLM="strix-primary"
export LLM_API_BASE="http://localhost:4000"
```

## Finding the Right Model Identifier

1. Check [LiteLLM's provider docs](https://docs.litellm.ai/docs/providers)
2. Look for the model prefix (e.g., `cohere/`, `mistral/`)
3. Find the exact model name from the provider's documentation

## Considerations for Strix

When choosing a provider, ensure the model supports:

| Feature | Required | Notes |
|---------|----------|-------|
| Chat completions | Yes | Must support chat/conversation format |
| Large context | Recommended | 64K+ tokens for codebase analysis |
| Vision/multimodal | Recommended | For browser screenshot analysis |
| Function calling | Optional | Strix uses XML-based tool calling |

## Troubleshooting

### "Model not found"
1. Verify the provider prefix is correct
2. Check exact model name with the provider
3. Ensure your API key has access to that model

### "Unsupported parameters"
Some providers don't support all parameters. Strix uses `litellm.drop_params = True` to handle this, but some edge cases may occur.

### Rate Limits
Each provider has different limits. Adjust as needed:
```bash
export LLM_RATE_LIMIT_CONCURRENT=2
export LLM_RATE_LIMIT_DELAY=10
```
