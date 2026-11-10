---
tags: [concept]
source: code (packages/core/src/text.ts, receipt.ts; verifier `doc`)
---
# Artifacts and text fingerprints

An action can name the documents it used: `artifacts[{role, label, media_type, sha256, text?}]` (receipt v2+). Sent by the SDK as OTLP span events `sigillo.artifact` (`artifact()` helper); not available on the native API.

- `sha256` is the exact bytes. `text` (v3+) is a fingerprint of the **text** with canon `sigillo-text/1`: strip soft hyphen, zero-width space, word joiner, BOM; Unicode NFC; collapse whitespace runs to one space; trim. So the same text copied from a PDF or a Word file still matches.
- Same JS code runs in the browser page "verify a document" on the site.
- `sigillo-verify doc <pack> <file>` finds which receipts used a file. Table `artifacts` holds the index; export file `artifacts-index.jsonl`.

Related: [[Receipt]], [[Verifier]].
