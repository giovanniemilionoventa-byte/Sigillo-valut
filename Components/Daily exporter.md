---
tags: [component, export]
source: code (apps/server/src/backup/daily-export.ts)
---
# Daily exporter

Per account, opt-in (`daily-export.json` in the backup dir). At **23:59 Italian time** writes one [[Evidence pack]] per system for the day under `<backup>/exports/<owner>/`, keeps **30 days**.
