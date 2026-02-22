# NOW.md — Current Task

DOING: Wire net-pnl into remaining crons (hourly-pulse + evening-funding-briefing)

## Just Completed (last 3 items)
- ✅ /api/net-pnl tested + confirmed working with full per-asset data
- ✅ daily-funding-analysis cron updated to use net-pnl as PRIMARY source
- ✅ perp-collateral-alert timeout fixed 30s → 60s (confirmed working)

## Blocked On
- [x] ~~Verify hourly-pulse fix works at 10:45am SGT run~~ — CONFIRMED working ✅
- [ ] Google auth expired — blocking calendar + wellness

## Context
- TRUE Net P&L: -$1,074/day (-$392K/yr). PAXG worst at -$692/day
- ETH only profitable asset: +$67/day
- Dashboard banner live at perp-dashboard.pages.dev
- OpenClaw 2026.2.21-2 available (current 2026.2.17)
- All cron message sends need explicit `to=+6596926916`
