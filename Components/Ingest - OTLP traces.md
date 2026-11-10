---
tags: [component, ingest]
source: code (apps/server/src/ingest/adapter.ts, otlp.ts; http/server.ts)
---
# Ingest: OTLP traces (`POST /v1/traces`)

The main door. Agents instrumented with OpenTelemetry (through the [[Python SDK]] or [[TypeScript SDK]], or any OTLP exporter) send spans here, JSON or protobuf, with `Authorization: Bearer sigillo_<16hex>_<64hex>`.

## Span → receipt
- OpenInference spans (`openinference.span.kind`) are read first, else OpenTelemetry GenAI (`gen_ai.*`); other spans are ignored. Unknown kinds become `agent_step` and are reported.
- `outcome`: status ok/error; unset → ok, unless `error.type` (error) or an exception event (unknown).
- Hashes: `sigillo.input.sha256` / `.nonce` from the SDK; otherwise the raw value is salted on the server and dropped ([[Salted digests and openings]]).
- Artifacts from span events `sigillo.artifact`; model from `gen_ai.request.model` / `llm.model_name`.
- `actor.agent` = agent name, provider or `service.name`; `on_behalf_of` = `user.id` / `enduser.id`.
- Sorted by start time; `ts_event` = span start.

## Answers
401 bad key · 503 paused (`SIGILLO_INGEST_PAUSED`) · 429 monthly quota or per-minute rate · 403 stdlib client not enabled · 200 `{partialSuccess:{}, sigillo:{accepted, duplicates, ignored, unknown, rawContentHashed}}`. Signer down → 503.

Related: [[Native receipts API]], [[Limits and throttling]], [[Flow - agent action to receipt]].
