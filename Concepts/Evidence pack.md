---
tags: [concept, export]
source: code (packages/core/src/export.ts, manifest.ts, zip.ts; apps/server/src/export/*)
---
# Evidence pack (export zip)

What the customer hands to an auditor or a judge. One zip per system and range:

| file | content |
|---|---|
| `manifest.json` | version, system, range, counts, public keys |
| `receipts.jsonl` | every [[Receipt]], canonical JSON with signature |
| `checkpoints.jsonl` | [[Checkpoint]]s with inclusion proofs and timestamp references |
| `timestamps/*.tsr` | RFC 3161 tokens |
| `artifacts-index.jsonl` | documents used |
| `openings.jsonl` | only with `--open`, see [[Salted digests and openings]] |
| `subjects.jsonl` | only with `--subject` |
| `report.pdf` | human report (pdfkit), English or Italian |
| `VERIFY.md` | how to check it yourself |

The server **verifies the archive with the real verifier before writing the report**. Made from the console, the CLI (`sigillo-server export`), or every night by the [[Daily exporter]]. Exports are recorded in `export_register`.

Related: [[Verifier]], [[Flow - export and verification]].
