---
tags: [concept, crypto]
source: code (packages/core/src/receipt.ts, apps/signer/src/signer.ts)
---
# Hash chain

Each [[Receipt]] carries `prev_hash`, the hash of the receipt before it. Receipt 0 is the **genesis** (`action.kind = genesis`, `prev_hash` = 64 zeros). Changing, removing, adding or reordering any receipt breaks every link after it.

- One chain per system (`system_id`).
- The [[Signer]] keeps its own copy of the head of every chain (seq + hash) and refuses any receipt whose `seq` is not head+1 or whose `prev_hash` is not the head hash. So even the [[Server]] cannot fork or rewrite a chain without the signer noticing.
- The [[Verifier]] re-walks the links (checks 4 to 6: sequence, genesis, chain link).
- What the chain cannot show alone: that the **tail** was not cut. That is what [[Checkpoint]] + [[Timestamp]] and the verifier's `--previous` option are for.

Related: [[Merkle tree]], [[Chain health]].
