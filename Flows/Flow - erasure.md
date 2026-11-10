---
tags: [flow, privacy]
---
# Flow: erasure (GDPR)

Receipts are append-only and must stay verifiable, so erasure removes the **keys to meaning**, not the receipts:
- A person: `sigillo-server subject erase <identifier>` → `subjects` row deleted, WAL truncated. The pseudonym stays, nobody can link it back.
- Contents: `openings erase` → nonces deleted; salted hashes can no longer be opened.
- A customer leaving: `org erase` → all its systems, receipts, keys deleted (allowed exception to append-only).
- An empty system: `system delete`. Operator's own system: dedicated erase.
- Check with `privacy scan "<phrase>"`.
Every action goes to `admin_log`. See [[Pseudonyms and subjects]], [[Salted digests and openings]], [[Data protection]].
