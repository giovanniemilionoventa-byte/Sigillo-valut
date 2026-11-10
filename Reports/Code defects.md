---
tags: [report, defects]
source: code review of 2026-11-10 (code not changed)
---
# Code defects found in the code review (2026-11-10)

**In breve (IT):** nessun difetto grave di sicurezza o di crittografia. Sono soprattutto commenti vecchi, numeri di versione fissi, testi legali del sito non allineati al codice e due piccole mancanze nei comandi. Il codice **non è stato toccato**: decide Giovanni cosa correggere.

## Legal / site texts (most visible to customers)
1. `apps/server/src/http/site-legal.ts`: Terms say the database is copied "every two hours"; the server cron is hourly and keeps 2 copies.
2. `site-legal.ts`, `site-privacy.ts`: Terms, DPA and privacy name only FreeTSA; production also uses DigiCert (`TSA_URL_2`) — a sub-processor missing from the DPA.
3. The site says "open source" but the repository has **no LICENSE file** (only `sdk-python` declares Apache-2.0).
4. Standard plan (100,000 receipts) and billing are advertised but not built; the code has one 10,000/month quota for every organization.

## Behaviour
5. `sigillo-server checkpoint` (CLI) ignores `TSA_URL_2`; only the running server asks the second authority.
6. `sigillo-server export` requires `--signer-socket` although exporting signs nothing.
7. Verifier `doc` and `open` subcommands have no `--key-id` option, so they cannot pin the trusted key.
8. `deploy/Caddyfile` CSP `img-src 'self'` blocks the `data:` SVG grain image used by the site style.
9. Local model upstream port is reachable without authentication (known audit item; decision pending: random port + alarm).

## Stale comments and version strings
10. Version hard-coded `"0.1.0"` in verifier `cli.ts:157`, signer CLI, server CLI and `service.version` (not taken from `SIGILLO_VERSION`).
11. Verifier `cli.ts:37` help says "every artifact a v2 receipt names" (v2–v4 apply).
12. `storage/schema.ts:156-157` heartbeat comment sits above `export_register` instead of `connections`.
13. `store.ts` reconcile comment says "one receipt ahead"; recovery copies N receipts from the signer journal.
14. `auth/api-keys.ts` and `schema.ts` say "only the scrypt hash" and "one API key per system"; since 2026-11-07 new keys use SHA-256 and a system can have several keys.
15. `assurance/privacy-check.ts` lines ~97 and ~234 say "scrypt" / "scrypt salt", so the generated `docs/STORAGE-INVENTORY.md` is stale too (test-coupled: change both together).
16. `http/server.ts:478` says "the whole batch in one transaction"; each receipt is committed in its own transaction.
17. `gateway/llm.ts` header says GET is not recorded; code records every GET except model lists.
18. `http/ui.ts` `themeOf` comment says light, code returns dark.
19. Verifier `timestamps.ts` ~277: a token demoted from failed to imprint-only carries no genTime (cosmetic).
20. `sdk-python/src/sigillo/__init__.py` docstring: event name `sigillo.local_model.open` (real: `sigillo.connection.model_open`), "64-character limit" for `on_behalf_of` (real: 256), link to missing `docs/PROPOSTA-FASE-4.md`. Same old event name in `apps/server/test/security-audit.test.ts:82`.
21. `DEPLOY.md` links to missing `docs/RAPPORTO-VERIFICA-2026-10-01.md`.

Docs that disagreed with the code were fixed in PR #149 (docs only). See [[Status and open items]].
