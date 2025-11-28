# DeepSeek

> **⚠️ Not Currently Supported**
>
> DeepSeek direct API does not work with Strix due to LiteLLM compatibility issues ([#101](https://github.com/usestrix/strix/issues/101), [#143](https://github.com/usestrix/strix/issues/143)).
>
> **Use OpenRouter instead** - see below.

> **Note**: Strix is under active development. Not all providers have been fully tested. For help, join [#support in the Strix Discord](https://discord.gg/YjKFvEZSdZ).

## Use DeepSeek via OpenRouter

The **only working method** to use DeepSeek models with Strix is through [OpenRouter](openrouter.md):

```bash
export STRIX_LLM="openrouter/deepseek/deepseek-chat-v3-0324"
export LLM_API_KEY="sk-or-your-openrouter-key"
```

For reasoning tasks:
```bash
export STRIX_LLM="openrouter/deepseek/deepseek-r1"
export LLM_API_KEY="sk-or-your-openrouter-key"
```

Get your OpenRouter key at [openrouter.ai/keys](https://openrouter.ai/keys)

## Available Models (via OpenRouter)

| Model | OpenRouter Identifier | Notes |
|-------|----------------------|-------|
| DeepSeek Chat V3 | `openrouter/deepseek/deepseek-chat-v3-0324` | General purpose |
| DeepSeek R1 | `openrouter/deepseek/deepseek-r1` | Reasoning model |
| DeepSeek Coder | `openrouter/deepseek/deepseek-coder` | Code-focused |

## Why Direct API Doesn't Work

1. **Authentication**: LiteLLM requires `DEEPSEEK_API_KEY`, but Strix passes keys via `LLM_API_KEY`
2. **Response parsing**: DeepSeek's API returns JSON that causes deserialization errors in LiteLLM

These are upstream issues that cannot be fixed via configuration.

## Getting Help

- Discord: [#support in strix-ai](https://discord.gg/YjKFvEZSdZ)
- GitHub: [usestrix/strix/issues](https://github.com/usestrix/strix/issues)
