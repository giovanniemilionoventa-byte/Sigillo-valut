---
tags: [security]
source: code + docs (SECURITY.md)
---
# Security model

## Trust split
| piece | holds | cannot |
|---|---|---|
| [[Server]] | data, accounts | sign, read provider keys |
| [[Signer]] | signing key, own chain heads | reach the network |
| [[Keyholder]] | provider keys | sign receipts |
| [[Verifier]] | nothing | — runs at the customer's or auditor's |

## What a pack proves
Receipts not changed after signing; none removed, added or reordered; the chain existed at a time ([[Timestamp]]); a file or text matches a recorded fingerprint.

## What it does not prove (always say it)
- That recording is **complete**: an agent that bypasses the gateway or patches the SDK in its own process is not recorded ([[Heartbeat and guard]] and [[Tool reconciler]] reduce this, do not remove it).
- That what the agent declares is true, or the real effect of an action.
- `ts_event` (only `ts_received` is trusted).
- Timestamps are **not eIDAS qualified**.
- No local queue: if the server is unreachable for more than about 10 s, those actions are lost.
- A cut tail shows only with `--previous`; key ownership only with `--key-id`.
- A compromised server can append false receipts at the end, not rewrite the past.

Words: say "tamper-evident", "independently verifiable"; never "tamper-proof", "compliant", "AI Act ready", "certified".

What an attacker who breaks into the server sees and can do (Italian, 2026-11-10): [[Cosa vede un attaccante sul server]].
