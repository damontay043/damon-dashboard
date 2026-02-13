# NOW.md — Current Task

DOING: idle (post-compaction flush)

## Just Completed (last 3 items)
- ✅ Cron delivery audit — patched 6 crons with explicit message delivery (fix-one-audit-all)
- ✅ Run EF direction fix — HIGHER = better, updated wellness cron + logged MISS
- ✅ Wallet spy fix — each wallet = different operator, no aggregated P&L
- ✅ DeFi Pulse format — show all 3 timeframes (7d/30d/90d)

## Blocked On
- [ ] Google auth re-auth — bro was working on it ~06:45, waiting for auth code paste
- [ ] Headlines source config — bro hasn't chosen preferences
- [ ] Twitter cookie refresh — bro hasn't done it
- [ ] Damon audit with Momo — not rescheduled after missing Feb 12

## Context
- OpenClaw 2026.2.9, model opus-4-6, thinking high
- 19+ crons active, all firing normally (morning-wellness consecutiveErrors reset after fix)
- Hourly pulse running near 300s timeout edge — may need optimization
- Scarlet2023 node offline (known, not new)
- BTC ~$66.4k, funding rates positive ~10% APR across venues
- Lighter funding notably low (2.63% vs peers ~10%)
- Key cron IDs: hourly-pulse `91f41b4b`, hl-oi-top10 `0747f05d`, wellness `158b3650`
- Dashboard last synced: commit `0d755de` (Feb 13 08:07)
