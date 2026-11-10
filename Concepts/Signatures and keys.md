---
tags: [concept, crypto]
source: code (packages/core/src/signing.ts, keys.ts; apps/signer/src/key-file.ts)
---
# Signatures and keys

- Algorithm: **Ed25519** from `node:crypto` only (no external crypto library, repo rule).
- What is signed: the 32-byte SHA-256 digest of the canonical receipt (not re-hashed).
- `key_id` = first 16 hex chars of SHA-256 of the raw 32-byte public key.
- The private key lives **only** in the [[Signer]] container (PKCS8 PEM, file mode 0600; the signer refuses a key file with looser permissions and refuses to overwrite one).
- The server stores known public keys in table `signing_keys` and puts them in every export's `manifest.json`.
- A reader who wants to know *who* signed must pass the expected key with `sigillo-verify --key-id`; otherwise the verifier only proves the pack is self-consistent.

Related: [[Receipt]], [[Checkpoint]], [[Verifier]].
