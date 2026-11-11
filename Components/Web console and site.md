---
tags: [component, ui]
source: code (apps/server/src/http/*)
---
# Web console and public site

Server-rendered HTML (no front-end framework), English by default, Italian with `SIGILLO_ITALIAN=1` (`strings-en.ts`, `strings-it.ts`).

## Console `/ui`
Views **Status / Actions / Evidence** (console v2, `SIGILLO_CONSOLE_V2`): chain colour ([[Chain health]]), connection state, list of actions and runs (`SIGILLO_RUNS`), export, "seal now", API keys, provider keys for the [[LLM gateway]], agent setup wizard (`agent-setup.ts`, `agent-detect.ts`), pack reader (`SIGILLO_PACK_READER`), transfer of a system (`SIGILLO_TRANSFER`). Operator area `/ui/admin`.

First steps (PR #153, 2026-11-11): welcome list + optional example register `<org>.esempio` (`sample-system.ts`, `SIGILLO_SAMPLE_SYSTEM`), first-receipt hint on Stato until the first seal, offer of a 20-minute call.

## Public site (`site-v2.ts`, `site-demo.ts`, `site-blog.ts`, `site-legal.ts`, `site-privacy.ts`)
Home, how it works, AI Act section, prices, blog, demo zone with a 4-step tutorial, `/verify` (check a pack or a document in the browser), `/terms`, `/dpa`, privacy. Prices: see [[Business model and prices]].

Caddy in front sets TLS and a strict CSP ([[Deployment]]).
