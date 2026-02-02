# HEARTBEAT.md - Periodic Check-in Tasks (Slimmed Down)

**Most "always report" checks moved to the 30-min Pulse cron for consistency.**

When you wake up on a heartbeat, run through this checklist. Only message bro if something is worth flagging. Otherwise reply HEARTBEAT_OK.

## Active Checks (conditional/silent)

1. **Crypto Movers** — Fetch top 20 coins from CoinGecko:
   `https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=20&page=1&price_change_percentage=24h`
   - ONLY alert if any coin moved **±8% in 24h**
   - Exclude stablecoins (USDT, USDC)
   - If none moved ±8%, stay silent on this check
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

4. **Memory Maintenance** — Every 3 days minimum (check `memory/memory-maintenance.json`)
   - Review daily notes in `memory/YYYY-MM-DD.md` since last maintenance
   - Consolidate important items into MEMORY.md
   - Update the "Last updated" date in MEMORY.md header
   - Track last maintenance date in `memory/memory-maintenance.json`
   - Silent unless MEMORY.md was significantly updated

5. ~~Self-Review~~ — **MOVED TO DEDICATED CRON** (Opus, every 3hrs)

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
| AQI (3 regions) | 30min-pulse | Every 30 min |
| BTC Basis Spread | 30min-pulse | Every 30 min |
| Funding Rates (HL + Paradex) | 30min-pulse | Every 30 min |
| Discord Sentiment | 30min-pulse | Every 30 min (6am-11pm SGT) |
| Weather + Headlines | morning-briefing | 7am SGT |
| Research Report | afternoon-research | 3pm SGT |
| UV Index + Stories | uv-index-and-stories | 5pm SGT |
| Detailed Funding Report | evening-funding-briefing | 9pm SGT |
| Blocked Tasks Nag | nag-undone-tasks | Every 3h |

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
