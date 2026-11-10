---
tags: [component, security]
source: code (apps/keyholder/src, apps/server/src/gateway/key-box.ts, keyholder-client.ts)
---
# Keyholder

Separate container that holds the customers' **model provider API keys** (OpenAI, Anthropic, …) for the [[LLM gateway]], so the [[Server]] never sees them in clear.

- HTTP over a Unix socket. Routes: `GET /public-key`, `POST /adopt` (one-time move of the old AES key), `ANY /call/<provider>/<path>` with headers `x-sigillo-system` and `x-sigillo-sealed`.
- The server seals each key with the keyholder's public key: `box1:` = RSA-3072 OAEP-SHA256 wrapping an AES-256-GCM key; associated data binds system + provider, so a sealed key cannot be reused for another account or provider.
- The keyholder opens the box and calls the provider itself; upstream address only from its own catalogue.
- Older modes still in code: legacy file `llm-gateway.key` (AES-GCM inside the server) and a passphrase **key vault** (`SIGILLO_KEY_VAULT`, off, suspended 2026-10-23).

Related: [[LLM gateway]], [[Security model]].
