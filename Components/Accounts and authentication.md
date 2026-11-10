---
tags: [component, security]
source: code (apps/server/src/auth/*, http/accounts.ts)
---
# Accounts and authentication

## Who
- **Operator** (Giovanni, admin console `/ui/admin`): password `SIGILLO_ADMIN_PASSWORD(_FILE)` ≥ 12 chars (no password → no web console). Optional second factor TOTP RFC 6238 (`sigillo-server admin-code`, file `admin-code.key`).
- **Organizations** (customers): sign up with Firebase Authentication (checked server-side against Google certificates), approved by the operator (`org approve`). Members in `users`.
- Tenancy: `storeFor()` gives each viewer a proxy that only allows its own systems.

## Sessions
HMAC-signed cookie `<expiry>.<who>.<mac>`, 12 h; the secret is in memory (restart logs everyone out); per-viewer sign-out counter.

## API keys
`sigillo_<16 hex id>_<64 hex secret>`; stored as SHA-256("sigillo-api-key:" + secret) (older scrypt hashes migrated on first use). Several keys per system (`SIGILLO_EXTRA_KEYS`); `key create|revoke|list`.

Limits: [[Limits and throttling]]. Related: [[Security model]].
