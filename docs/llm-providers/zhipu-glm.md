# z.ai / Zhipu GLM

> **⚠️ Not Compatible with Strix**
>
> Zhipu.ai uses JWT token authentication format, which is not compatible with Strix/LiteLLM's OpenAI-style API key approach.
>
> For updates, join [#support in the Strix Discord](https://discord.gg/YjKFvEZSdZ).

## Why It Doesn't Work

Zhipu.ai requires JWT (JSON Web Token) authentication rather than standard API key authentication. Strix passes API keys via LiteLLM's OpenAI-compatible format, which Zhipu.ai does not accept.

**Status**: There is a PR in progress to add Zhipu.ai support to LiteLLM: [BerriAI/litellm#16359](https://github.com/BerriAI/litellm/pull/16359)

## Alternatives

Consider these working providers instead:

- [OpenRouter](openrouter.md) - Access multiple models with one API key
- [Google AI Studio](google-gemini.md) - Gemini models with free tier
- [Groq](groq.md) - Fast inference
- [OpenAI](openai.md) - GPT models
- [Anthropic](anthropic.md) - Claude models

## Getting Help

- Discord: [#support in strix-ai](https://discord.gg/YjKFvEZSdZ)
- GitHub: [usestrix/strix/issues](https://github.com/usestrix/strix/issues)
