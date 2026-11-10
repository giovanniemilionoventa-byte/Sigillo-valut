---
tags: [concept, privacy]
source: code (packages/core/src/privacy.ts, storage/store.ts)
---
# Pseudonyms and subjects

`actor.on_behalf_of` (the person an agent acts for) is replaced by a pseudonym `psn_` + 32 hex (16 random bytes). The mapping identifier → pseudonym is in table `subjects` (identifier normalised: NFC, trimmed, lowercase).

- `sigillo-server subject find <identifier>` / `subject erase` (GDPR erasure: deletes the row, truncates the WAL; receipts stay valid but the link to the person is gone).
- Export `--subject <token>` adds `subjects.jsonl` so a reader can see who a pseudonym was.
- The SDKs can pseudonymise on the client already: `pseudonym()` = "p:" + 32 hex HMAC of the customer key.

Related: [[Salted digests and openings]], [[Flow - erasure]].
