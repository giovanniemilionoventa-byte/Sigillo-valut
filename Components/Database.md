---
tags: [component, storage]
source: code (apps/server/src/storage/schema.ts, store.ts)
---
# Database (SQLite)

`better-sqlite3`, WAL, `synchronous=FULL`, foreign keys, `secure_delete=ON`; a second read-only connection for pages. Schema version **10** (`PRAGMA user_version`), upgraded with `sigillo-server migrate`.

## Tables
| table | holds |
|---|---|
| `systems` | agent systems (display name, archived, owning organization) |
| `organizations`, `users` | customer accounts ([[Accounts and authentication]]) |
| `api_keys` | API keys, only a hash of the secret |
| `transfer_links` | links to hand a system to another account (7 days, hash only) |
| `receipts` | canonical JSON, signature, hash (unique), trace/span ids |
| `checkpoints`, `timestamps` | [[Checkpoint]]s and [[Timestamp]] tokens |
| `artifacts` | document fingerprints |
| `signing_keys` | known public keys |
| `export_register` | every export made |
| `connections` | SDK sessions for the [[Heartbeat and guard]] |
| `provider_keys`, `vaults`, `vault_schedule` | sealed model keys for the [[LLM gateway]] |
| `admin_log` | every administrative action |
| `subjects` | identifier ↔ pseudonym, see [[Pseudonyms and subjects]] |
| `openings` | nonces of salted digests, see [[Salted digests and openings]] |

## Append-only
Triggers forbid UPDATE/DELETE on receipts, checkpoints, timestamps, artifacts, admin_log, signing_keys, with narrow exceptions: deleting an empty (genesis-only) system, the operator erasing its own system, and erasing a whole organization. A deleted `system_id` is never reused.

## Writing
`ReceiptStore` has one write queue; each receipt: `BEGIN IMMEDIATE` → build v4 receipt → ask the [[Signer]] → insert → commit. Max receipt 192 KiB. OTLP duplicates dropped by unique (system, trace_id, span_id).

Related: [[Flow - agent action to receipt]], [[Backups and updates]].
