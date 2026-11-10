---
tags: [operations, security]
source: code (apps/server/src/auth/throttle.ts, http/server.ts, cli.ts)
---
# Limits and throttling

| limit | default | variable |
|---|---|---|
| receipts per organization per month | 10,000 | `SIGILLO_ORG_MONTHLY_RECEIPTS` |
| receipts per system per minute (in memory) | 3,000 | `SIGILLO_SYSTEM_RECEIPTS_PER_MINUTE` |
| systems per organization | 100 | `SIGILLO_ORG_MAX_SYSTEMS` |
| bad API keys per IP | 20/min → lock 1 min, doubling to 15 | `SIGILLO_INGEST_MAX_FAILURES` |
| console logins | 5 in 15 min → lock 5 min, doubling to 60 | `SIGILLO_LOGIN_*` |
| operator logins, global | 50/h → lock 15 min up to 4 h | — |
| request body | 8 MiB (Fastify), 16 MB (Caddy), receipt 192 KiB | — |

The 100,000 Standard plan quota and billing are **not built** (only one quota for all organizations). See [[Business model and prices]].
