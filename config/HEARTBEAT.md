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
   - If changed: copy to `/tmp/damon-dashboard/config/`, regenerate HTML if needed, git commit + push
   - Track in `memory/dashboard-state.json`
   - Silent — don't message bro unless push fails

3. **Config Sync (Local)** — Sync core files to OneDrive shared folder.
   - **Path:** `/mnt/c/Users/pujing/OneDrive/clawdbot-shared/vps-config/`
   - Files: AGENTS.md, SOUL.md, USER.md, IDENTITY.md, TOOLS.md, HEARTBEAT.md, NOW.md, DASHBOARD.md, MEMORY.md
   - Now local (no node connection needed) — just copy files directly
   - OneDrive will sync to cloud automatically

4. **Memory Maintenance** — Every 3 days minimum (check `memory/memory-maintenance.json`)
   - Review daily notes in `memory/YYYY-MM-DD.md` since last maintenance
   - Consolidate important items into MEMORY.md
   - Update the "Last updated" date in MEMORY.md header
   - Track last maintenance date in `memory/memory-maintenance.json`
   - Silent unless MEMORY.md was significantly updated

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

## Discord Scroll Fix (added 2026-01-30)

When checking Discord via browser relay:
- **DO NOT use End or Ctrl+End** — doesn't work with Discord's virtualized message list
- **USE PageDown key x5-10** — this reliably scrolls to recent messages
- Always verify timestamps in snapshot match expected timeframe before reporting

## Track State

Update `memory/heartbeat-state.json` with timestamps of conditional checks.
