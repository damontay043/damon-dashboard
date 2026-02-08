# 📋 DASHBOARD — Task Tracker

*Last updated: 2026-02-08 09:15 SGT*

---

## 🔴 ACTIVE (doing now)
- **Audit Backlog Cleanup** — Clearing 4 items from Momo's Feb 7 audit. USER.md ✅, Tier 1 files ✅, MEMORY.md ✅, DASHBOARD.md (this). *(2026-02-08)*

---

## 🟡 IN PROGRESS (halfway done)
- **System Cron Decommission** — 24h parallel run of Linux system cron alongside fixed OpenClaw crons. Remove via `crontab -r` on Feb 9 ~08:00 if all crons still green. *(2026-02-07)*

---

## 👁️ OBSERVING (live, monitoring for issues)
- **Cron Health Post-Fix** — OpenClaw 2026.2.6-3 fixed cron scheduler bug. All 17 crons green. Monitoring for regression. *(since 2026-02-08)*
- **Aave Health Monitoring** — 2 wallets (~$6.7M total debt). Alert thresholds: ⚠️ <1.39, 🚨 <1.3. ETH wallet HF ~3.7, ARB wallet HF ~1.75. *(since 2026-02-06)*
- **BTC Wrapped Peg** — WBTC/cbBTC peg tracking via Paraswap + Binance. Alert on depeg >0.5%. *(since 2026-02-06)*
- **Paradex Liquidity** — 2-min cron tracking HL + LS on ETH + ARB. *(since 2026-02-06)*
- **Discord Sentiment (Paradex)** — Via Chrome Relay. Currently tab detached — needs bro to reattach. *(since 2026-01-28)*
- **Dashboard Auto-Sync** — Heartbeat pushes changes to GitHub Pages. *(since 2026-01-28)*

---

## 🔵 KIV (discussed, parked for later)
- **Multi-DEX Discord Monitoring** — When bro deploys capital on other perp DEXes, add their Discords to sentiment rotation.
- **App Building Project** — Brainstormed 8 ideas. Top: Funding Rate Dashboard (80/100), Health Newsletter (75/100). No follow-up.

---

## 🟠 BLOCKED ON BRO
- **Chrome Relay Reattach** — Discord tab not connected. Need bro to click OpenClaw extension icon on Discord tab.

---

## 🟤 ABANDONED
*Nothing currently*

---

## ✅ RECENTLY COMPLETED
- **Cron Bug Fix** — OpenClaw 2026.2.6-3 patched the `ensureLoaded` → `recomputeNextRuns` bug. All jobs firing. *(2026-02-08)*
- **Momo Audit Response** — Score 72/100. Identified config staleness, repeat MISSes. Action items in progress. *(2026-02-07)*
- **System Cron Workaround** — Linux crontab for aave-alert (*/2 min) + paradex-liquidity (*/30 min) during bug. *(2026-02-07)*
- **Aave Health Infrastructure** — Scripts + cron for real-time health factor monitoring. *(2026-02-06)*
- **BTC Peg Monitoring** — btc-peg-monitor.js with Paraswap + Binance (not CoinGecko). *(2026-02-06)*
- **Morning Wellness Pipeline** — Garmin + TrainingPeaks PMC + Google Sheets. Fully automated 7:30 AM cron. *(2026-02-04)*
- **DeFi Pulse Script** — 5 metrics from DefiLlama, added to morning briefing. *(2026-02-04)*
- **QMD Memory Backend** — BM25 search via qmd-wrapper.sh. 0.25s, local, zero API cost. *(2026-02-05)*

---

## ⏰ CRON SCHEDULE
| Cron | Schedule | What |
|------|----------|------|
| **paradex-liquidity-monitor** | */2 * * * * | HL + LS on ETH + ARB |
| **aave-health-alert** | 0 * * * * | Health factor check, 2 wallets |
| **hourly-pulse** | 45 * * * * | AQI + BTC Basis + Funding Rates + Aave + Peg + Discord |
| **morning-briefing** | 0 7 * * * | Weather, AQI, News, Market Ratios, DeFi Pulse, Joke |
| **morning-wellness** | 30 7 * * * | Garmin + TrainingPeaks + Sheets wellness analysis |
| **afternoon-research** | 0 15 * * * | Deep-dive research report on rotating topic |
| **uv-index-and-stories** | 0 17 * * * | UV Index + curated stories |
| **evening-funding** | 0 21 * * * | Detailed funding rate report |
| **nag-undone-tasks** | Every 3h | Reminds of blocked/pending items |
| **deep-self-review** | Active hours | Config compliance + MISS review (Opus) |
| **aboutme-enrichment** | 0 16 * * * | Daily interview question for aboutme.md |

---

## 🧭 CORE OPERATING PRINCIPLES
1. **Process > Memory** — Context gets compacted. Processes survive. If it's not written down, it doesn't exist.
2. **Dashboard = Single Source of Truth** — Any new process gets added HERE immediately.
3. **Text > Brain** — Write it to a file. Mental notes don't survive compaction.
4. **Don't Lose Jingzhou** — Check what exists before building new. Coordinate with Momo.
5. **Backup Before Big Changes** — `cp file file.YYMMDD.bak` before risky edits.
6. **Never use everyMs** — Always cron expressions. Delete + recreate if stuck.
7. **Post-restart cron verification** — MANDATORY within 2 mins of any gateway restart.
