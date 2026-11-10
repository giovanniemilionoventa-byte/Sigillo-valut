---
tags: [operations]
source: code (deploy/*.sh)
---
# Backups and updates

| script | does |
|---|---|
| `backup.sh` | SQLite online backup, keeps 2 copies, refuses if free space < 2× DB (cron hourly on the server [docs]) |
| `offsite-backup.sh` | encrypts with `age`, copies with `rclone` (Google Drive chosen 2026-11-09), keeps 30, alarm if newest older than 6 h |
| `attiva-backup-fuori-sede.sh` | guided setup of the off-site copy (sftp, s3, drive) |
| `update.sh` | `git pull`, build, `up`, restart Caddy, check the CSP hash, `signer check` |
| `auto-update.sh` | cron every minute, follows `main` only |

The [[Daily exporter]] also writes evidence packs into the backup folder. After each merge on `main`, Giovanni restarts the stack (or auto-update does) [docs].

Open points (only Giovanni): test a restore, off-site copies that cannot be deleted, stop auto-update as root. See [[Status and open items]].
