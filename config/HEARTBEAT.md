# HEARTBEAT.md - Periodic Check-in Tasks (Slimmed Down)

**Most "always report" checks moved to the 30-min Pulse cron for consistency.**

When you wake up on a heartbeat, run through this checklist. Only message bro if something is worth flagging. Otherwise reply HEARTBEAT_OK.

## Active Checks (conditional/silent)

0. **🚨 REPORT RELAY CHECK (RUN FIRST — highest priority)**
   Scan `/tmp/cron-reports/` for unreleyed review-mode cron reports.
   - Read `memory/report-relay-state.json` for last relay timestamps
   - Run: `stat -c '%n %Y' /tmp/cron-reports/*.md 2>/dev/null` to get file modification times
   - For each file: if file mtime > last relay timestamp (or null), the report is NEW and needs relaying
   - For each new report, use TWO-MESSAGE FORMAT:
     1. Read the full report file
     2. **Message 1:** Send the FULL original report (adapted for WhatsApp — no markdown tables, use bullets). Include ALL data, ALL timeframes, ALL sections. If report contains `SCREENSHOT:` path, attach via `filePath` parameter.
     3. **Message 2:** Send separate "DAMON'S TAKE" with contextual analysis, connections to recent conversations, QA corrections. This is ADDITIVE — don't repeat data from Message 1.
     4. Update `memory/report-relay-state.json` with current timestamp
   - This catches any reports that `delivery: announce` failed to push to the main session
   - **WHY THIS EXISTS:** On Mar 1 2026, switching to `delivery: none` caused 2:30/2:45 PM reports to silently sit in files for 2+ hours. This scanner is the safety net.

1. **Crypto Movers** — Fetch top 20 coins from CoinGecko:
   `https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=20&page=1&price_change_percentage=24h`
   - ONLY alert if any coin moved **±8% in 24h**
   - Exclude stablecoins (USDT, USDC)
   - If none moved ±8%, stay silent on this check
   - If API rate-limited or fails, skip quietly
   - Format (only if triggered):
     ```
     🚨 *Crypto Alert*
     • [COIN]: $[price] ([+/-X.X]% 24h)
     ```

2. **Dashboard + Config Sync** — Check if any core files changed since last GitHub push.
   - Files to sync: `DASHBOARD.md`, `AGENTS.md`, `SOUL.md`, `USER.md`, `IDENTITY.md`, `TOOLS.md`, `HEARTBEAT.md`, `NOW.md`
   - Update `/tmp/damon-dashboard/config/LAST_CHECKED.txt` with current SGT timestamp (at least daily or when syncing changes) so config.html shows last check time
   - If changed: copy to `/tmp/damon-dashboard/config/`, regenerate HTML if needed, git commit + push
   - Track in `memory/dashboard-state.json`
   - Silent — don't message bro unless push fails

3. **Node Health + Full Config Sync** — Check if Scarlet2023 node is connected.
   - Only mention if it JUST went offline
   - Don't spam about known offline status
   - **If online:** Sync ALL core files to `/mnt/c/Users/pujing/OneDrive/clawdbot-shared/vps-config/`:
     - AGENTS.md, SOUL.md, USER.md, IDENTITY.md
     - TOOLS.md, HEARTBEAT.md, NOW.md, DASHBOARD.md
     - MEMORY.md (private, not on public GitHub)
   - This gives bro a private local copy of everything

4. **Memory Maintenance** — ~~Every 3 days via heartbeat~~ **NOW AUTOMATED via nightly-memory-consolidation cron (1:50am SGT daily)**
   - Cron reviews daily notes + session history, updates MEMORY.md/TOOLS.md/USER.md
   - Also runs bottleneck scan (flags corrections bro made → automation candidates)
   - Heartbeat no longer needs to do memory maintenance — just verify the cron ran:
     - Check `cron action=list` for `nightly-memory-consolidation` lastStatus
     - If last run failed, do a manual consolidation pass

5. **Background Task Verification** — Check `memory/background-tasks.json` for any tasks with `status: "in_progress"`.
   - Verify each task using its `verifyCommand`
   - If task exceeded `expectedDurationMin` without completing → retry (up to `maxRetries`)
   - If completed → update status, run `onComplete` action, remove from list
   - If failed and max retries hit → alert bro
   - Silent unless action needed

6. ~~Self-Review~~ — **MOVED TO DEDICATED CRON** (Opus, every 3hrs)

7. **Bootstrap File Size Monitor** — Once daily (first heartbeat after 6am SGT), check bootstrap file sizes:
   - Run: `wc -c AGENTS.md TOOLS.md MEMORY.md /mnt/c/Users/pujing/OneDrive/clawdbot-shared/aboutme.md`
   - If ANY file > 15,000 chars: alert bro:
     ```
     ⚠️ Bootstrap file approaching limit:
     - [FILE]: Xk chars (limit: 20k)
     Action: graduation review needed — move older content to reference files
     ```
   - If all files < 15,000 chars: silent pass
   - Track last check in `memory/heartbeat-state.json` (`lastBootstrapSizeCheck`)

## When to message bro

- Crypto mover ±8% detected
- Dashboard push failed
- Node just went offline
- Something genuinely urgent

## When to stay quiet (HEARTBEAT_OK)

- Late night (23:00-08:00 SGT) unless urgent
- No conditional alerts triggered
- Bro is in active conversation

## Moved to Crons (don't duplicate!)

These are now handled by dedicated cron jobs with explicit formats:

| Check | Cron | Schedule |
|-------|------|----------|
| Aave Health (3 wallets) | aave-health-alert | Every 2 min (6am-11pm) |
| AQI + BTC/PAXG Basis + Funding + Aave + Peg + FUD + Gold Spread + Discord (Paradex+Variational) | hourly-pulse | 5x/day (06:45, 10:45, 14:45, 18:45, 20:45 SGT) |
| Weather + Headlines + Mayer Multiple | morning-briefing | 7am SGT |
| Wallet Spy (12 wallets) | wallet-spy | 7am/1pm/7pm SGT |
| Variational Funding Snapshot | variational-funding-rates | 7:40am/3:40pm SGT |
| DeFi Dojo #dojo-chat (daytime) | defidojo-day | 5x/day (06:20–21:20 SGT) |
| DeFi Dojo #hedge-arb + #hl-and-frenemies | defidojo-daily-channels | 4:20pm SGT |
| DeFi Dojo #dojo-chat (overnight) | defidojo-night | Hourly 10pm-5am SGT |
| Trello Q1 Tasks | trello-q1-helper | 1pm SGT |
| HL OI Top 10 | hl-oi-top10 | 2pm SGT |
| Research Report | afternoon-research | 3pm SGT |
| Aboutme Enrichment | aboutme-enrichment | 4pm SGT |
| UV Index + Stories | uv-index-and-stories | 5pm SGT |
| Business Idea | daily-business-idea | 6pm SGT |
| Wellness Deep Analysis | morning-wellness-deep-analysis | 6:30pm SGT |
| Calibration Interview | daily-calibration-interview | 7pm SGT |
| Detailed Funding Report | evening-funding-briefing | 9pm SGT |
| Config Compliance + MISS Review | deep-self-review | 6x/day (12am, 9am-9pm every 3h) |
| Blocked Tasks Nag | nag-undone-tasks | 5x/day (9am-9pm every 3h) |
| Headline Source Nag | nag-headline-inputs | 5x/day (9am-9pm every 3h) |
| Rules Refresh | refresh-rules-nudge | 4x/day (10am, 2pm, 6pm, 10pm) |

## Discord Monitoring (updated 2026-01-31)

**Method:** OpenClaw Playwright-based browser automation (simpler than old relay approach)

**Commands:**
```bash
# Scroll to latest, take screenshot
openclaw browser press End && sleep 1 && openclaw browser screenshot

# For more context, scroll up first  
openclaw browser press PageUp && openclaw browser press PageUp && sleep 1 && openclaw browser screenshot
```

**For heartbeat FUD checks:**
1. Take 2 screenshots (latest + scrolled up for context)
2. Analyze images for sentiment/FUD keywords
3. Screenshots saved to `~/.openclaw/media/browser/`

**Alert if:**
- Score <40 or drop >15pts from last check
- Keywords: exploit, hack, rug, drain, compromised

## Track State

Update `memory/heartbeat-state.json` with timestamps of conditional checks.
