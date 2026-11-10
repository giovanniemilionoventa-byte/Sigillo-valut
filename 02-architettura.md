# Architettura

Repo del codice: monorepo pnpm, Node 22, TypeScript. [CODICE]

| cartella | cosa fa |
|---|---|
| `packages/core` | libreria pura: schema, forma canonica, hash, catena, firma, Merkle, checkpoint, export. Vietati rete, disco, orologio, variabili d'ambiente |
| `packages/verifier` | `sigillo-verify`, dipende solo da core più `openssl` |
| `packages/sdk-typescript` | SDK Node/TS, zero dipendenze |
| `apps/signer` | processo separato con la chiave privata Ed25519 |
| `apps/keyholder` | processo separato con la chiave che apre le chiavi dei modelli dei clienti |
| `apps/server` | ingresso dati, archivio SQLite, checkpoint, marche temporali, export, console, sito, gateway |
| `sdk-python` | SDK Python 0.12.0, fuori dal workspace pnpm |
| `deploy` | Dockerfile, compose, Caddyfile, script di aggiornamento e backup |
| `docs`, `SPEC.md`, `PROGRESS.md`, `CLAUDE.md` | documentazione; `docs/SIGILLO-COMPLETO.md` è la panoramica lunga (italiano) |

Tecnologie: Fastify, better-sqlite3, zod, protobufjs, pdfkit, canonicalize, commander. Crittografia solo `node:crypto`.

## Produzione: quattro contenitori

- `signer`: chiave di firma, **nessuna rete**, parla via socket Unix.
- `keyholder`: chiave RSA-3072 che apre le chiavi dei modelli (formato `box1`, AES-256-GCM più RSA-OAEP).
- `server`: tutto il resto, porta 8080 verso Caddy.
- `caddy`: HTTPS, intestazioni di sicurezza.

Tutti in sola lettura, senza privilegi. Il signer firma **solo la ricevuta successiva** a quella che conosce, tenendo un proprio stato per ogni catena: per questo un server compromesso non riscrive il passato. Una sola chiave di firma per tutte le organizzazioni, niente HSM.

## Server di produzione [DOCUMENTI]

VPS Aruba Cloud (40 GB, 4 GB RAM), repo in `/root/sigillo/deploy`. Aggiornare con `./update.sh`. **Mai `docker compose down -v`** (cancella i volumi, chiave compresa). Backup locale ogni ora (2 copie). Backup fuori sede ogni giorno alle 03:30, cifrato con `age`, su Google Drive, 30 copie; la chiave privata `age` ce l'ha solo Giovanni. Mai provato un ripristino vero.

Cosa gira davvero sul server vivo non è verificabile dal cloud: [NON VERIFICATO].
