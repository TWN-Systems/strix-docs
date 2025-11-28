# Troubleshooting Guide

This guide covers common issues when running Strix and how to resolve them.

## Quick Diagnostics

Before troubleshooting, verify your setup:

```bash
# Check environment variables
echo $STRIX_LLM
echo $LLM_API_KEY
echo $LLM_API_BASE

# Check Docker is running
docker info

# Check Strix version
strix --version
```

## Authentication Errors

### "Invalid API Key"

```
LLM request failed: Invalid API key
```

**Causes**:
- API key is incorrect or expired
- Key doesn't have required permissions
- Key is for wrong environment (test vs production)

**Solutions**:

1. Verify your API key format:
   - OpenAI: Starts with `sk-`
   - Anthropic: Starts with `sk-ant-`
   - OpenRouter: Starts with `sk-or-`

2. Check the key is active in your provider's dashboard

3. Ensure the environment variable is set correctly:
   ```bash
   # Wrong - quotes included in value
   export LLM_API_KEY="'sk-your-key'"

   # Correct
   export LLM_API_KEY="sk-your-key"
   ```

4. For local models (Ollama/LM Studio), no API key is needed:
   ```bash
   unset LLM_API_KEY
   ```

### "Authentication Error" with Correct Key

**Causes**:
- Billing not set up
- Account suspended
- Key lacks model access

**Solutions**:
1. Add payment method to your provider account
2. Check for account suspension emails
3. Verify the key has access to the specified model

## Rate Limiting

### "Rate limit exceeded"

```
LLM request failed: Rate limit exceeded
```

**Causes**:
- Too many requests per minute
- Token usage limit exceeded
- Concurrent request limit hit

**Solutions**:

1. Reduce concurrent requests:
   ```bash
   export LLM_RATE_LIMIT_CONCURRENT=2
   ```

2. Increase delay between requests:
   ```bash
   export LLM_RATE_LIMIT_DELAY=10
   ```

3. Wait for rate limit reset (usually 1 minute)

4. Upgrade your API tier for higher limits

5. Consider using a different provider temporarily

## Context Window Errors

### "Context window exceeded" / "Context too long"

```
LLM request failed: Context too long
```

**Causes**:
- Codebase too large for model's context
- Conversation history grew too long
- Initial prompt exceeds limits

**Solutions**:

1. Target smaller codebases or specific directories:
   ```bash
   strix --target ./src/api  # Instead of entire project
   ```

2. Use a model with larger context:
   - Gemini: 1M tokens
   - Claude: 200K tokens
   - GPT-5: 128K tokens

3. Strix automatically compresses conversation history, but initial input must fit

4. For local models, increase context length (if VRAM allows):
   ```bash
   # Ollama example
   OLLAMA_NUM_CTX=32768 ollama serve
   ```

## Connection Errors

### "Connection error" / "Connection refused"

```
LLM request failed: Connection error
```

**Causes**:
- Local model server not running
- Incorrect API base URL
- Network/firewall issues

**Solutions**:

1. For local models, verify server is running:
   ```bash
   # Ollama
   ollama serve

   # LM Studio
   # Check Local Server tab is active
   ```

2. Verify `LLM_API_BASE` is correct:
   ```bash
   # Ollama
   export LLM_API_BASE="http://localhost:11434"

   # LM Studio
   export LLM_API_BASE="http://localhost:1234/v1"
   ```

3. Test the connection:
   ```bash
   curl http://localhost:11434/api/tags  # Ollama
   curl http://localhost:1234/v1/models   # LM Studio
   ```

4. Check firewall isn't blocking the port

### "Service unavailable"

```
LLM request failed: Service unavailable
```

**Causes**:
- Provider experiencing outage
- Server overloaded

**Solutions**:

1. Check provider status page:
   - [OpenAI Status](https://status.openai.com/)
   - [Anthropic Status](https://status.anthropic.com/)
   - [Google Cloud Status](https://status.cloud.google.com/)

2. Wait and retry (Strix has automatic retry with backoff)

3. Switch to alternative provider temporarily

## Model Errors

### "Model not found"

```
LLM request failed: Model not found
```

**Causes**:
- Incorrect model identifier
- Model not available in your region
- Model requires approval/access

**Solutions**:

1. Verify model identifier format:
   ```bash
   # Correct format: provider/model-name
   export STRIX_LLM="openai/gpt-5"
   export STRIX_LLM="anthropic/claude-sonnet-4-5"
   export STRIX_LLM="gemini/gemini-2.5-pro"
   ```

2. Check model name spelling and version

3. For local models, verify the model is downloaded:
   ```bash
   ollama list  # Ollama
   ```

4. Some models require access approval (e.g., GPT-5)

### "Unsupported parameters"

```
LLM request failed: Unsupported parameters
```

**Causes**:
- Model doesn't support certain features

**Solutions**:

1. Strix uses `litellm.drop_params = True` to handle most cases

2. If persistent, try a different model

3. Report the issue with model details

## Docker/Sandbox Errors

### Windows Docker Issues

> **Warning**: Docker on Windows has known compatibility issues with Strix.
>
> Common problems include path mounting, networking, and container isolation differences.
>
> **Recommended**: Use WSL2 (Windows Subsystem for Linux) with Docker Desktop for best results. Native Windows Docker is not fully supported.

### "Docker not running"

**Solutions**:

1. Start Docker:
   ```bash
   # Linux
   sudo systemctl start docker

   # macOS/Windows
   # Start Docker Desktop
   ```

2. Verify Docker is running:
   ```bash
   docker info
   ```

### "Permission denied" with Docker

**Cause**: Your user is not in the docker group.

**Solution** (Required):

```bash
sudo usermod -aG docker $USER
```

Then **restart your PC** for the change to take effect.

**Workaround** (if you must avoid restart): Run `newgrp docker` in your terminal session. This only applies to that session - you should still restart when convenient.

To verify you're in the docker group:
```bash
groups | grep docker
```

### "Sandbox image pull failed"

**Solutions**:

1. Check internet connectivity

2. Manually pull the image:
   ```bash
   docker pull ghcr.io/usestrix/strix-sandbox:0.1.10
   ```

3. Check Docker Hub/GHCR status

### "Container failed to start"

**Solutions**:

1. Check Docker logs:
   ```bash
   docker logs <container_id>
   ```

2. Ensure sufficient disk space

3. Check for port conflicts

## Timeout Errors

### "Request timed out"

```
LLM request failed: Request timed out
```

**Causes**:
- Large input taking too long to process
- Network latency
- Model overloaded

**Solutions**:

1. Increase timeout:
   ```bash
   export LLM_TIMEOUT=900  # 15 minutes
   ```

2. For reasoning models (o3, Claude), timeouts are expected to be longer

3. Use a faster model for initial scanning

## Vision/Screenshot Issues

### Screenshots not working

**Causes**:
- Model doesn't support vision
- Image encoding issues

**Solutions**:

1. Verify your model supports vision:
   - GPT-5: Yes
   - Claude Sonnet/Opus: Yes
   - Gemini: Yes
   - Most local models: No

2. Check browser is launching correctly (Strix logs)

3. For local models, vision is severely limited - consider cloud

### "Content policy violation"

```
LLM request failed: Content policy violation
```

**Causes**:
- Security testing content triggered safety filters

**Solutions**:

1. Some providers have stricter filters than others
2. Try Anthropic Claude (more flexible for security research)
3. For Azure, review content filtering settings
4. Frame requests in professional security testing context

## Performance Issues

### Very slow responses

**Causes**:
- Large context
- Complex reasoning
- Rate limiting (backing off)
- Local model hardware limitations

**Solutions**:

1. Check if rate limiting is causing delays
2. Use a faster model tier
3. For local models, see [Hardware Requirements](local-models/hardware-requirements.md)
4. Reduce codebase scope

### High costs

**Solutions**:

1. Use prompt caching (automatic with Anthropic)
2. Target specific directories, not entire codebases
3. Use budget-friendly models for initial scans:
   ```bash
   export STRIX_LLM="openrouter/openai/gpt-4o-mini"
   ```
4. Monitor usage in provider dashboard

## Getting Help

If you're still experiencing issues:

1. **Check existing issues**: [GitHub Issues](https://github.com/usestrix/strix/issues)

2. **Search Discord**: [Strix Discord](https://discord.gg/YjKFvEZSdZ)

3. **Report a bug**: Include:
   - Strix version (`strix --version`)
   - Python version
   - Operating system
   - Full error message
   - Steps to reproduce
   - Model being used (with sensitive info redacted)

4. **For provider-specific issues**: Contact the LLM provider's support
