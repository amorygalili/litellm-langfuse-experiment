# LiteLLM + Langfuse Proxy

Use LiteLLM as an OpenAI-compatible proxy to route requests to your LLM providers while automatically logging traces to Langfuse.

## Architecture

```text
Client (Roo Code, Open WebUI, Python)
                │
                ▼
         LiteLLM Proxy
          │         │
          ▼         ▼
     LLM Provider  Langfuse
```

## Start

```bash
docker compose up -d
```

The proxy is available at:

```
http://localhost:4000/v1
```

## Configure Clients

Use the proxy as an OpenAI-compatible endpoint.

| Setting | Value |
|---------|-------|
| Base URL | `http://<host>:4000/v1` |
| API Key | `sk-my-proxy-key` |

Works with:
- Roo Code
- Open WebUI
- Python/OpenAI SDK
- Any OpenAI-compatible client

## Test

```bash
curl http://localhost:4000/v1/chat/completions \
  -H "Authorization: Bearer sk-my-proxy-key" \
  -H "Content-Type: application/json" \
  -d '{
    "model":"gpt-5",
    "messages":[{"role":"user","content":"Hello!"}]
  }'
```

If successful, the request will appear in Langfuse.

## What Gets Logged

- Prompts
- Responses
- Model
- Token usage
- Cost
- Latency
- Errors

## Limitations

LiteLLM only logs LLM API requests. It does **not** capture:
- IDE actions
- MCP tool calls
- File reads/writes
- Internal agent reasoning

For full agent tracing, instrument the application with the Langfuse SDK or OpenTelemetry.

## References

- LiteLLM: https://docs.litellm.ai/
- Langfuse LiteLLM Integration: https://langfuse.com/integrations/gateways/litellm