---
tags: [component]
source: code (apps/server/src/checkpoint/checkpointer.ts, timestamp/rfc3161.ts)
---
# Checkpointer

Runs at start-up and every `SIGILLO_CHECKPOINT_MINUTES` (60): for each system with new receipts asks the [[Signer]] for a [[Checkpoint]], checks it against the DB, stores it, then gets a [[Timestamp]] from `TSA_URL` (and `TSA_URL_2` best effort). Retries pending timestamps every 5 min. "Seal now" from the console triggers a run (minimum 10 s apart). CLI: `sigillo-server checkpoint` (note: uses only `TSA_URL`, see [[Code defects]]).

Related: [[Flow - hourly seal]].
