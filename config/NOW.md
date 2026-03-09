# NOW.md — Current Task

DOING: Nothing active — monitoring crons + awaiting bro

## Just Completed (last 3 items)
- ✅ Regime duration backtest: 61 days HL data → negative regimes avg 2.7 days (max 4, n=7). Script: scripts/funding/regime-backtest.js
- ✅ Regime header updated in both hourly-pulse + daily-funding crons with real backtested data (replaced "5-15 days" placeholder)
- ✅ Paradex API investigated — working fine, 18:45 timeout was transient. /v1/markets/summary = reliable endpoint (300-400ms, no auth)
- ✅ Sent full methodology writeup to bro for Cody/Momo to reproduce + extend

## Blocked On
- [ ] Chrome relay broken — Discord tab CDP connection failing ("tab not found"). Needs bro to re-click toolbar icon
- [ ] EDMW digest stale — Momo's scout script hasn't run since Mar 7 07:39

## Open Commitments (from commitment-tracker + bro requests)
- REM-005: NOW.md/DASHBOARD.md/MEMORY.md freshness <3 days — NOW.md updated, DASHBOARD.md stale
- REM-006: Stop Reasoning: dumps in replies — needs verification
- REM-007: aboutme.md size guard for enrichment cron — due Mar 14
- REM-008: Wire commitment-tracker into daily cron — due Mar 14
- REGIME-DURATION: ✅ DONE. Bro approved adding real data to headers. Deployed.

## Active Alerts
- 🔔 **Funding spread monitor:** Alert bro if blended funding APR delta between HL-BTC vs Paradex-BTC or Lighter-BTC exceeds 4%. Current: HL-PX ~21% spread on current rates (HL +10.95%, PX -10.18%) — but this is spot rate, not blended.

## Context
- **Net P&L:** -$560/day at 20:45 pulse (improving from -$976 Sat). Day 3 of negative regime.
- **Regime outlook:** HL BTC already flipped positive (+10.95%). Paradex BTC still deeply negative (-10.18%). Historical avg negative regime = 2.7 days — at tail end.
- **Paradex BTC = biggest bleeder:** -$329/day (59% of all funding losses). If PX flips positive, net P&L could swing near breakeven.
- **Chrome relay:** Broken since afternoon. DeFi Dojo + Discord scans failing.
- **Google OAuth:** Fixed (Mar 9). Calendar + Sheets working.
- **Mar 14 spot-check:** 5 days away. Scripts deployed.
- **Bro interactions:** Active evening Mar 9 — regime backtest, header update, Paradex API check
