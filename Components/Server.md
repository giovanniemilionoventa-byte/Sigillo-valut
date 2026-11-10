---
tags: [component]
source: code (apps/server/src)
---
# Server (`sigillo-server`)

Node + Fastify process that receives agent actions, stores them, and serves the console and the public site. It **cannot sign**: every receipt goes through the [[Signer]].

## Parts
| folder | note |
|---|---|
| `storage/` | [[Database]] (SQLite) and `ReceiptStore` |
| `ingest/` | [[Ingest - OTLP traces]] |
| `http/server.ts` | ingest routes, [[Native receipts API]], [[Heartbeat and guard]] |
| `gateway/` | [[LLM gateway]] |
| `check/` | [[Tool reconciler]] |
| `health/` | [[Chain health]] |
| `checkpoint/`, `timestamp/` | [[Checkpointer]] |
| `export/`, `backup/` | [[Evidence pack]], [[Daily exporter]] |
| `auth/` | [[Accounts and authentication]] |
| `http/*` (pages, site, ui) | [[Web console and site]] |
| `assurance/privacy-check.ts` | [[Data protection]] inventory and scan |
| `cli.ts` | [[Server CLI]] |

## Start-up (`serve`)
connect signer → open DB → API keys → provider keys (keyholder or file) → reconcile with signer → health monitor → Checkpointer (runs now, then hourly) → Daily exporter → connection watch → tool reconciler → vault sweep (30 s).

Configuration: [[Configuration]]. Limits: [[Limits and throttling]].
