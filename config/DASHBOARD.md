# 📋 DASHBOARD — Task Tracker

*Last updated: 2026-02-19 09:10 SGT*

---

## 🔴 ACTIVE (doing now)
- **Config Compliance Fix** — Fixing 4 critical + 3 high + 5 medium issues found in full audit. TOOLS.md, HEARTBEAT.md, DASHBOARD.md, NOW.md, syncs. *(2026-02-19)*

---

## 🟡 IN PROGRESS (halfway done)
- **PAXG Venue Redistribution** — Trio brainstorm consensus: move 150-200 PAXG from Lighter → HL via leapfrog Strategy A. Awaiting bro's go-ahead. *(2026-02-18)*
- **Damon Audit with Momo** — Overdue since Feb 12 (7+ days). Need to schedule. *(2026-02-12)*

---

## 👁️ OBSERVING (live, monitoring for issues)
- **Cron Health (22 active)** — All green, zero consecutive errors. OpenClaw 2026.2.12. *(since 2026-02-08)*
- **Aave Health Monitoring** — 3 wallets: 1-3 (dormant WBTC), 2-3 (120 XAUT, HF ~2.2), 3-3 (ETH+ARB). Every 2 min 6am-11pm. *(since 2026-02-06)*
- **BTC Wrapped Peg** — WBTC/cbBTC via Paraswap + Binance. Alert on depeg >0.5%. *(since 2026-02-06)*
- **Gold Token Spread** — PAXG (Binance) vs XAUT (Bybit). In hourly pulse. Current: +$26 (+0.53%). *(since 2026-02-18)*
- **Mayer Multiple** — In morning briefing. Current: 0.664 (Deep Value 🟢🟢). *(since 2026-02-18)*
- **Wallet Spy** — 12 wallets across ETH+ARB+HL. 3x daily (7am/1pm/7pm). *(since 2026-02-08)*
- **DeFi Dojo Discord** — 3 channels: #dojo-chat (5x day + hourly overnight), #hedge-arb + #hl-and-frenemies (1x daily 4:20pm). *(since 2026-02-15)*
- **Discord Sentiment (Paradex + Variational)** — Dual server monitoring in hourly pulse. *(since 2026-02-08)*
- **Dashboard Auto-Sync** — Heartbeat pushes changes to GitHub Pages. ⚠️ CURRENTLY BROKEN (repo missing). *(since 2026-01-28)*

---

## 🔵 KIV (discussed, parked for later)
- **App Building Project** — Brainstormed 8 ideas. Top: Funding Rate Dashboard (80/100), Health Newsletter (75/100). No follow-up.
- **ETH Activation** — On watchlist: activate when funding >5% APR 14d avg across 2+ venues AND Aave >2%.

---

## 🟠 BLOCKED ON BRO
- **Headlines Source Config** — Which sections from CoinDesk/The Block/CNN/CNBC? Nagged multiple times.
- **OpenClaw Update** — 2026.2.12 → 2026.2.17 available. Alerted 5+ times. No action.
- **PAXG Redistribution Go-Ahead** — Trio consensus reached, need bro to confirm start.
- **Twitter @realdamontay Appeal** — Account suspended. Appeal submitted by bro. Waiting on X.
- **Variational Discord Tab** — Chrome relay tab consistently unresponsive. Need bro to reattach.

---

## ⏸️ PAUSED
- **Paradex Liquidity Monitor** — Cron `05d98969` disabled. Bro withdrew funds from Paradex (Feb 16). Re-enable when funds return.
- **Twitter Auto-Like** — Cron `twitter-auto-like` disabled. Account suspended (Feb 13).

---

## 🟤 ABANDONED
*Nothing currently*

---

## ✅ RECENTLY COMPLETED
- **Config Compliance Audit** — Full audit of all core files. Found BOOTSTRAP.md still existing, DASHBOARD stale, syncs broken, TOOLS.md stale entries. Fixing now. *(2026-02-19)*
- **Gold Token Spread Tracking** — Script + hourly-pulse integration. PAXG (Binance) vs XAUT (Bybit). *(2026-02-18)*
- **Mayer Multiple Tracking** — Script + morning-briefing integration. Binance 200d klines. *(2026-02-18)*
- **Trio Brainstorm: Forward Allocation** — PAXG redistribution consensus. *(2026-02-18)*
- **Wellness Cron Time Shift** — Moved 7:30am → 6:30pm per bro. *(2026-02-18)*
- **NYSE Trading Holidays** — 2026 + 2027 added to Google Calendar. *(2026-02-18)*
- **Day-of-Week Verification Fix** — Mandatory `date -d` in DeFi Dojo crons. *(2026-02-18)*
- **HL OI Top 10 Timeout Fix** — Standalone Node.js script. *(2026-02-17)*
- **EF Root Cause Resolution** — speed × 100 / HR, column confusion identified + fixed. *(2026-02-17)*
- **Systemic Cron Failure Fix** — Gateway restart resolved broken payload delivery. *(2026-02-17)*
- **Confidence Scoring** — 1-100 scale added to all analysis crons. *(2026-02-17)*
- **System Cron Decommission** — Linux crontab removed Feb 9 after 24h parallel run. *(2026-02-09)*

---

## ⏰ CRON SCHEDULE (22 active)
| Cron | Schedule | What |
|------|----------|------|
| **aave-health-alert** | */2 6-23 * * * | Health factor check, 3 wallets |
| **morning-briefing** | 0 7 * * * | Weather, AQI, News, Ratios, Mayer, DeFi Pulse, Joke |
| **wallet-spy** | 0 7,13,19 * * * | 12-wallet forensic monitoring |
| **variational-funding-rates** | 40 7,15 * * * | Variational majors + extreme outliers |
| **hourly-pulse** | 45 6,10,14,18,20 * * * | Full market + health + sentiment dashboard |
| **defidojo-day** | 20 6,10,14,18,21 * * * | DeFi Dojo #dojo-chat daytime |
| **nag-headline-inputs** | 0 9,12,15,18,21 * * * | Nag for headline preferences |
| **nag-undone-tasks** | 0 9,12,15,18,21 * * * | Blocked/forgotten task reminders |
| **deep-self-review** | 0 0,9,12,15,18,21 * * * | Config compliance + MISS review |
| **refresh-rules-nudge** | 0 10,14,18,22 * * * | Rules refresh (main session) |
| **trello-q1-helper** | 0 13 * * * | Trello Q1 tasks |
| **hl-oi-top10** | 0 14 * * * | Hyperliquid OI top 10 |
| **afternoon-research** | 0 15 * * * | Deep-dive / Twitter trends |
| **aboutme-enrichment** | 0 16 * * * | Interview question for aboutme |
| **defidojo-daily-channels** | 20 16 * * * | #hedge-arb + #hl-and-frenemies |
| **uv-index-and-stories** | 0 17 * * * | UV, story, evening focus |
| **daily-business-idea** | 0 18 * * * | Solopreneur idea analysis |
| **morning-wellness** | 30 18 * * * | Garmin + TP + Sheets health report |
| **daily-calibration** | 0 19 * * * | Calibration interview |
| **evening-funding** | 0 21 * * * | Detailed funding rates |
| **defidojo-night** | 20 22-5 * * * | Overnight #dojo-chat (hourly) |

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
