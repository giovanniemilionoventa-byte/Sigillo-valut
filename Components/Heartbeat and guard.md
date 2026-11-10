---
tags: [component, completeness]
source: code (apps/server/src/connection/watch.ts; sdk-python _watchdog.py; sdk-typescript guard.ts)
---
# Heartbeat and guard

Answers "was the recorder on?". The SDK opens a session (32 hex) and posts to `POST /api/v1/heartbeat`.

- Events: `start`, `beat` (every 60 s), `stop`, `ended`, `ping`, `gap`, `alarm`.
- A separate **guard process** (Python: `_watchdog.py`, run with its own interpreter; TypeScript: `guard.ts`) notices if the agent dies, the machine sleeps or goes offline, and reports `ended` / `gap`.
- Server marks a session **lost** after 3 min of silence (sweep every 30 s).
- Receipts (actor `sigillo`, kind `agent_step`) only on change: `sigillo.connection.start|stop|lost|ended|restored|script_changed|sleep|offline|power_off|model_open|model_foreign`.
- **Script guard** (`SIGILLO_SCRIPT_GUARD`): SDK sends a hash of the main script and of itself; a change → `script_changed`; in `strict` mode the SDK stops the agent (exit 70) on a change or after 3 missed beats.
- **Alarms** from the local-model door: `model_open` (local model port reachable without Sigillo), `model_foreign`.
- Limits: 60 new sessions/hour/system, 24 gaps/hour/session.

Related: [[Python SDK]], [[Chain health]].
