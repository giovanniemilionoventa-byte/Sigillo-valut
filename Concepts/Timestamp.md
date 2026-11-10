---
tags: [concept, crypto, external]
source: code (apps/server/src/timestamp/rfc3161.ts, packages/verifier/src/timestamps.ts)
---
# Timestamp (RFC 3161)

An outside authority (TSA) signs "this hash existed at time T". Sigillo asks it over the [[Checkpoint]] `root_hash`.

- Request built with `openssl ts -query -sha256 -no_nonce -cert`, sent as `application/timestamp-query`, 20 s timeout, 3 attempts; pending ones retried every 5 min (`SIGILLO_TSA_RETRY_MINUTES`).
- `TSA_URL` (default in compose: freetsa.org) is the main one. `TSA_URL_2` (compose: DigiCert) is best effort for checkpoints of the last 24 h. Optional basic auth `TSA_USERNAME` / `TSA_PASSWORD(_FILE)`.
- Stored in table `timestamps`, exported as `timestamps/checkpoint-<size>-<n>.tsr`.
- The [[Verifier]] checks tokens with `openssl ts -verify` against a CA file (`--tsa-ca`). Status: `verified`, `imprint-only` (hash matches, certificate not checked), `failed`, `not-checked`. It also re-reads `genTime` itself from the DER (`gentime.ts`).
- **Neither default authority is eIDAS qualified.** A qualified one needs a company (decision 2026-09-21: wait). See [[Legal and compliance]].

Related: [[Anchoring]].
