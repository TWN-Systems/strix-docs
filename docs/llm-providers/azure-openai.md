# Azure OpenAI

[Azure OpenAI Service](https://azure.microsoft.com/en-us/products/ai-services/openai-service) provides enterprise-grade access to OpenAI models with Azure's security, compliance, and regional deployment options.

## Overview

**Why Azure OpenAI?**
- **Enterprise compliance**: SOC 2, HIPAA, GDPR compliance
- **Regional deployment**: Data residency requirements
- **Private endpoints**: VNet integration for security
- **Existing Azure integration**: Use existing Azure credentials
- **SLA guarantees**: Enterprise-level availability

## Getting Started

### Prerequisites

1. An Azure subscription
2. Access granted to Azure OpenAI Service (may require application)
3. An Azure OpenAI resource created in Azure Portal

### Creating a Deployment

1. Go to [Azure OpenAI Studio](https://oai.azure.com/)
2. Select your resource
3. Navigate to "Deployments"
4. Click "Create new deployment"
5. Select a model (e.g., gpt-4o, gpt-4-turbo)
6. Give it a deployment name (e.g., `strix-gpt4`)
7. Note your deployment name and endpoint

## Configuration

### Environment Variables

```bash
export STRIX_LLM="azure/your-deployment-name"
export LLM_API_KEY="your-azure-api-key"
export LLM_API_BASE="https://your-resource.openai.azure.com"
export AZURE_API_VERSION="2024-02-15-preview"
```

### Finding Your Credentials

1. **API Key**: Azure Portal → Your OpenAI Resource → Keys and Endpoint
2. **Endpoint**: Same location, listed as "Endpoint"
3. **Deployment Name**: Azure OpenAI Studio → Deployments

## Recommended Models

Deploy these models in Azure OpenAI Studio:

| Model | Suggested Deployment Name | Notes |
|-------|--------------------------|-------|
| gpt-4o | `strix-gpt4o` | Recommended default |
| gpt-4-turbo | `strix-gpt4-turbo` | Good balance |
| gpt-4o-mini | `strix-gpt4o-mini` | Cost-effective |

## LiteLLM Proxy Configuration (Optional)

```yaml
# litellm_config.yaml
model_list:
  - model_name: strix-azure
    litellm_params:
      model: azure/strix-gpt4o
      api_key: your-azure-key
      api_base: https://your-resource.openai.azure.com
      api_version: "2024-02-15-preview"
```

## Common Issues & Troubleshooting

### "Invalid API Key"
**Solutions**:
1. Use Key 1 or Key 2 from Azure Portal (not the full connection string)
2. Verify the resource is in an active state
3. Check if the key has been regenerated

### "Deployment not found"
**Solutions**:
1. Verify deployment name matches exactly (case-sensitive)
2. Check the deployment is in "Succeeded" state
3. Ensure you're using the correct resource endpoint

### "Resource not found"
**Solutions**:
1. Verify `LLM_API_BASE` is correct
2. Format: `https://YOUR-RESOURCE-NAME.openai.azure.com`
3. Don't include `/openai/deployments/` in the base URL

### "API version not supported"
**Solution**: Update the API version:
```bash
export AZURE_API_VERSION="2024-02-15-preview"
```

### Content Filtering Blocking Requests
Azure OpenAI has default content filters. For security testing:
1. Review Azure's content filtering documentation
2. Consider applying for modified content filtering
3. Some security testing prompts may require filter adjustments
