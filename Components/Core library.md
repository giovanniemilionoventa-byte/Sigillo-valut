---
tags: [component]
source: code (packages/core)
---
# Core library (`packages/core`)

Pure TypeScript library shared by [[Server]], [[Signer]] and [[Verifier]]. **No network, no disk, no clock, no environment variables** (repo rule 4, enforced by `pnpm lint`). Time, keys and randomness are passed in.

| file | what |
|---|---|
| `receipt.ts` | zod schemas v1–v4, hashing, see [[Receipt]] |
| `canonical.ts` | RFC 8785 JCS (`canonicalize` package) |
| `signing.ts`, `keys.ts`, `base64.ts` | Ed25519, key ids, see [[Signatures and keys]] |
| `merkle.ts`, `merkle-frontier.ts` | [[Merkle tree]] |
| `checkpoint.ts` | [[Checkpoint]] |
| `anchoring.ts`, `gentime.ts` | [[Anchoring]], DER genTime reader |
| `privacy.ts` | pseudonyms, salted digests |
| `text.ts` | `sigillo-text/1`, see [[Artifacts and text fingerprints]] |
| `export.ts`, `manifest.ts`, `zip.ts` | [[Evidence pack]] layout; ZIP writer with no dependency |
| `version.ts` | `SIGILLO_VERSION = "0.1.0"` |

Test vectors: `packages/core/test/vectors.json`, re-derived independently in Python by `scripts/crosscheck_vectors.py` (CI).
