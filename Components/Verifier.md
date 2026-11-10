---
tags: [component, open-source]
source: code (packages/verifier/src: cli.ts, verify.ts, timestamps.ts)
---
# Verifier (`sigillo-verify`)

Small standalone CLI anyone can run on an [[Evidence pack]] without trusting Sigillo. Depends only on [[Core library]] + system `openssl` for timestamps. Kept small on purpose (repo rule 5).

## Checks, in order (stops at the first failure)
1. manifest and keys readable
2. every receipt matches its schema version
3. one system only
4. sequence without holes
5. genesis correct
6. chain links (`prev_hash`)
7. signature and key (`--key-id` makes the key trusted)
8. artifacts index matches receipts
9. openings, 10. subjects
11. checkpoint signatures
12. Merkle root rebuilt (when the pack starts at seq 0)
13. inclusion proofs
14. manifest range and counts
Then [[Timestamp]] tokens and [[Anchoring]] (errors/warnings).

## CLI
`sigillo-verify <pack.zip|dir> [--tsa-ca file] [--key-id id]... [--previous older-pack] [--clock-tolerance 5] [--max-anchor-delay 60] [--strict] [--quiet]`
- `--previous`: proves the new pack continues the old one (detects a cut tail).
- `doc <pack> <file>`: which receipts used this document.
- `open <pack> <seq> <role> --text|--file [--nonce] [--json]`: open a salted hash.
- Exit codes: 0 ok, 1 failed, 2 usage or read error.

Released as `sigillo-verify.zip` by `.github/workflows/verifier-release.yml`; also served by the server at the "verify" page (`http/verifier-bundle.ts`).

Related: [[Flow - export and verification]].
