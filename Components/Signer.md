---
tags: [component, security]
source: code (apps/signer/src)
---
# Signer (`sigillo-signer`)

Separate process that holds the **private key** and its **own record of every chain**. In Docker it runs with **no network** (`network_mode: none`) and talks to the [[Server]] only over a Unix socket.

## Commands
- `keygen --key <file>`: new Ed25519 key (PKCS8 PEM, 0600).
- `serve --key --socket --state [--clock-tolerance-seconds 300]` (env `SIGILLO_SIGNER_CLOCK_TOLERANCE_SECONDS`).
- `init-from-db --db --state`: rebuild its state from an existing database (once; a second run writes nothing).

## Protocol (JSON lines, version 2, max 256 KiB per line, 16 connections, socket mode 660)
| request | does |
|---|---|
| `PUBKEY` | public key and key id |
| `SIGN_RECEIPT` | checks and signs one [[Receipt]] |
| `CHECKPOINT` | builds and signs a [[Checkpoint]] from its own frontier and clock |
| `GET_HEAD` | head of a chain |
| `GET_RECEIPTS` | up to 50 receipts from its journal (recovery) |
Refusal codes: `malformed`, `version`, `invalid`, `sequence`, `clock`, `unknown_system`.

## Rules it enforces
key id matches; genesis only at seq 0; seq = head+1 and prev_hash = head hash; `ts_received` within ±5 min of its clock and not earlier than the previous; a retired system never gets a new chain.

## State
Per system, file `<sha256(system_id)>.json` (seq, hash, head, Merkle frontier), written atomically (tmp + fsync + rename), plus a journal `<sha>.receipts.jsonl` appended **before** the state. A fatal write error stops it (exit 1) instead of signing blind.

At server start, `reconcileWithSigner` compares heads: in sync, recovered (copies missing receipts from the journal), or diverged (logged as `signer.divergence`, chain goes red). `sigillo-server signer check` does it by hand.

Related: [[Hash chain]], [[Signatures and keys]], [[Deployment]].
