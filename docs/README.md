# Strix Documentation

Welcome to the Strix documentation. Strix is an open-source, AI-powered autonomous penetration testing framework that uses AI agents to discover, validate, and report security vulnerabilities.

## Quick Links

- [Quickstart Guide](quickstart.md) - Get up and running in minutes
- [LLM Provider Configuration](llm-providers/index.md) - Configure your AI provider
- [Local Models Setup](local-models/index.md) - Run Strix with local LLMs
- [Troubleshooting](troubleshooting.md) - Common issues and solutions

## Prerequisites

Before using Strix, ensure you have:

- **Docker** - Running and accessible
- **Python 3.12+** - Required for installation
- **LLM Provider API Key** - From OpenAI, Anthropic, Google, or another supported provider

## Installation

```bash
pipx install strix-agent
```

## Minimal Configuration

```bash
export STRIX_LLM="openai/gpt-5"
export LLM_API_KEY="your-api-key"
```

## First Scan

```bash
strix --target ./your-app-directory
```

For detailed provider-specific setup, see the [LLM Provider Configuration](llm-providers/index.md) guide.

## Getting Help

- [GitHub Issues](https://github.com/usestrix/strix/issues) - Report bugs and request features
- [Discord Community](https://discord.gg/YjKFvEZSdZ) - Get help from the community
- [Contributing Guide](https://github.com/usestrix/strix/blob/main/CONTRIBUTING.md) - Help improve Strix
