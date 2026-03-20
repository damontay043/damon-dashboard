# MEMORY.md — Long-Term Memory

*Last updated: 2026-03-20 21:50*
*Older entries archived to: MEMORY-archive.md*

## Recent Updates (2026-03-20)
- **variational-funding-rates CONFIRMED** — Bumped 120→180s, ran 3:40 PM in 93s ✅. consecutiveErrors=0.
- **nag-undone-tasks timeout** — Timed out at 3:00 PM (180s). Bumped 180→240s. Next test tomorrow 3 PM.
- **Momo audit 49/100** — Down from 51. Housekeeping/noise issues, not output quality.
- **Session cleanup** — 501MB → 26MB (95% reduction). `cron.sessionRetention: "1h"` configured. Gateway SIGUSR1 restart, all 22 crons verified.
- **Morning briefing enriched** — Google Trends SG added (RSS feed). Discord-sentiment updated with deposit/withdrawal highlights.
- **defidojo-daily-channels close to wire** — 393s/420s (93.6%). Monitor for timeout bump.
- **Clean day** — 0 new MISSes across 5 self-reviews (09:00-21:00). Best operational day continues from Mar 19.

## Recent Updates (2026-03-19)
- **FOMC result** — Fed held rates at 3.5-3.75%, 11-1 vote. No rate change as expected. Iran war dominating geopolitical backdrop.
- **nanny-birthday-video-reminder** — Fired 8 AM ✅, auto-deleted. Birthday dinner at Garibaldi 7 PM tonight.
- **Upcoming events** — HSBC Gang dinner Mon 23 Mar 7 PM, Guoxiang birthday Tue 24 Mar.
- **Both timeout tests PASSED** — wallet-spy 195s/540s ✅, defidojo-daily-channels 259s/420s ✅. All 18+ crons at 0 consecutive errors.
- **Best operational day** — 0 MISSes across 6 self-reviews (09:00-21:00). No bro corrections.
- **Chrome relay instability** — CDP WebSocket goes stale after 1-2hrs, tabs show but "tab not found" on interact. Bro restarted Chrome 3x. OpenClaw 2026.3.13 has browser session lifecycle fix for this.
- **OpenClaw 2026.3.13 update research done** — Key fixes: browser session lifecycle (relay issue), cron deadlock prevention, memory regression, compaction persona preservation. 80/100 confidence. Awaiting bro's approval.
- **BTC funding flipped NEGATIVE** — HL BTC -13.22% instantaneous at 6:45 PM after 4-day positive run. Net P&L dropped +$335→-$63/day. BTC confirmed drag.
- **KAT blacklisted** — +119% spike, bro said "BL kat".
- **Cron snapshot exported** — Full 22-job list to `clawdbot-shared/vps-config/CRON-SNAPSHOT-260319.md` for Momo.

## Recent Updates (2026-03-18)
- **Position update confirmed** — Bro's current positions: SHORT BTC (HL, Lighter), SHORT ETH (HL, Paradex). NO GMX ETH. NO Lighter ETH. All crons updated with 📍/📎 tagging system.
- **Google OAuth fixed** — Both Calendar + Sheets tokens refreshed Mar 17 3:36 PM. Wellness cron, calendar checks, haircut reminders all unblocked.
- **Lunch with Yaozu** — Wed Mar 18 12pm, Fun Toast Woodleigh Mall. coffee-teck-reminder fired 8 AM ✅.
- **daily-funding-report PASSED** — 39s with 540s timeout ✅. Massive improvement (was 437s).
- **wallet-spy timeout bumped 420→540s** — Timed out at 425s. Bump applied. Testing Mar 19 1:15 PM.
- **defidojo-daily-channels timeout bumped 300→420s** — Timed out at 300s limit. Bump applied. Testing Mar 19 4:20 PM.
- **Clean day** — 7 self-reviews (00:00-00:00), 0 new MISSes. No bro corrections.

## Recent Updates (2026-03-17)
- **WhatsApp overnight instability** — Critical perp collateral alert queued ~45min during disconnects (00:55-06:50).
- **Google Calendar tokens fixed** — Both refreshed Mar 17 3:36 PM ✅.
- **Mayer Multiple 0.797** — Deep Value territory (20.4% below 200d SMA). BTC $74,680.
- **Cron timeouts bumped** — daily-funding 420→540s (PASSED Mar 18 ✅), wallet-spy 300→420s→540s, deep-self-review 300→420s ✅.
- **WA delivery** — ~50% success rate (up from 23% Mar 16). Still intermittent.

## Recent Updates (2026-03-16)
- **Wallet-spy HIP-3 integration deployed** — Scans all 7 HIP-3 dexes (xyz, flx, vntl, hyna, km, abcd, cash) per wallet. HL API: pass `"dex":"<name>"` to `clearinghouseState`. `perpDexs` endpoint lists available dexes. fab3+96e4 positions discovered.
- **Wallet-spy Ostium integration deployed** — Public subgraph at `api.subgraph.ormilabs.com`, no auth. fab3: 7x SHORT CL/USD $5.75M on Ostium (bro says delta-neutral with long oil elsewhere).
- **Perp collateral buffer raised 15% → 20%** — Bro's request via alert response.
- **Aave ETH 3-3 muted till 9pm + false RECOVERED alert fix** — Added muteUntil check to suppress recovery msgs during mute period. One-shot unmute cron at 9 PM.
- **BTC funding flipped green across ALL venues** — First time in 7+ days. Lighter diverging (deeply negative on BTC+ETH).
- **WhatsApp delivery ~23% success rate** — 3 delivered out of ~13 attempts. Worst day yet. 3rd consecutive day intermittent.

## Recent Updates (2026-03-14/15)
- **Momo spot-check results (Mar 14)** — Mixed: 2A, 3B/C, 3F. Immediate fixes applied.
- **Memory-consolidation rescheduled 4x/day (Mar 15)** — Runs at 03:50, 09:50, 15:50, 21:50 SGT.
- **OpenClaw 2026.3.13 released (Mar 14)** — Researched + bro alerted. Awaiting update (still on 2026.2.17).

## Key Lessons (Added from 2026-03-01/02)
- **Cron relay format LOCKED IN — TWO-MESSAGE mandatory** — All review-mode cron reports: (1) Full original report, no condensing. (2) Separate "Damon's Take" with contextual analysis. Bro confirmed nag persistence: keep nagging on schedule, don't self-censor.
- **Review-mode delivery architecture** — Removing a delivery mechanism without replacing its function is worse than keeping it broken. Multiple delivery paths prevent single points of failure.
- **Verification theater pattern** — Four consecutive reviews claimed fields existed when missing. Always verify actual data, never assume success without confirmation.
- **Calendar API error handling** — Never report "clear" when API fails. Always flag errors explicitly: "⚠️ Calendar check FAILED" vs fabricating "clear week ahead" when there were 5 events.

---

## About Bro
- **Tier 1 files:** `aboutme-core.md` (condensed profile) + `aboutme-archive.md` (full details) + `wisdom.md` in clawdbot-shared/
- Prefers aqicn.org over NEA for air quality (US EPA AQI standard)
- Uses Wispr Flow dictation — expect phonetic typos
- Calls me "zai" (capable) — appreciates competence over politeness
- Gaming policy: only "polytopia-like" games (short sessions, no fixed time commitment)

## Setup & Config
- **TTS:** Edge TTS, voice = en-SG-WayneNeural (Singapore male, free)
- **STT:** Groq Whisper (WhatsApp voice notes)
- **Morning briefing:** 7am SGT daily — Weather, AQI, Calendar, News, Market Ratios, Crypto overnight, Discord, Workspace changes, Vault nudge, Joke
- **Market Ratios:** BTC/Gold ratio, Gold/AISC ratio ($1,550 hardcoded). Sanity checks: BTC $50K-200K, Gold $2.5K-6K
- **Heartbeat:** Active hours 6am–11pm SGT (model = Codex)

## The Team — Two Agents, One Lord
- **Damon (me)** = Guan Yu at the frontier. 24/7 WhatsApp, always-on, proactive
- **Momo** = Zhuge Liang in the tent. Claude Code on bro's PC. Full MCP access (Trello, Gmail, Sheets)
- **Don't lose Jingzhou** = don't overreach, stay in lane, coordinate

## Platform Notes
- WhatsApp is primary comms channel
- Google Calendar write access, Sheets read access, Gmail IMAP read access
- Twitter read access via Bird CLI (@realdamontay). No posting.

## Key Lessons (Stable)
- Browser Relay: Use `profile="chrome"`. Use `action=tabs` first, never `action=open` for monitoring
- Discord scroll: PageDown x5-10 (End key doesn't work in virtualized list)
- Cron delivery: Always use explicit `message` tool calls, don't rely on `announce`
- WSL2 = direct file access via `/mnt/c/`. Always accessible.
- Signal before going dark: always tell bro before tasks >30s or restarts

## Infrastructure
- **Platform:** WSL2 Ubuntu 24.04 on home PC (DESKTOP-HT67ISQ). OpenClaw 2026.2.17 (REGRESSED from 2026.3.12 — needs re-update)
- **GitHub**: damontay043 account. Dashboard at damontay043.github.io/damon-dashboard/
- **Gmail IMAP**: damontay043@gmail.com with app password
- **Direct file access**: `/mnt/c/Users/pujing/OneDrive/clawdbot-shared/`
- **Morning briefing pre-compute**: `scripts/morning-briefing/morning-briefing-gather.sh` parallelizes 14 sources (~12.5s)
- **Discord scan state**: Unified JSON at `memory/discord-scan-state.json` for 5 channels ("NEW MESSAGES ONLY" filtering)
- **PAXG**: Removed from ALL funding analysis (bro's decision). Gold MACRO data stays in morning briefing.
- **Macro Catalyst**: Added permanently to morning-briefing, hourly-pulse, daily-funding-report
