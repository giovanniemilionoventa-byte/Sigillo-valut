---
tags: [component, sdk]
source: code (sdk-python/src/sigillo)
---
# Python SDK (`sigillo`, 0.12.0)

Thin layer over OpenTelemetry. Python ≥ 3.10; deps `opentelemetry-sdk`, `opentelemetry-exporter-otlp-proto-http`; extras for LangChain, CrewAI, OpenAI (OpenInference).

## API
`sigillo.init(endpoint, api_key, system_id, instrument=[...], ollama_url, redact_content=True, salt_content=True, customer_key, heartbeat_seconds=60, show_sent, strict, on_halt, local_model)`, plus `artifact()`, `pseudonym()`, `nonce_for()`, `current_span_from_callbacks()`, `Tracing.tool()`, `flush()`, `shutdown()`.

## What it does
- **Content filter exporter**: before anything leaves the machine, replaces prompts and outputs with `sigillo.input.sha256` / `sigillo.output.sha256` (+ nonce or "customer"), keeps only an allow-list of attributes, reduces exceptions to their type. See [[Salted digests and openings]].
- [[Heartbeat and guard]]: heartbeat thread + guard process `_watchdog.py`; `strict` mode stops the agent.
- `_stdlib.py`: optional recording of urllib/requests/httpx and file writes (only when named in `instrument`).
- `door.py`: **local-model door** (`python -m sigillo.door`) in front of a local model (Ollama), records calls, raises `model_open` alarm if the model is reachable directly.
- `_gateway.py`: routes the Mistral client through the [[LLM gateway]] when `MISTRAL_BASE_URL` is set.

Not yet on PyPI. Related: [[TypeScript SDK]].
