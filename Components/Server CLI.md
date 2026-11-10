---
tags: [component, operations]
source: code (apps/server/src/cli.ts)
---
# Server CLI (`sigillo-server`)

| command | does |
|---|---|
| `serve` | run the server |
| `system create|list|rename|archive|unarchive|delete|assign` | manage systems (delete only if empty) |
| `org create|approve|members|list|erase` | organizations; `erase` deletes everything of one customer |
| `admin-code [--off]` | operator second factor |
| `admin-log` | read the admin log |
| `key create|revoke|list` | API keys |
| `signer check` | reconcile with the [[Signer]] |
| `migrate` | upgrade DB schema |
| `backup --out` | consistent SQLite copy |
| `checkpoint` | one [[Checkpointer]] run |
| `export <system> --out [--subject] [--open]` | [[Evidence pack]] |
| `subject find|erase`, `openings erase` | GDPR tools ([[Flow - erasure]]) |
| `privacy inventory|scan` | what is stored; search a phrase in DB, logs, backups ([[Data protection]]) |
