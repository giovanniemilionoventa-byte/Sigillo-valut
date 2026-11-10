---
tags: [privacy]
source: code (apps/server/src/assurance/privacy-check.ts, storage) + docs (DATA-PROTECTION.md)
---
# Data protection

- **No plaintext** of prompts, tool arguments or outputs is ever stored or logged; only hashes (repo rule 2). The SDK filters content on the client; the server salts any raw value and drops it ([[Salted digests and openings]]).
- People are pseudonyms ([[Pseudonyms and subjects]]).
- Logs: method, path without query, IP only. Caddy filters logs too.
- `STORAGE_INVENTORY` in `privacy-check.ts` classifies every DB column (a test fails if a new column is not classified); `sigillo-server privacy inventory` prints it, `privacy scan "<phrase>"` searches DB, logs and backups for a marker.
- Erasure tools: [[Flow - erasure]].
- Hosting in Italy (Aruba) [docs]. Legal side: [[Legal and compliance]].
