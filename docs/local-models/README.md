# Local Models Overview

This guide covers running Strix with locally-hosted LLMs using Ollama or LM Studio.

> **Local LLMs Are Not Recommended**
>
> Local models are **currently unfeasible** for serious Strix usage. Even high-end hardware (2x RTX 4090s) cannot compete with cloud providers.
>
> **Limitations:**
> - Context windows too small (8K-32K vs 128K-1M for cloud)
> - Reduced reasoning capability
> - No/limited vision support for browser screenshots
> - Significantly slower inference
>
> **Use cloud providers.** Local models are only for air-gapped environments, strict privacy requirements, or experimentation.

## When Local Models Make Sense

Local models are appropriate **only** if:

1. **Air-gapped environment**: No internet access to cloud APIs
2. **Strict data privacy**: Code absolutely cannot leave your network
3. **Compliance requirements**: Regulations prohibit cloud AI usage
4. **Experimentation**: Learning how Strix works, not real assessments

For all other cases, use cloud providers.

## Setup Guides

If you must use local models:

- [Ollama Setup](ollama.md) - Recommended for Linux/macOS/Windows
- [LM Studio Setup](lmstudio.md) - Desktop application with GUI
- [Why Local Doesn't Work](hardware-requirements.md) - Understanding the limitations

## Cloud is the Right Choice

For security assessments, use cloud providers:

1. **[OpenRouter](../llm-providers/openrouter.md)** - Access multiple providers, free tier options
2. **[Anthropic Claude](../llm-providers/anthropic.md)** - Best reasoning for security analysis
3. **[Google Gemini](../llm-providers/google-gemini.md)** - Free tier, 1M token context

See the [LLM Provider Configuration](../llm-providers/index.md) guide to get started.
