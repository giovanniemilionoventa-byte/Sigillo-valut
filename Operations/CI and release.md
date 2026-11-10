---
tags: [operations]
source: code (.github/workflows, scripts/lint.mjs)
---
# CI and release

- `ci.yml`: Node 22 job (lint → typecheck → build → test → `smoke-dist.mjs`), Python job (SDK + demos), vectors cross-check (`crosscheck_vectors.py`). About 1,700 Node tests (1713 on 2026-11-10), including browser tests with Chromium.
- `dependency-audit.yml`: `pnpm audit` + `pip-audit` on every push and every Monday.
- `verifier-release.yml`: on `main`, publishes release `verifier` with `sigillo-verify.zip` + sha256.
- `pnpm lint` (`scripts/lint.mjs`) checks architecture, not style: core purity, dependency allow-list, no `.only`, no `any`, no key material.
- Rules: never push to `main`; work on a branch + PR.
