---
tags: [concept, crypto]
source: code (packages/core/src/merkle.ts, merkle-frontier.ts)
---
# Merkle tree

RFC 6962 style (the Certificate Transparency tree): leaf = SHA-256(0x00 ‖ receipt hash bytes), node = SHA-256(0x01 ‖ left ‖ right).

- The [[Signer]] keeps a compact **frontier** of each chain's tree in its state file, so it can compute the root without the database.
- A [[Checkpoint]] signs `tree_size` + `root_hash`.
- Exports include **inclusion proofs** (first and last receipt of each checkpointed tree) so the [[Verifier]] can prove a receipt is in the sealed tree, and rebuilds the full root when the pack starts at seq 0.

Related: [[Hash chain]], [[Evidence pack]].
