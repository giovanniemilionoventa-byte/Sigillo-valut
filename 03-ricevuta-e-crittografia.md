# Ricevuta e crittografia

[CODICE] Dettagli completi: `docs/FORMAT.md` nella repo del codice.

## Ricevuta (schema attuale v4)

Campi: `v`, `system_id`, `seq`, `ts_event` (non fidata), `ts_received`, `actor {agent, on_behalf_of}`, `action {kind, name}` con kind `tool_call | llm_call | agent_step | decision | genesis`, `input_hash`, `output_hash`, `outcome` (`ok|error|blocked|unknown`), `source {type: otlp|sdk|api, trace_id, span_id}`, `prev_hash`, `key_id`, `sig`. Dalla v2 `artifacts` e `model`; dalla v3 il testo `sigillo-text/1`; dalla v4 `input_hash_scheme` e `output_hash_scheme` (`plain|salted`).

Regola: non cambiare il formato senza aumentare `v`, e le versioni vecchie restano verificabili.

## Firma e catena

1. Forma canonica JSON (RFC 8785). 2. SHA-256. 3. Firma Ed25519 sui 32 byte dell'impronta. La catena parte da `genesis` con `seq` 0 e `prev_hash` di 64 zeri.

## Privacy nel formato (v4, 2026-10-01)

- `on_behalf_of` è uno pseudonimo `psn_` più 32 esadecimali.
- Impronta salata: SHA-256(nonce di 32 byte e forma canonica). Con la **chiave del cliente** il nonce è un HMAC e la chiave resta dal cliente.
- Le ricevute v1-v3 possono contenere nomi in chiaro (160 nel DB di produzione al 2026-10-02, tutte di prova).

## Checkpoint e marche

Checkpoint `{v, system_id, tree_size, root_hash, ts, key_id, sig}`, albero di Merkle RFC 6962. Marca RFC 3161 sulla radice con due enti: FreeTSA e DigiCert. Nessuno qualificato eIDAS.

## Pacchetto di prove e verificatore

Zip deterministico: `receipts.jsonl`, `checkpoints.jsonl`, `artifacts-index.jsonl`, `timestamps/*.tsr`, `manifest.json`, più `report.pdf` e `VERIFY.md`. Il verificatore `sigillo-verify` fa 14 controlli (manifest, sequenza, genesis, prev_hash, firme, Merkle, prove di inclusione, marche via openssl, tempi di ancoraggio). Opzioni: `--key-id`, `--previous`, `--strict`, `--tsa-ca`. Si scarica dal sito e dalle release GitHub con SHA-256. 18 scenari di manomissione provati nei test. Dimensione 1792 righe, accettata dal proprietario il 2026-09-21.
