# LM Studio Setup

[LM Studio](https://lmstudio.ai/) is a desktop application for running local LLMs with a user-friendly interface. It provides an OpenAI-compatible API server.

> **Important**: Read the [Local Models Overview](index.md) first to understand the limitations of local models for Strix.

## Installation

1. Download LM Studio from [lmstudio.ai](https://lmstudio.ai/)
2. Install the application
3. Launch LM Studio

Supported platforms: Windows, macOS, Linux

## Downloading Models

1. Open LM Studio
2. Click the **Search** tab (magnifying glass icon)
3. Search for a model (e.g., "qwen2.5" or "codestral")
4. Click **Download** on your chosen model
5. Wait for the download to complete

### Recommended Models

| Model | Search Term | Size | Notes |
|-------|-------------|------|-------|
| Qwen 2.5 72B Instruct | `qwen2.5-72b-instruct` | ~40GB | Best for coding |
| Llama 3.3 70B | `llama-3.3-70b` | ~40GB | Good general capability |
| Codestral 22B | `codestral-22b` | ~13GB | Code-focused, smaller |
| Deepseek Coder V2 | `deepseek-coder-v2` | Varies | Code specialized |

Choose GGUF quantized versions (Q4_K_M, Q5_K_M, Q6_K) based on your VRAM.

## Starting the API Server

1. Load a model in LM Studio (click on downloaded model)
2. Go to the **Local Server** tab (left sidebar)
3. Click **Start Server**
4. Note the server address (default: `http://localhost:1234`)

### Server Settings

- **Port**: Default 1234 (can be changed)
- **GPU Layers**: Set based on VRAM (higher = faster, more VRAM)
- **Context Length**: Maximum tokens (affects VRAM usage)

## Configuration for Strix

```bash
export STRIX_LLM="openai/qwen2.5-72b-instruct"
export LLM_API_BASE="http://localhost:1234/v1"
export LLM_API_KEY="lm-studio"
```

Notes:
- Use `openai/` prefix (LM Studio provides OpenAI-compatible API)
- The model name should match what's loaded in LM Studio
- The API key can be any string (LM Studio doesn't validate it)

## Recommended Models for Strix

| Model | VRAM (Q4) | VRAM (Q6) | Notes |
|-------|-----------|-----------|-------|
| Qwen 2.5 72B Instruct | 42GB | 56GB | Best local coding model |
| Llama 3.3 70B Instruct | 40GB | 54GB | Good general use |
| Codestral 22B | 13GB | 17GB | Smaller, code-focused |
| Deepseek Coder V2 Lite | 10GB | 13GB | Lightweight option |

## Quantization Guide

GGUF files come in different quantization levels:

| Quantization | Quality | Size | Use When |
|--------------|---------|------|----------|
| Q8_0 | Highest | Largest | You have plenty of VRAM |
| Q6_K | Very Good | Large | Good balance |
| Q5_K_M | Good | Medium | Recommended default |
| Q4_K_M | Acceptable | Smaller | Limited VRAM |
| Q3_K_M | Lower | Smallest | Very limited VRAM |

For Strix, use Q5_K_M or Q6_K for best results.

## Performance Settings

### GPU Offloading

In LM Studio's model settings:

1. **GPU Layers**: Set to max layers for full GPU usage
2. Reduce if you get out of memory errors
3. "0" means CPU-only (very slow)

### Context Length

1. Default is often 4096 tokens
2. Increase for larger codebases (if VRAM allows)
3. Each doubling roughly doubles VRAM usage

### Batch Size

1. Affects throughput
2. Lower if experiencing memory issues
3. Default is usually fine

## Verifying Setup

Test the LM Studio server:

```bash
# Check server is running
curl http://localhost:1234/v1/models

# Test completion
curl http://localhost:1234/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen2.5-72b-instruct",
    "messages": [{"role": "user", "content": "Hello!"}]
  }'
```

## Common Issues & Troubleshooting

### "Connection refused"
```
LLM request failed: Connection error
```
**Solutions**:
1. Ensure LM Studio server is running (check Local Server tab)
2. Verify the port matches your `LLM_API_BASE`
3. Check firewall settings

### "Model not found"
```
LLM request failed: Model not found
```
**Solutions**:
1. Verify a model is loaded in LM Studio
2. Check the model name in your config matches LM Studio
3. Try using just the base model name

### Out of Memory
**Solutions**:
1. Reduce GPU layers
2. Use a smaller quantization (Q4_K_M instead of Q6_K)
3. Use a smaller model
4. Reduce context length

### Slow Generation
**Solutions**:
1. Increase GPU layers (if VRAM allows)
2. Use a smaller model
3. Use a more aggressive quantization
4. Reduce context length

### Vision Not Supported
LM Studio currently has limited vision model support. For vision:
1. Check for LLaVA or similar multimodal models
2. Consider cloud providers for vision requirements

## Strix-Specific Settings

```bash
# Increase timeout for slower local inference
export LLM_TIMEOUT=900

# Reduce concurrent requests
export LLM_RATE_LIMIT_CONCURRENT=1

# LM Studio configuration
export STRIX_LLM="openai/qwen2.5-72b-instruct"
export LLM_API_BASE="http://localhost:1234/v1"
export LLM_API_KEY="lm-studio"
```

## Tips for Best Results

1. **Start simple**: Test with a smaller model first
2. **Monitor VRAM**: Watch GPU memory in task manager/nvidia-smi
3. **Optimize quantization**: Q5_K_M offers good balance
4. **Set realistic expectations**: Local models are slower and less capable than cloud
5. **Check loaded model**: Ensure the right model is loaded before starting Strix
