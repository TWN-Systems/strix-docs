# Quickstart Guide

Get Strix running in under 5 minutes with this quickstart guide.

## Prerequisites

1. **Docker** must be installed and running
2. **Your user must be in the docker group** (Linux):
   ```bash
   sudo usermod -aG docker $USER
   ```
   Then **restart your PC**. If you must force access without restarting, run `newgrp docker` in your terminal session.
3. **Python 3.12+** is required
4. An **LLM API key** from a supported provider

## Step 1: Install Strix

```bash
pipx install strix-agent
```

Or with pip:

```bash
pip install strix-agent
```

## Step 2: Configure Your LLM Provider

See [Environment Variables](configure-environment-variables.md) for the full reference.

### Example .env File

```bash
# Required
STRIX_LLM="openrouter/x-ai/grok-4.1-fast:free"
LLM_API_KEY="xxxxxx"

# Optional - for local models (Ollama, LMStudio, etc.)
# LLM_API_BASE="http://localhost:11434"

# Optional - enables web search
# PERPLEXITY_API_KEY=""

# Optional - request timeout in seconds (default: 600)
# LLM_TIMEOUT=600
```

Or choose one of the recommended providers:

#### Option A: OpenRouter (Recommended)

Access multiple models with one API key. Free tier available.

```bash
export STRIX_LLM="openrouter/anthropic/claude-3.5-sonnet"
export LLM_API_KEY="sk-or-your-openrouter-key"
```

For free tier models, add `:free` suffix:
```bash
export STRIX_LLM="openrouter/google/gemini-2.0-flash-exp:free"
```

Get your key at [openrouter.ai](https://openrouter.ai/)

#### Option B: OpenAI

```bash
export STRIX_LLM="openai/gpt-5"
export LLM_API_KEY="sk-your-openai-api-key"
```

#### Option C: Anthropic Claude

```bash
export STRIX_LLM="anthropic/claude-sonnet-4-5"
export LLM_API_KEY="sk-ant-your-anthropic-key"
```

#### Option D: Google Gemini

```bash
export STRIX_LLM="gemini/gemini-2.5-pro"
export LLM_API_KEY="your-google-ai-studio-key"
```

For other providers, see the [LLM Provider Configuration](llm-providers/index.md) guide.

## Step 3: Run Your First Scan

```bash
# Scan a local codebase
strix --target ./your-app

# Scan a GitHub repository
strix --target https://github.com/org/repo

# Scan a live web application
strix --target https://your-app.com
```

The first run will automatically pull the Strix sandbox Docker image.

## Step 4: View Results

Results are saved to `strix_runs/<run-name>/` including:

- Vulnerability findings with proof-of-concept details
- Remediation recommendations
- Full scan logs

## Optional: Enable Web Search

For enhanced reconnaissance capabilities:

```bash
export PERPLEXITY_API_KEY="your-perplexity-key"
```

## Next Steps

- [Configure additional LLM providers](llm-providers/index.md)
- [Set up local models](local-models/index.md) (advanced)
- [CI/CD Integration](https://github.com/usestrix/strix#-cicd-github-actions)
- [Troubleshooting](troubleshooting.md)

## Learn More

- [Prompt Modules](modules.md) - Specialized vulnerability testing modules
- [Agent Tools](tools.md) - Browser, proxy, terminal, and other capabilities
- [Vulnerability Types](vulnerabilities.md) - What Strix scans for
- [Supported Technologies](technologies.md) - Frameworks and backends

## Common First-Run Issues

| Issue | Solution |
|-------|----------|
| Docker not running | Start Docker Desktop or `systemctl start docker` |
| Invalid API key | Verify your key and ensure it has correct permissions |
| Model not found | Check the model name format: `provider/model-name` |
| Timeout errors | Increase timeout: `export LLM_TIMEOUT=900` |
