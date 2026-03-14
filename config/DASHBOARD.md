# 📋 DASHBOARD — Task Tracker

*Last updated: 2026-03-14 07:55 SGT*

---

## 🔴 ACTIVE (doing now)
- **Momo Spot-Check Fixes** — Fixing F-grade items: DASHBOARD staleness (this update), backup cleanup ✅, message noise reduction. *(2026-03-14)*
- **OpenClaw Version Re-Update** — Regressed from 2026.3.12 → 2026.2.17 during WhatsApp nuclear fix (Mar 13). Need to re-update. *(2026-03-13)*

---

## 🟡 IN PROGRESS (halfway done)
- **Message Noise Reduction** — Momo spot-check: ~40% noise vs 15% target. Working on behavioral changes + silent work patterns. *(2026-03-14)*
- **Multi-Timeframe P&L Baselines** — 7d/15d/31d from API working. Own data: 2 days recorded, 88 more for 90d historical. Auto-recording daily via daily-funding-report cron. *(2026-03-13)*
- **Verification Hygiene** — Momo spot-check: good on code, poor on operational claims. Need to verify-before-claiming pattern. *(2026-03-14)*

---

## 👁️ OBSERVING (live, monitoring for issues)
- **Cron Health (20 enabled)** — 16 healthy, 4 with recent errors (deep-self-review, defidojo-daily-channels, defidojo-night, variational-funding-rates). *(since 2026-02-08)*
- **Aave Health Monitoring** — 3 wallets. Every 15 min 6am-11pm. ETH 3-3 HF at 1.35 🟠. *(since 2026-02-06)*
- **BTC Wrapped Peg** — WBTC/cbBTC monitoring. Alert on depeg >0.5%. *(since 2026-02-06)*
- **Mayer Multiple** — In morning briefing. Current: 0.75 (Deep Value 🟢🟢). *(since 2026-02-18)*
- **Wallet Spy** — 12 wallets across ETH+ARB+HL. 1x daily (1:15pm). *(since 2026-02-08)*
- **DeFi Dojo Discord** — 3 channels: #dojo-chat (3x day + hourly overnight), #hedge-arb + #hl-and-frenemies (1x daily 4:20pm), #security-alerts (every 15min). *(since 2026-02-15)*
- **Discord Sentiment (Paradex + Variational)** — Standalone cron 5x/day. *(since 2026-03-10)*
- **Perp Collateral Health** — Every 15 min, all venues. Script-driven alerts. *(since 2026-03-01)*
- **Funding Rate Alerts** — Hourly state transition detection (1d blended flip). *(since 2026-03-01)*
- **Per-Instrument Regime Tracker** — BTC/ETH regime streaks with 90-day HL backfill. *(since 2026-03-10)*
- **Lenovo ThinkPad P16 Price** — Daily 10:30am. Baseline SG$3,385 (38% off). *(since 2026-03-10)*

---

## 🔵 KIV (discussed, parked for later)
- **App Building Project** — Brainstormed 8 ideas. Top: Funding Rate Dashboard (80/100). No follow-up.
- **ETH Activation** — On watchlist: activate when funding >5% APR 14d avg across 2+ venues AND Aave >2%.

---

## 🟠 BLOCKED ON BRO
- **GitHub PAT Expired** — Dashboard config sync to damontay043.github.io failing. Need fresh PAT with repo scope. *(since ~Mar 12)*
- **OpenClaw Re-Update** — Regressed to 2026.2.17 during WhatsApp fix. Need bro's go-ahead to re-update to latest. *(2026-03-13)*
- **ClawdStrike Security Fixes** — 3 items from Feb 20 audit still pending: mDNS broadcasting, no ufw firewall, plaintext secrets. *(2026-02-20)*
- **Twitter @realdamontay Appeal** — Account suspended. Appeal submitted. Waiting on X. *(2026-02-13)*
- **Aboutme Enrichment Questions** — Travel, origin story, content consumption still awaiting responses.

---

## ⏸️ PAUSED
- **Paradex Liquidity Monitor** — Disabled. Bro withdrew funds from Paradex (Feb 16). Re-enable when funds return.
- **Twitter Auto-Like** — Disabled. Account suspended (Feb 13).
- **Morning Wellness Deep Analysis** — Cron disabled. *(since ~Mar 3 pitstop)*
- **Aboutme Enrichment Cron** — Disabled. *(since ~Mar 3 pitstop)*
- **Daily Calibration Interview** — Disabled. *(since ~Mar 3 pitstop)*
- **Several Others** — afternoon-research, uv-index-and-stories, daily-business-idea, evening-funding-briefing, trello-q1-helper, nag-headline-inputs all disabled since Mar 3 pitstop.

---

## 🟤 ABANDONED
*Nothing currently*

---

## ✅ RECENTLY COMPLETED
- **260303 Backup Cleanup** — Moved 6 files (264KB) to archive-backups/. *(2026-03-14)*
- **Discord Stale DOM Fix** — F5 reload → wait 8s → End → extract. Applied to ALL 5 Discord crons. *(2026-03-13)*
- **Pulse Pre-Compute** — pulse-gather.sh parallelizes data fetch. Pulse 430s→130s. *(2026-03-10)*
- **Morning Briefing Pre-Compute** — morning-briefing-gather.sh parallelizes 14 sources (~12.5s). *(2026-03-11)*
- **Discord Sentiment Standalone Cron** — Carved out from pulse. 5x/day with screenshots. *(2026-03-10)*
- **PAXG Full Exit** — Removed from ALL funding analysis. Gold MACRO data stays. *(2026-03-11)*
- **Macro Catalyst Section** — Added permanently to morning-briefing, pulse, daily-funding. *(2026-03-11)*
- **Discord Scan State Tracking** — Unified JSON for "NEW MESSAGES ONLY" filtering. *(2026-03-11)*
- **WhatsApp Nuclear Fix** — 9-hour outage (Mar 13 15:00→00:00). Fresh QR rescan. *(2026-03-14)*
- **Nightly Memory Consolidation** — Automated cron at 1:50am SGT. *(2026-03-07)*
- **relay-qa.sh** — QA script for cron report relay. *(2026-03-02)*
- **PAXG Full Exit** — Exited Feb 24. Redistribution discussions closed. *(2026-02-24)*

---

## ⏰ CRON SCHEDULE (20 enabled)
| Cron | Schedule | What |
|------|----------|------|
| **aave-health-alert** | :03,:18,:33,:48 6-23 | Health factor, 3 wallets |
| **perp-collateral-alert** | :08,:23,:38,:53 6-23 | Perp collateral deficits |
| **security-alerts-monitor** | */15 6-23 | DeFi Dojo #security-alerts |
| **funding-rate-alert** | :00 6-23 | 1d blended state transitions |
| **defidojo-day** | :20 6,14,21 | #dojo-chat daytime |
| **morning-briefing** | 7:00 | Full morning report |
| **discord-sentiment** | :25 7,11,15,18,20 | Paradex + Variational sentiment |
| **variational-funding-rates** | :40 7,15 | Majors + extreme outliers |
| **hourly-pulse** | :45 6,10,14,18,20 | Full market dashboard |
| **deep-self-review** | :00 0,9,12,15,18,21 | Config compliance + MISS |
| **refresh-rules-nudge** | :00 10,14,18,22 | Rules refresh (main session) |
| **lenovo-price-monitor** | 10:30 | ThinkPad P16 price check |
| **wallet-spy** | 13:15 | 12-wallet forensic scan |
| **hl-oi-top10** | 14:00 | Hyperliquid OI top 10 |
| **daily-funding-report** | 14:30 | Strategy + P&L analysis |
| **nag-undone-tasks** | 15:00 | Mega nag (blocked tasks) |
| **defidojo-daily-channels** | 16:20 | #hedge-arb + #hl-and-frenemies |
| **defidojo-night** | :20 22-5 | Overnight #dojo-chat |
| **nightly-memory-consolidation** | 1:50 | Memory + bottleneck review |

---

## 🧭 CORE OPERATING PRINCIPLES
1. **Process > Memory** — Context gets compacted. Processes survive. If it's not written down, it doesn't exist.
2. **Dashboard = Single Source of Truth** — Any new process gets added HERE immediately.
3. **Text > Brain** — Write it to a file. Mental notes don't survive compaction.
4. **Don't Lose Jingzhou** — Check what exists before building new. Coordinate with Momo.
5. **Backup Before Big Changes** — `cp file file.YYMMDD.bak` before risky edits.
6. **Never use everyMs** — Always cron expressions. Delete + recreate if stuck.
7. **Post-restart cron verification** — MANDATORY within 2 mins of any gateway restart.
8. **Fix-one-audit-all** — After fixing any cron issue, check ALL other crons for same pattern.
9. **Verify before claiming** — Never make temporal/operational claims without checking data first.
10. **Silent work** — Work silently, send ONE summary at end. Reduce message noise.
