---
tags: [concept, crypto]
source: code (packages/core/src/checkpoint.ts, apps/server/src/checkpoint/checkpointer.ts)
---
# Checkpoint

A signed statement "chain X had N receipts with Merkle root R at time T":
`{v:1, system_id, tree_size, root_hash, ts, key_id, sig}`, canonicalised and signed like a [[Receipt]].

- Built by the [[Signer]] from its own [[Merkle tree]] frontier and its own clock (request `CHECKPOINT`).
- The server compares `tree_size` and root with its database before storing it (table `checkpoints`).
- Made by the [[Checkpointer]] every hour (`SIGILLO_CHECKPOINT_MINUTES`, default 60), at start-up, and on "seal now" from the console. Nothing new → no checkpoint.
- Then sent to a [[Timestamp]] authority.

Related: [[Anchoring]], [[Flow - hourly seal]].
