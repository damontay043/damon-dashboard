# NOW.md - Current Task

DONE: Discord scroll experiments — **SOLVED!** ✅

## Solution Found
- **"Jump to Present" button WORKS** (click it when viewing older messages)
- **"Jump to last unread message" button DOESN'T WORK** (click doesn't trigger)
- Updated cron to use correct button
- Now getting <15 min staleness vs 60+ min before

## Cron Sleep Schedule (locked in)
- **30-min Pulse:** Runs 06:00-22:30 SGT only (`*/30 6-22 * * *`)
- Sleeps 23:00-06:00 SGT to save tokens

## Control Panel
- Built and ready at `/root/clawd/apps/control-panel/`
- Run: `npm start` → http://localhost:3333

## Migration Tomorrow
- Backup tarball ready: `/root/clawd-backup.tar.gz` (119MB)
- Momo's plan: 9 phases, ~90 min, ~5 min downtime

---
Last updated: 2026-01-31 04:45 SGT
