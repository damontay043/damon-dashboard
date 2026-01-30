# MEMORY.md — Long-Term Memory

*Last updated: 2026-01-30*

## About Bro
- Reads aboutme_redacted.md on Scarlet2023 node for deep context
- Prefers aqicn.org over NEA for air quality (US EPA AQI standard, stricter)
- Uses Wispr Flow dictation — expect phonetic typos ("zhai" vs "zai", "CloudMD" vs "CLAUDE.md")
- Calls me "zai" (capable) — appreciates competence over politeness

## Setup & Config
- **TTS:** Edge TTS configured, voice = en-SG-WayneNeural (Singapore male, free). Bro picked this over American/British options
- **STT:** Pending — bro signing up for Groq API key (free tier). Python Whisper too heavy for VPS (3.7GB RAM)
- **Morning briefing cron:** 7am SGT daily — 11 items:
  1. Weather 2. AQI (3 regions) 3. Calendar 4. News 5. Market Ratios 6. Crypto overnight 7. Discord overnight 8. Workspace changes 9. Vault nudge 10. Node status 11. Joke
- **Market Ratios details:**
  - BTC price: `https://api.binance.com/api/v3/ticker/price?symbol=BTCUSDT`
  - Gold price: `https://api.binance.com/api/v3/ticker/price?symbol=PAXGUSDT`
  - BTC/Gold ratio = BTC ÷ Gold (oz of gold per BTC)
  - Gold/AISC ratio = Gold ÷ $1,550 (hardcoded AISC). Staleness reminder after Feb 11 2026
  - Sanity checks: BTC $50K-200K, Gold $2.5K-6K
- **Cron timeout:** 300s for morning briefing (extra API calls)
- **Heartbeat:** Every 30 min
- **Node:** Scarlet2023 (bro's home PC via WSL2) — has aboutme file and shared vault

## Bro's Interests (from conversations)
- Interested in building apps to make money — brainstormed 8 ideas
- Top picks: Funding Rate Dashboard (#1, 80/100) and Health Newsletter (#2, 75/100)
- Hasn't picked one to pursue yet — waiting for his response

## The Team — Two Agents, One Lord
- **Damon (me)** = Guan Yu at the frontier. 24/7 WhatsApp, always-on, proactive. Limited access by design (unsupervised = small blast radius)
- **Momo** = Zhuge Liang in the tent. Claude Code running locally on bro's PC (Scarlet2023). Summoned for heavy lifting — coding, analysis, spreadsheets, deep dives
- **Momo has full access:** Trello, Gmail, Google Sheets, full vault. I don't — intentional security boundary
- **"Momo said X"** = bro relaying from the other agent
- **Chatlogs:** DISCONTINUED (2026-01-28) — Momo now pulls conversations directly from WhatsApp MCP during /pitstop. No action needed from me.
- **Don't lose Jingzhou** = don't overreach, stay in lane, coordinate. Guan Yu's lesson.

## Platform Notes
- Moltbot rebranded from Clawdbot on 2026-01-27 (Anthropic trademark)
- WhatsApp is primary comms channel
- No email, calendar, or social integrations yet (Momo handles Gmail/Sheets for now)

## Lessons Learned (Recent: 2026-01-30)
- **Discord scroll fix:** End/Ctrl+End doesn't work in Discord's virtualized message list. Use PageDown x5-10 to scroll to latest messages.
- **Browser relay tab bug:** `browser action=open` creates NEW tab without relay attached. Always use `action=tabs` first to find existing attached tab, then use its `targetId` for snapshots. Never open new tabs for Discord monitoring!
- **30-min pulse cron** now includes: AQI (3 regions), BTC Basis (HL + Paradex), Funding Rates (HL + Paradex), Discord Sentiment
- **Funding rate spread** between HL and Paradex can be significant (saw ~20% APR delta) — useful arb signal for bro
- **Memory maintenance cron** added: Mon/Thu 2pm SGT, auto-updates MEMORY.md + syncs config to GitHub + sends bro .txt file

## Lessons Learned (Older)
- data.gov.sg PSI API works but bro doesn't trust NEA numbers
- Demo token on waqi.info API returns Shanghai data (useless), just scrape aqicn.org directly
- VPS too small for PyTorch/Python Whisper — always check resources before heavy installs
- **Don't lose Jingzhou!** Always check what Momo has already built before proposing new approaches
- Datacenter IP (Hetzner) = instant bot detection on Discord/social platforms. Residential IP only.
- Headless Chrome leaves fingerprints (webdriver flag, no audio devices). Not viable for bot-detecting platforms.
- Don't contaminate damontay043@gmail.com with risky activities (Discord bots etc.)
- Momo has **peterpoon** Discord account with Paradex access + extraction scripts already built
- For Discord sentiment monitoring: defer to Momo's setup on bro's PC, don't duplicate on VPS
- **Browser Relay pipeline working!** Chrome (Windows) → Relay (18791/18792) → Tailscale Serve → VPS Gateway → Damon
- controlUrl: https://scarlet2023.taile92d1e.ts.net/
- Use `profile="chrome"` to access bro's Chrome tabs
- Requires: Chrome open with extension ON + browser serve running (auto-starts via VBS) + Tailscale serve (backgrounded) + WSL2 node
- After PC reboot: only manual step is open Chrome Relay profile → go to Discord → click extension icon. Everything else auto-starts.
- Sentiment scoring: mood 30%, team responsiveness 25%, FUD intensity 20%, activity 15%, topic quality 10%. Alert if <40 or drop >15pts or exploit/hack/rug keywords
- Paradex Discord channels available: #general, #announcements, #network-upgrades, #trading-guild, #wen-tge, #developers, #feedback
- Node exec via `nodes run` needs approval — can't freely write files to Scarlet2023 yet
- GitHub Pages legacy build = simpler than workflow build for static HTML (no Actions workflow needed)

## Infrastructure
- **GitHub**: damontay043 account, PAT with repo scope. Dashboard at damontay043.github.io/damon-dashboard/
- **Gmail IMAP**: damontay043@gmail.com with app password (for email checking)
- **Brave Search**: API key configured in gateway
- **Dashboard system**: DASHBOARD.md → dashboard.html → GitHub Pages (heartbeat auto-sync)
- **Git repo clone**: /tmp/damon-dashboard (may not persist across reboots — re-clone if needed)
