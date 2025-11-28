# Ollama Setup

[Ollama](https://ollama.ai/) is the easiest way to run local LLMs on Linux, Windows and macOS. This guide covers setting up Ollama for use with Strix.

> **Important**: Read the [Local Models Overview](index.md) first to understand the limitations of local models for Strix.

## Installation

### Linux

```bash
curl -fsSL https://ollama.ai/install.sh | sh
```

### macOS

```bash
brew install ollama
```

Or download from [ollama.ai/download](https://ollama.ai/download)

### Windows

Download the installer from [ollama.ai/download](https://ollama.ai/download)

## Starting Ollama

```bash
# Start the Ollama server
ollama serve
```

The server runs on `http://localhost:11434` by default.

## Pulling Models

Download models before using them with Strix:

```bash
# Recommended: Qwen 2.5 for coding tasks (no vision)
ollama pull qwen2.5:72b

# Alternative: Llama 3.3 (no vision)
ollama pull llama3.3:70b

# Vision-capable (requires significant VRAM)
ollama pull llama3.2-vision:90b

# Smaller options for testing
ollama pull qwen2.5:32b
ollama pull codestral:22b
```

## Configuration for Strix

Strix requires three environment variables for Ollama. The format is:

```bash
export STRIX_LLM="ollama/<model-name>"
export LLM_API_BASE="http://<ip-address>:11434"
export LLM_API_KEY="1234"  # Required placeholder value
```

### Basic Setup

```bash
export STRIX_LLM="ollama/mistral"
export LLM_API_BASE="http://192.168.1.100:11434"
export LLM_API_KEY="1234"
```

> **Note**: Use your server's IP address (e.g., `192.168.1.100`), not `localhost`. The `LLM_API_KEY` is required but can be any placeholder value.

### With Vision Model

```bash
export STRIX_LLM="ollama/llama3.2-vision:90b"
export LLM_API_BASE="http://192.168.1.100:11434"
export LLM_API_KEY="1234"
```

### Model Name Format

The `STRIX_LLM` format is `ollama/<model-name>`:

| Model | STRIX_LLM Value |
|-------|-----------------|
| Mistral | `ollama/mistral` |
| Qwen 2.5 72B | `ollama/qwen2.5:72b` |
| Llama 3.3 70B | `ollama/llama3.3:70b` |
| Llama 3.2 Vision | `ollama/llama3.2-vision:90b` |
| Codestral | `ollama/codestral:22b` |

## Recommended Models for Strix

| Model | Command | VRAM | Vision | Notes |
|-------|---------|------|--------|-------|
| Qwen 2.5 72B | `ollama pull qwen2.5:72b` | 48GB+ | No | Best local coding model |
| Llama 3.3 70B | `ollama pull llama3.3:70b` | 48GB+ | No | Good general capability |
| Qwen 2.5 32B | `ollama pull qwen2.5:32b` | 24GB+ | No | Smaller alternative |
| Llama 3.2 Vision 90B | `ollama pull llama3.2-vision:90b` | 48GB+ | Yes | Only if you need vision |
| Codestral 22B | `ollama pull codestral:22b` | 16GB+ | No | Code-focused, smaller |

## Known Model Behavior

Based on community testing:

| Model | Status | Notes |
|-------|--------|-------|
| GPT-OSS 20B | Does not work well | Insufficient capability |
| GPT-OSS 120B | Partially works | May struggle with complex tasks |
| Qwen 2.5 | Untested | Should work in theory |
| Llama 3.3 70B+ | Best option | Most reliable for local |

> **Note**: Local model performance varies significantly. Cloud providers are strongly recommended for production use.

## Performance Tuning

### GPU Layers

Ensure your model uses GPU acceleration:

```bash
# Check GPU usage
nvidia-smi

# Set number of GPU layers (higher = more VRAM, faster)
OLLAMA_NUM_GPU=99 ollama serve
```

### Parallel Requests

For better throughput:

```bash
# Allow 2 parallel requests (requires more VRAM)
OLLAMA_NUM_PARALLEL=2 ollama serve
```

### Context Length

Some models support extended context:

```bash
# Modelfile with extended context
echo 'FROM qwen2.5:72b
PARAMETER num_ctx 32768' > Modelfile

ollama create qwen2.5-32k -f Modelfile
```

Then use:
```bash
export STRIX_LLM="ollama/qwen2.5-32k"
```

## Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `OLLAMA_HOST` | `localhost:11434` | Server address |
| `OLLAMA_NUM_PARALLEL` | `1` | Parallel request handling |
| `OLLAMA_NUM_GPU` | Auto | GPU layers to use |
| `OLLAMA_KEEP_ALIVE` | `5m` | Model keep-alive time |

## Verifying Setup

Test your Ollama installation:

```bash
# Check Ollama is running
curl http://localhost:11434/api/tags

# Test generation
curl http://localhost:11434/api/generate -d '{
  "model": "qwen2.5:72b",
  "prompt": "Hello, world!"
}'
```

## Common Issues & Troubleshooting

### "Connection refused"
```
LLM request failed: Connection error
```
**Solutions**:
1. Ensure Ollama is running: `ollama serve`
2. Check the port: `curl http://localhost:11434`
3. Verify `LLM_API_BASE` is set correctly

### "Model not found"
```
LLM request failed: Model not found
```
**Solutions**:
1. Pull the model first: `ollama pull qwen2.5:72b`
2. Verify model name: `ollama list`
3. Use full model name including size: `qwen2.5:72b` not just `qwen2.5`

### Out of Memory (OOM)
```
Error: out of memory
```
**Solutions**:
1. Use a smaller model
2. Reduce context length
3. Close other GPU-using applications
4. Use CPU inference (very slow): `OLLAMA_NUM_GPU=0`

### Slow Generation
**Causes**: GPU too small, model too large, context too long

**Solutions**:
1. Check GPU usage: `nvidia-smi`
2. Use a smaller model
3. Reduce `num_ctx` parameter
4. Ensure model fits in VRAM

### Vision Not Working
**Cause**: Most local models don't support vision

**Solutions**:
1. Use `llama3.2-vision:90b` (requires 48GB+ VRAM)
2. Accept that browser screenshots won't work
3. Consider cloud providers for vision support

## Strix-Specific Settings

Complete configuration for Strix with Ollama:

```bash
# Required: Model and connection
export STRIX_LLM="ollama/mistral"
export LLM_API_BASE="http://192.168.1.100:11434"
export LLM_API_KEY="1234"

# Recommended: Increase timeout for slower local inference
export LLM_TIMEOUT=900

# Recommended: Reduce concurrent requests
export LLM_RATE_LIMIT_CONCURRENT=1
```

## Remote Ollama Server

If running Ollama on a different machine:

```bash
# On the Ollama server - bind to all interfaces
OLLAMA_HOST=0.0.0.0:11434 ollama serve

# On the Strix machine - use server's IP address
export STRIX_LLM="ollama/mistral"
export LLM_API_BASE="http://192.168.1.100:11434"
export LLM_API_KEY="1234"
```

Replace `192.168.1.100` with your Ollama server's actual IP address.
