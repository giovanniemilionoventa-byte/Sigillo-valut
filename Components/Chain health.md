---
tags: [component]
source: code (apps/server/src/health/chain-health.ts)
---
# Chain health

Every minute re-checks new receipts' links and signatures incrementally. A failure is **sticky red**.

- Red: broken link or signature, signer down, divergence with the signer.
- Yellow: no actions yet, stale (> `SIGILLO_STALE_AFTER_MINUTES`, default 1440) unless a heartbeat is open, lost connection, checkpoint missing or timestamp late (> `SIGILLO_MAX_ANCHOR_DELAY_MINUTES`, 60), not sealed.
- Green otherwise.
Shown in the console Status view and `/healthz`.

Related: [[Heartbeat and guard]], [[Checkpointer]].
