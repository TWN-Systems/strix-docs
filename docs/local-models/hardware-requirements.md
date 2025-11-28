# Local Models: A Reality Check

> **Bottom Line**: Local LLMs are currently unfeasible for serious Strix usage. Even high-end consumer hardware (2x RTX 4090s) cannot compete with cloud providers. Use cloud.

## The Current State of Local LLMs

Local language models have significant limitations that make them impractical for Strix:

- **Context windows are too small**: Local models typically offer 8K-32K tokens vs. 128K-1M for cloud providers. Strix needs large context to analyze codebases effectively.

- **Reasoning capability is reduced**: Cloud models are trained and optimized at scales that local hardware cannot replicate.

- **Vision/multimodal is limited**: Most local models lack the vision capabilities Strix needs for browser screenshot analysis.

- **Inference is slow**: Even with expensive hardware, response times are significantly slower than cloud APIs.

## Why Hardware Specs Don't Matter

We don't provide hardware recommendations because:

1. **consumer hardware insufficient** - Even dual RTX 4090s or an RTX 5090 won't match cloud performance
2. **Cost-benefit doesn't work out** - The hardware investment rarely justifies the reduced capability
3. **Small model research is ongoing** - We need breakthroughs in efficient small models before local becomes viable due to limit context window.

## When to Use Local Models Anyway

Local models make sense **only** in these specific scenarios:

| Scenario | Rationale |
|----------|-----------|
| **Air-gapped environments** | No network access to cloud APIs |
| **Strict data privacy** | Code/data absolutely cannot leave your network |
| **Experimentation** | Learning how Strix works, not real assessments |
| **Compliance requirements** | Regulations prohibit cloud AI usage |

For all other use cases, cloud providers are the right choice.

## Recommended Path Forward

1. **Use cloud providers** - See [LLM Provider Configuration](../llm-providers/index.md)
2. **Start with OpenRouter** - Access multiple providers with one API key
3. **Consider free tiers** - Google Gemini and OpenRouter offer free options
4. **Monitor small model research** - The landscape may change as efficient models improve

## Setting Up Local Models (If Required)

If you must use local models despite the limitations:

- [Ollama Setup](ollama.md) - Easiest local deployment
- [LM Studio Setup](lmstudio.md) - Desktop application

Expect reduced capability and slower performance.
