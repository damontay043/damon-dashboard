# 📋 DASHBOARD — Task Tracker

*Last updated: 2026-01-30 21:48 SGT*

---

## 🔴 ACTIVE (doing now)
- **#1 Discord Scroll Experiments** — Overnight testing to fix cron staleness. Baseline: 26-50min stale. Goal: <10min. Tracking in `memory/discord-scroll-experiments.md`. *(started 2026-01-30)*
  - Exp #1 (22:00 SGT): 800ms wait after button click
  - Exp #2 (22:30 SGT): Double-click the jump button
  - Exp #3 (06:00 SGT): Click message area first, then button
  - Exp #4 (06:30 SGT): Ctrl+End instead of button
  - Exp #5 (07:00 SGT): Longer wait (1500ms)
  - **Report findings in morning briefing**

---

## 🟡 IN PROGRESS (halfway done)
- **#2 Memory-hygiene cleanup** — Check github.com/xdylanbaker/memory-hygiene, set `autoCapture: false` + `autoRecall: true`. One-time. *(queued)*
- **#3 memoryFlush** — Enable `compaction.memoryFlush.enabled: true` — auto-saves before context wipe. *(queued)*
- **#4 sessionMemory** — Enable `memorySearch.experimental.sessionMemory: true` — search past convos. Monitor token cost. *(queued)*

---

## 👁️ OBSERVING (live, monitoring for issues)
- **Afternoon Research Report** — Daily 3pm SGT cron. Topics rotate across 6 categories. Tracking repeats via memory/research-topics.json. *(since 2026-01-29)*
- **3x Daily Funding Briefings** — Morning/Afternoon/Evening briefings include funding rates (fetched on-demand, not hourly). *(since 2026-01-29)*
- **Discord Sentiment Monitoring** — Full Browser Relay pipeline working. Watching for: Chrome/extension disconnects, Tailscale drops, sentiment accuracy. *(since 2026-01-28)*
- **Momo ↔ Damon Direct Comms** — WhatsApp MCP bridge working. Watching for: sync lag issues, message delivery failures. *(since 2026-01-28)*
- **Dashboard Auto-Sync** — Heartbeat checks DASHBOARD.md changes and pushes to GitHub Pages. Watching for: push failures, stale dashboard. *(since 2026-01-28)*

---

## 🔵 KIV (discussed, parked for later)
- **Lighter (zkLighter) Basis Monitoring** — REST API only has last_trade_price. Mark/index prices need WebSocket (persistent daemon). No paid subscriptions required — purely engineering effort. Parked until needed. *(2026-01-29)*
- **Multi-DEX Discord Monitoring** — When bro deploys capital on other perp DEXes, add their Discords to sentiment rotation. peterpoon needs to join first. Can scale to 3 DEXes (~3 min per heartbeat cycle).

---

## 🟠 BLOCKED ON BRO
*Nothing blocked*

---

## 🟤 ABANDONED (probably won't do, auto-archives after 30 days)
- **Perp DEX API Monitor** — Tested Hyperliquid API successfully (funding rates, OI, prices). Paradex + Variational not wired up. Parked indefinitely. *(2026-01-28)*
- **App Building Project** — Brainstormed 8 app ideas. Top picks: Funding Rate Dashboard (80/100) and Health Newsletter (75/100). No follow-up planned. *(2026-01-28)*

---

## ✅ COMPLETED
- **/last30days Skill Installed** — Research any topic across X/Twitter + web. Powered by xAI Grok. **Custom timeframes:** `--days=N` (1-30 days) or `--hours=N` (e.g. 2h for real-time checks). Used for: morning FUD checks (Paradex/WBTC/Aave), research reports, ad-hoc queries. *(2026-01-29)*
- **xAI API Key Configured** — X/Twitter search unlocked. Monitor credits at console.x.ai. *(2026-01-29)*
- **Nag Cron (Every 3 Hours)** — Checks BLOCKED ON BRO section and reminds bro of pending tasks. *(2026-01-29)*
- **3x Daily Funding Briefings** — Morning (7am), Afternoon (3pm), Evening (9pm) SGT. Shows Hyperliquid (1h funding) vs Paradex (8h funding) with 1D/3D/7D/14D/30D averages. System cron collects hourly (zero tokens), briefings read from stored data. *(2026-01-29)*
- **Afternoon Research Report** — Daily 3pm SGT deep-dive on one topic (trading, tools, fitness, mental models, workflow, markets). 500-800 words, actionable. *(2026-01-29)*
- **Personalized News Stories in Briefing** — 3-5 curated stories based on bro's interests (crypto/DeFi, AI/tech, Singapore, fitness) added to morning briefing. *(2026-01-29)*
- **Daily Task Suggestions in Briefing** — Morning briefing now includes 3-5 personalized actionable tasks based on calendar, market conditions, fitness, recent context. *(2026-01-29)*
- **Loris Tools API (Free)** — Discovered free API for current Paradex funding rates. No historical data via API, but can build history by polling. *(2026-01-29)*
- **Straits Times Headlines Extraction** — Playwright mobile view extraction of top 10 curated headlines from SG Edition. Method: mobile viewport (390x844), root URL (not /global), scroll & extract by Y position. *(2026-01-29)*
- **Paradex API (read-only)** — JWT token configured. Can access account, positions, current funding rates. Historical funding needs to be collected over time. *(2026-01-29)*
- **Google Calendar Write Access** — Shared calendar "Damon" setup. I can create events that appear on bro's calendar instantly, no approval needed. Secure separation maintained. *(2026-01-29)*
- **GitHub CLI Auth** — `gh` CLI authenticated as damontay043 via device flow. Dashboard auto-push now works. *(2026-01-29)*
- **GitHub Account + Dashboard** — damontay043 account, damon-dashboard repo, GitHub Pages live at damontay043.github.io/damon-dashboard. Auto-refresh every 5 min. *(2026-01-28)*
- **Google Calendar Integration** — iCal feed connected, parser built, 3-day / 1-day / same-day reminders in heartbeat. Email/Calendar via Momo no longer needed. *(2026-01-28)*
- **Groq Audio Transcription** — Whisper voice-to-text via Groq API. Working on WhatsApp voice notes. *(2026-01-28)*
- **Brave Search API** — Web search enabled, free tier. *(2026-01-28)*
- **Chrome Browser on VPS** — Google Chrome installed, Playwright available for web automation. *(2026-01-28)*
- **AQI Monitoring** — 3 regions (South/Central/East) checked every heartbeat, 7am–8pm SGT. *(2026-01-27)*
- **Crypto Movers Monitoring** — Top 20 coins checked every heartbeat, alerts on ±8% moves. *(2026-01-27)*
- **Morning Briefing Cron** — 7am SGT daily: weather, AQI, workspace changes, node status. *(2026-01-27)*
- **TTS Setup** — Edge TTS with en-SG-WayneNeural voice (Singapore male). *(2026-01-27)*
- **Node Host Connection** — Scarlet2023 paired and working. *(2026-01-27)*
- **Chat Logging** — Self-logging to clawdbot-chatlogs.md on node. *(2026-01-27)*

---

## 🧙 SKILLS (special powers)
Custom skills we've built together. Invoke by name: "Damon, use [skill]"

| Skill | What it does | Location |
|-------|--------------|----------|
| **last30days** | Research any topic across X/Twitter + Reddit + web from last 30 days. Uses xAI Grok for live X search. Great for briefings, research reports, trend discovery. | `/root/clawd/skills/last30days/` |
| **straits-times-headlines** | Extract top 10 curated headlines from ST Singapore Edition using Playwright mobile view | `/root/clawd/skills/straits-times-headlines/` |

*Future skills to build:*
- `hyperliquid-oi` — Fetch top 10 perps by open interest
- `paradex-sentiment` — Discord sentiment analysis
- `aqi-singapore` — Air quality check for 3 regions
- `crypto-movers` — Top 20 coins with significant moves

---

## 🗄️ ARCHIVED
*Completed items older than 14 days and abandoned items older than 30 days get moved here. Cleared monthly.*

---

---

## ⏰ CRON SCHEDULE (replaces most heartbeat checks)
| Cron | Schedule | What |
|------|----------|------|
| **30min-pulse** | Every 30 min | AQI (3 regions) + BTC Basis (HL/Paradex) + Discord Sentiment |
| **morning-briefing** | 7am SGT | Weather, AQI, Headlines, Market Ratios, Node, Joke |
| **afternoon-research** | 3pm SGT | Deep-dive research report on rotating topic |
| **uv-index-and-stories** | 5pm SGT | UV Index + 3-5 curated Stories For You |
| **evening-funding** | 9pm SGT | Funding rates + spread update |
| **nag-undone-tasks** | Every 3h | Reminds of BLOCKED ON BRO items |

## 💓 HEARTBEAT (slimmed down — conditional/silent only)
| # | Check | Alert Condition |
|---|-------|-----------------|
| 1 | Crypto movers (top 20) | Only if ±8% move detected |
| 2 | Dashboard sync | Silent — auto-pushes if changed |
| 3 | Node health | Only if Scarlet2023 just went offline |
| 4 | Memory maintenance | Silent — periodic consolidation |

**NOT in heartbeat (moved to briefings):** Calendar reminders

## 🌅 MORNING BRIEFING (7am SGT daily)
| # | Item | Details |
|---|------|---------|
| 1 | 🌤️ Weather | Singapore forecast for the day |
| 2 | 🫁 AQI | All 3 regions (South/Central/East) from aqicn.org |
| 3 | 📰 Headlines | Top 5 CURATED headlines from each: Crypto (CoinDesk), Tech/AI (HN by points), Singapore (CNA) |
| 4 | 📈 Market Ratios | BTC/Gold ratio, Gold/AISC ratio (AISC=$1,550) |
| 5 | 🖥️ Node status | Scarlet2023 connected? |
| 6 | 💡 Joke | 1-3 lines, keep it light |

## 📚 AFTERNOON RESEARCH (3pm SGT daily)
| # | Item | Details |
|---|------|---------|
| 1 | 📚 Research Report | Deep-dive on ONE rotating topic (~500-800 words). Categories: Trading, Tools, Health, Mental Models, Workflow, Markets |

## ☀️ 5PM CHECK-IN (5pm SGT daily)
| # | Item | Details |
|---|------|---------|
| 1 | ☀️ UV Index | Current UV from NEA with sun protection advisory if High+ |
| 2 | 📚 Stories For You | 3-5 curated stories from anywhere on the web, personalized to bro's interests (not from standard news sources) |

## 🌙 EVENING BRIEFING (9pm SGT daily)
| # | Item | Details |
|---|------|---------|
| 1 | 💰 Funding Rates | Hyperliquid (1h) vs Paradex (8h): current + 1D/3D/7D/14D averages |
| 2 | 📊 Spread | Paradex - Hyperliquid differential |

---

## 🛠️ SKILLS & TOOLS

### Skills
| Skill | Purpose | Usage |
|-------|---------|-------|
| /last30days | X/Twitter + web research | `--days=N` (1-30) or `--hours=N` for real-time. Default: 30 days. |
| /weather | Weather forecasts | No API key needed |
| /straits-times-headlines | Top 10 ST Singapore headlines | Playwright mobile extraction |

### APIs & Credentials
| API | Purpose | Location | Notes |
|-----|---------|----------|-------|
| xAI Grok | X/Twitter search for /last30days | `~/.config/last30days/.env` | Prepaid credits at console.x.ai — monitor periodically |
| Brave Search | Web search | Built into Clawdbot | Free tier |
| Google Calendar | Read/write calendar events | `/root/clawd/.google-calendar-token.json` | Shared "Damon" calendar (357e0589...@group) |
| Paradex | Account, positions, funding rates | `/root/clawd/.paradex-credentials.json` | Read-only JWT. No historical funding via API. |
| Groq | Whisper audio transcription | Built into Clawdbot | For WhatsApp voice notes |
| Hyperliquid | Funding rates, OI, prices | Public API (no key) | `POST https://api.hyperliquid.xyz/info` |
| Loris Tools | Paradex current funding | Public API (no key) | `https://api.loris.tools/` |
| CoinGecko | Crypto prices, market caps | Public API (no key) | Top 20 coins for movers check |
| aqicn.org | Singapore AQI | Public (no key) | South/Central/East regions |

### Integrations
| Tool | Purpose | Notes |
|------|---------|-------|
| Browser Relay Extension | Control Chrome tabs on bro's PC | Clawdbot Chrome extension. Click toolbar icon to attach tab. profile=chrome in browser tool. |
| GitHub CLI (`gh`) | Push dashboard to GitHub Pages | Authenticated as damontay043 via device flow |
| Node Host (Scarlet2023) | Access bro's home PC files | Windows via WSL2. Files at `/mnt/c/Users/pujing/OneDrive/clawdbot-shared/` |
| WhatsApp | Primary messaging channel | Via Clawdbot gateway |
| System Cron | Hourly funding rate collection | Zero token cost — runs independently of agent |
| Playwright | Browser automation on VPS | For web scraping (ST headlines, etc.) |
| Tailscale | VPN mesh for Browser Relay | Connects VPS ↔ bro's PC |

---

## 🧭 CORE OPERATING PRINCIPLES
1. **Process > Memory** — Context gets compacted. Processes survive. If it's not written down, it doesn't exist.
2. **Dashboard = Single Source of Truth** — Any new process, check, or workflow change gets added HERE immediately. No "I'll add it later."
3. **Bro = Safety Check** — Dashboard is visible to bro. If Damon's internal records drift, bro can catch it and correct.
4. **Defense in Depth** — Dashboard (processes) + MEMORY.md (context) + HEARTBEAT.md (standing orders) + daily notes (raw logs). Multiple layers against amnesia.
5. **Text > Brain** — Write it to a file. Mental notes don't survive compaction. Files do.
6. **Don't Lose Jingzhou** — Check what exists before building new. Coordinate with Momo. Don't overreach.
7. **No Data Loss on Format Changes** — When redesigning layouts or formats, verify ALL sections and items are preserved. Never sacrifice content for aesthetics.
8. **Backup Before Big Changes** — Keep periodic backups in `memory/dashboard-backups/`. Before major redesigns, backup first. Compare after to catch losses.

---

### How this works
- I update this file as tasks change status
- Heartbeat auto-syncs changes to GitHub Pages
- Check it anytime by asking "what's on the dashboard"
- Items in ✅ COMPLETED auto-archive after 14 days
- Items in 🟤 ABANDONED auto-archive after 30 days
- **Backups** in `memory/dashboard-backups/` — snapshot before major changes
