---
tags: [component, ingest, api]
source: code (apps/server/src/http/server.ts)
---
# Native receipts API (`POST /api/v1/receipts`)

For callers without OpenTelemetry. Body:
`{system_id?, ts_event?, actor{agent, on_behalf_of?}, action{kind, name}, outcome, source?{type sdk|api, trace_id, span_id}, input_hash | input, output_hash | output, input_nonce | output_nonce}` (nonce = 64 hex or `"customer"`).

Answer `201 {seq, system_id, ts_received, key_id}`. No artifacts or model on this route. Same key, quota and rate rules as [[Ingest - OTLP traces]].

Other routes: `GET /healthz` (503 if signer unhealthy or a chain broken), `POST /api/v1/heartbeat` ([[Heartbeat and guard]]).
