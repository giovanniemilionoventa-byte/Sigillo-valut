---
tags: [concept, privacy, crypto]
source: code (packages/core/src/privacy.ts, apps/server/src/storage/store.ts, sdk-python nonce_for)
---
# Salted digests and openings

Content (prompts, tool arguments, outputs) is **never stored**, only hashes (repo rule 2). A plain SHA-256 of a short text can be guessed, so Sigillo salts it:

`salted digest = SHA-256(nonce32 ‖ JCS(value))`, scheme `salted` in the [[Receipt]] (`plain` = ordinary SHA-256 the client sent).

Three ways a digest is made (`store.ts` digestFor):
1. Client sends a hash only → `plain`.
2. Client sends hash + nonce → `salted`; the nonce goes in table `openings` so the customer can later **open** it. Nonce `"customer"` means the customer keeps it (derived with `nonce_for`: HMAC-SHA256(customer_key, "sigillo-nonce-v1\n"+trace+"\n"+span+"\n"+role)) and the server stores none.
3. Client sends the raw value (e.g. raw OTLP span attributes) → the server salts it with a fresh nonce and drops the value.

**Opening**: export with `--open <seqs>` includes `openings.jsonl`; then `sigillo-verify open <pack> <seq> <role> --text|--file` proves a given text was that input/output. `sigillo-server openings erase` deletes the nonces so nothing can be opened any more.

Related: [[Pseudonyms and subjects]], [[Data protection]].
