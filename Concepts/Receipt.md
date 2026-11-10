---
tags: [concept, format]
source: code (packages/core/src/receipt.ts)
---
# Receipt

The unit of evidence. One receipt = one action of an AI agent (a model call, a tool call, a step, a decision), signed and linked to the previous one in a [[Hash chain]].

## Fields (version 4, the only version the server writes today)

| field | meaning |
|---|---|
| `v` | schema version, 1 to 4. Never change the format without raising it (repo rule). |
| `system_id` | the agent system the chain belongs to, at most 128 chars |
| `seq` | position in the chain, 0 = genesis |
| `ts_event` | time the agent says the action happened. **Not trusted.** |
| `ts_received` | time the server received it. Trusted (checked by the [[Signer]] within ±5 min, never earlier than the previous one). |
| `actor.agent` | name of the agent |
| `actor.on_behalf_of` | in v4 only a pseudonym `psn_` + 32 hex, see [[Pseudonyms and subjects]] |
| `action.kind` | `tool_call`, `llm_call`, `agent_step`, `decision`, `genesis` |
| `action.name` | at most 256 chars. Prefixes `sigillo.` and `model_requested.` are reserved to the server. |
| `input_hash`, `output_hash` | SHA-256 hex or null. Never the content itself. |
| `input_hash_scheme`, `output_hash_scheme` | v4: `plain`, `salted` or null, see [[Salted digests and openings]] |
| `outcome` | `ok`, `error`, `blocked`, `unknown` |
| `source` | `{type: otlp|sdk|api, trace_id?, span_id?}` |
| `artifacts` | v2+: documents the action used, see [[Artifacts and text fingerprints]] |
| `model` | v2+: `{name, provider, digest}` |
| `prev_hash` | hash of the previous receipt, 64 zeros for genesis |
| `key_id` | first 16 hex of SHA-256 of the raw public key, see [[Signatures and keys]] |
| `sig` | Ed25519 signature, base64, 88 chars |

## Versions
- v1: base fields.
- v2: adds `artifacts` and `model`.
- v3: artifacts may carry a `text` fingerprint (`sigillo-text/1`).
- v4: pseudonymous `on_behalf_of` and hash schemes. Older versions stay verifiable.

## How it is hashed
`hash = SHA-256(JCS(receipt without sig))` where JCS is RFC 8785 canonical JSON (`canonical.ts`). The signature is over those 32 bytes.

Related: [[Hash chain]], [[Checkpoint]], [[Core library]], [[Flow - agent action to receipt]].
