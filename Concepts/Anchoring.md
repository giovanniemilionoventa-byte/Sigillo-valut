---
tags: [concept]
source: code (packages/core/src/anchoring.ts)
---
# Anchoring (what time a receipt is proven by)

The [[Verifier]] turns timestamped [[Checkpoint]]s into "receipt N existed no later than T" ranges.

- Error `anchor-time`: a timestamp earlier than the checkpoint it covers, beyond the clock tolerance (default 5 min, `--clock-tolerance`).
- Error `anchor-order`: timestamps out of order along the chain.
- Warning `anchor-delay`: checkpoint timestamped later than `--max-anchor-delay` (default 60 min) after it was made. `--strict` turns warnings into failure.
- Receipts after the last timestamped checkpoint are the **unproven tail**; the report shows it and the longest gap.

Related: [[Timestamp]], [[Evidence pack]].
