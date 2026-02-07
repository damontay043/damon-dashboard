# NOW.md — Current Task

DOING: idle — system cron workaround in place

## Just Completed (last 1-3 items)
- ✅ Found OpenClaw cron bug root cause (ensureLoaded calls recomputeNextRuns before runDueJobs)
- ✅ Confirmed bug affects other users (Orinks on Discord reported same issue)
- ✅ Set up system cron workaround for critical monitoring

## Blocked On (if any)
- [ ] OpenClaw cron fix (bug introduced 02/04, awaiting patch)

## Context
- OpenClaw 2026.2.3-1 has cron scheduler bug — jobs never execute
- Root cause: `ensureLoaded(forceReload: true)` calls `recomputeNextRuns()` which advances all schedules BEFORE `runDueJobs()` checks them
- **Workaround active:** Linux system cron runs aave-alert (*/2 min) + paradex-liquidity (*/30 min)
- Complex jobs (hourly-pulse, morning-briefing) still broken until OpenClaw fix
- Remove workaround with `crontab -r` when fixed
