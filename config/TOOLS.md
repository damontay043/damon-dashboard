# TOOLS.md - Local Notes

**My superpowers cheat sheet. If I forget something exists, check here first!**

---

## Discord Monitoring

**PRIMARY: JavaScript DOM extraction**
```
# 1. Find Discord tab
browser action=tabs profile=chrome target=host

# 2. Scroll to bottom
browser action=act ... request={"kind":"press","key":"End"}

# 3. Wait 3s, extract messages via JS
browser action=act ... request={"kind":"evaluate","fn":"() => { const msgs = document.querySelectorAll('[id^=\"chat-messages-\"]'); const results = []; for (const msg of msgs) { const header = msg.querySelector('[class*=\"headerText\"]'); const content = msg.querySelector('[id^=\"message-content-\"]'); const timestamp = msg.querySelector('time'); if (content || header) { results.push({ user: header ? header.textContent.trim().split('\\n')[0] : '(continued)', text: content ? content.textContent.trim() : '', time: timestamp ? timestamp.getAttribute('datetime') : '' }); } } return JSON.stringify(results.slice(-30)); }"}
# Returns: JSON array of {user, text, time(ISO UTC)} — add 8h for SGT
```

**Multi-channel navigation:**
```
Paradex #general:  https://discord.com/channels/1107916848193863740/1107916848193863743
Variational #general-chat: https://discord.com/channels/1196857788220067943/1197568193313640509
Lighter #general:  https://discord.com/channels/987399783985590322/1263245127674105877
Lighter #trading:  https://discord.com/channels/987399783985590322/1343587329666711573
# Navigate: browser action=act ... request={"kind":"evaluate","fn":"() => { window.location.href = '<URL>'; return 'navigating'; }"}
```

**Chrome Relay:** If `browser action=tabs` returns empty: wait 5s, retry 3x → CLI diagnostic `openclaw browser --browser-profile chrome tabs` → only then ask bro. Don't pass `target=host` — omit it.

**Discord Sentiment Report Format:**
```
💬 *Discord Sentiment ([HH:MM]-[HH:MM] SGT)*
*Score: XX/100 (Level)*
*Key conversations:*
• *[HH:MM]* — [Username]: "[actual quote]"
*Vibe:* [2-3 sentences]
```

---

## Local Setup: WSL2 on Home PC (DESKTOP-HT67ISQ)

**Direct file access:** `/mnt/c/Users/pujing/OneDrive/clawdbot-shared/` (OneDrive synced). No `host=node` needed.

**Read commands:** `cat`, `ls`, `head`, `tail`, `wc`, `grep`, `rg`, `find`, `stat`, `file`
**Write commands:** `cp`, `tee`, `mkdir` — ONLY to clawdbot-shared/. Notify bro of changes.
**NOT allowed:** `rm`, `mv` outside shared, any write outside `clawdbot-shared/`

| File | Purpose |
|------|---------|
| `.../clawdbot-shared/aboutme.md` | Core profile (condensed). Full version: `aboutme-archive.md` |
| `.../clawdbot-shared/wisdom.md` | Life principles, mental models, PVRR protocol |
| `.../clawdbot-shared/vps-config/` | Mirror of workspace config files |

**PowerShell:** Screenshot via `System.Drawing` + `CopyFromScreen`. Clipboard via `System.Windows.Forms.Clipboard`. Path: `/mnt/c/Windows/System32/WindowsPowerShell/v1.0/powershell.exe`

---

## Twitter/X Access

**Account:** @realdamontay — ⚠️ SUSPENDED. Auto-like disabled. Can still READ tweets via managed browser (profile=openclaw).
**Bro's account:** @realpujing

**Managed browser:** Already logged in. Use for reading tweets, FUD searches, profile checks. No liking/posting.
**Backup:** xAI/Grok API — semantic Twitter search. Config: `~/.config/last30days/.env`. Skill: `/last30days`. Uses prepaid credits, don't overuse.

---

## ⚠️ Funding Rate Annualization (NEVER GUESS — check this table)

| Venue | Interval | Raw Field | APR Formula | API Source |
|-------|----------|-----------|-------------|------------|
| **HL** | 1 hour | `funding` | rate × 8760 × 100 | POST /info `metaAndAssetCtxs` |
| **Paradex** | 8 hours | `funding_rate` | rate × 3 × 365 × 100 | GET /markets/summary?market=ALL |
| **Lighter** | 8 hours | `rate` | rate × 3 × 365 × 100 | GET /api/v1/funding-rates (filter exchange="lighter") |
| **Variational** | 8 hours | `funding_rate` | rate × 100 (already APR decimal) | GET /metadata/stats |
| **GMX** | continuous | from blended script | already annualized | dashboard-funding.sh |

**3rd-time MISS rule:** ALWAYS check this table before writing ANY funding rate formula. No exceptions.

---

## Price Data Reliability

| Source | Reliability | Best For |
|--------|-------------|----------|
| **Binance API** | ✅ High | Direct pairs (WBTC/BTC, BTC/USDT) |
| **Paraswap/1inch** | ✅ High | On-chain swap quotes |
| **DeFiLlama** | 🟡 Medium | Aggregated prices (can lag) |
| **CoinGecko** | 🔴 Low | Can be stale/wrong for wrapped tokens |

---

## Paradex API

**Base URL:** `https://api.prod.paradex.trade/v1/`
No creds migrated to WSL2. Funds withdrawn Feb 16 — monitoring paused. Public endpoints work (market data, funding rates).

---

## Lighter API ✅ WORKING

**Base URL:** `https://mainnet.zklighter.elliot.ai`
**No auth required.** Funding rate on 8-hour basis (pays hourly). APR = `rate × 3 × 365 × 100`.

Key endpoints: `/api/v1/orderBooks` (markets), `/api/v1/funding-rates` (all rates), `/api/v1/exchangeStats` (volume/OI).
Mark/Index prices: WebSocket only (`wss://mainnet.zklighter.elliot.ai/stream`). Script: `workspace/scripts/funding/lighter-prices.js`

---

## Variational API ✅ WORKING

**Base URL:** `https://omni-client-api.prod.ap-northeast-1.variational.io`
**No auth required.** Funding rate already APR decimal. APR = `funding_rate × 100`.

Key endpoint: `/metadata/stats` — all markets with mark_price, funding_rate, volume.
Index price NOT available — use cross-venue median. Script: `workspace/scripts/funding/variational-prices.js`

---

## TrainingPeaks API ✅ WORKING

**Account:** wuayteck@gmail.com | **Athlete ID:** 5177589
**Creds:** `/home/pujing/.openclaw/credentials/trainingpeaks.json`
**Script:** `/home/pujing/.openclaw/workspace/scripts/trainingpeaks/fetch-pmc.sh`

Quick fetch: `fetch-pmc.sh 180 /tmp/tp-pmc.json`
Token: Production_tpAuth cookie (long-lived) → auto-refresh access_token. If cookie expires: bro logs in, copies cookie from DevTools.
**Used in:** morning-wellness-deep-analysis cron

---

## Google Calendar (Write Access) ✅ WORKING

**Creds:** `/home/pujing/.openclaw/credentials/google-calendar-client.json`
**Tokens:** `/home/pujing/.openclaw/credentials/google-calendar-token.json`
**Script:** `/home/pujing/.openclaw/scripts/gcal-token.sh` (auto-refresh)
**Calendar:** damontay043@gmail.com (bro subscribed — events show on his calendar)

```bash
ACCESS_TOKEN=$(/home/pujing/.openclaw/scripts/gcal-token.sh)
curl -H "Authorization: Bearer $ACCESS_TOKEN" "https://www.googleapis.com/calendar/v3/calendars/primary/events"
```

---

## Google Sheets (Read Access) ✅ WORKING

**Tokens:** `/home/pujing/.openclaw/credentials/google-token.json` (calendar + spreadsheets.readonly scopes)
**Script:** `/home/pujing/.openclaw/scripts/gsheets-token.sh`
**Spreadsheet ID:** `1Asq3P2Nb54FR_0odDavw9kPIO0s8_ITX27dZzFmGyic`
**Sheets:** Workouts, Wellness, Journal, TDL, Sleep, Omron, Master, Books, Supplements, KIV, Archive, Night Schedule, Trips, AI Prompts, Links for Kindle, Cardio Time

```bash
SHEETS_TOKEN=$(/home/pujing/.openclaw/scripts/gsheets-token.sh)
curl -s -H "Authorization: Bearer $SHEETS_TOKEN" "https://sheets.googleapis.com/v4/spreadsheets/${SHEET_ID}/values/Wellness!A1:BZ5"
```
**Used in:** morning-wellness-deep-analysis cron

---

## Garmin Connect API ✅ WORKING

**Account:** wuayteck@gmail.com | **Lib:** `garminconnect` Python | **Tokens:** `~/.garminconnect/`

```python
from garminconnect import Garmin
client = Garmin()
client.login(tokenstore=os.path.expanduser("~/.garminconnect/"))
# Available: get_sleep_data, get_hrv_data, get_rhr_day, get_stress_data, get_respiration_data
```
Auto-refreshes on login. If tokens expire fully: need username+password from bro.
**Used in:** morning-wellness-deep-analysis cron

---

## Web Search (Brave API)

Built-in `web_search` tool. Returns titles, URLs, snippets. Use for research, docs, news, fact-checking.

---

## Browser Automation (Chrome Relay) ✅ ACTIVE

Chrome on Windows with OpenClaw extension. Discord logged in (peterpoon account).

**CRITICAL:** Use existing tabs only. `browser action=open` creates NEW tab without relay — DON'T USE. Always: `browser action=tabs` → find tab → use its `targetId`.

---

## TTS (Text-to-Speech)

**Edge TTS, voice = en-SG-WayneNeural.** Call `tts` tool → get file path → `message` with `filePath` + emoji-only caption (text captions get auto-TTS'd as female voice = duplicate clips).

---

## QMD Memory Search

**BM25 keyword search (fast, 0.25s):** `memory_search` tool
**Vector semantic search (slow, 12s):** `/home/pujing/.bun/bin/qmd vsearch "query" --json -n 5`

**Escalation:** memory_search → retry with keyword variations → qmd vsearch → OpenAI embeddings → accept "not found"

BM25 is weak for conceptual queries. Use exact words bro would have used. Direct CLI: `/home/pujing/.bun/bin/qmd search "keywords" --json -n 8`

---

## Cron Jobs (21 active, 3 undocumented)

| Name | Schedule | Model | Purpose |
|------|----------|-------|---------|
| aave-health-alert | Every 2m (6am-11pm) | Sonnet | Health factor monitoring, 3 wallets |
| morning-briefing | 7:00 AM | Opus | Weather, AQI, headlines, calendar, markets, Discord, joke |
| wallet-spy | 7am/1pm/7pm | Opus | 12 wallets (ETH+ARB+HL) |
| variational-funding-rates | 7:40am/3:40pm | Sonnet | ETC/ETH/SOL funding on Variational |
| hourly-pulse | :45 past (6,10,14,18,20) | Opus | AQI, basis, funding, Aave, peg, FUD, gold, Discord |
| defidojo-day | :20 past (6,10,14,18,21) | Opus | DeFi Dojo daytime intel |
| nag-headline-inputs | 9am/12/3/6/9pm | Sonnet | Nag for headline preferences |
| nag-undone-tasks | 9am/12/3/6/9pm | Sonnet | Check blocked/forgotten tasks |
| deep-self-review | 12am/9am/12/3/6/9pm | Opus | Config compliance audit + MISS/HIT logging |
| refresh-rules-nudge | 10am/2/6/10pm | systemEvent | Re-read self-review rules |
| trello-q1-helper | 1:00 PM | Opus | Trello Q1 tasks |
| hl-oi-top10 | 2:00 PM | Sonnet | Hyperliquid top 10 OI |
| afternoon-research | 3:00 PM | Opus | Alternating: Twitter trends / deep-dive |
| aboutme-enrichment | 4:00 PM | Opus | Learn about bro, log to interview file |
| defidojo-daily-channels | 4:20 PM | Opus | DeFi Dojo #hedge-arb + #hl-and-frenemies |
| uv-index-and-stories | 5:00 PM | Opus | UV index, story, evening focus points |
| daily-business-idea | 6:00 PM | Opus | Solopreneur business idea |
| morning-wellness-deep-analysis | 6:30 PM | Opus | Garmin + TP + Sheets health report |
| daily-calibration-interview | 7:00 PM | Opus | Personality/capability calibration |
| evening-funding-briefing | 9:00 PM | Opus | Detailed funding rates all venues |
| defidojo-night | :20 past (22-5) hourly | Opus | DeFi Dojo overnight scan |

**Note:** 3 additional crons discovered in system (perp-collateral-alert, funding-rate-alert, daily-funding-report) - schedule/purpose TBD.

Manage: `cron list`, `cron add`, `cron update`, `cron remove`, `cron runs <jobId>`

---

## Messaging

**WhatsApp:** Primary channel. DM allowlist (bro only). Text, images, audio, documents, reactions.

---

## Superpowers Summary

| Power | Status |
|-------|--------|
| Read tweets (managed browser) | ✅ (account suspended but can read) |
| Search Twitter (xAI/Grok + Brave) | ✅ |
| Web search (Brave) | ✅ |
| Read bro's files (/mnt/c/) | ✅ Always |
| Browser control (Chrome Relay) | ✅ |
| Discord monitoring (3 servers) | ✅ |
| Windows screenshots/clipboard | ✅ |
| Google Calendar (write) | ✅ |
| Google Sheets (read) | ✅ |
| Garmin health data | ✅ |
| TrainingPeaks PMC | ✅ |
| Wallet spy (12 wallets) | ✅ |
| Gold spread (PAXG vs XAUT) | ✅ |
| Mayer Multiple | ✅ |
| Lighter/Variational/Paradex data | ✅ |
| Aave health monitoring (3 wallets) | ✅ |
| BTC peg monitoring | ✅ |
| Trello tasks | ✅ |
| Voice output (Edge TTS) | ✅ |
| 22 active cron jobs | ✅ |
| Memory (files + QMD BM25) | ✅ |
| Codex CLI (coding delegation) | ✅ |
| Gemini CLI (large context) | ✅ |

**Codex/Gemini delegation templates:** See `TOOLS-codex-delegation.md` for PRD templates, QA checklists, and routing decision tree.

---

## Trello API

**Creds:** `/home/pujing/.openclaw/credentials/trello.json` | **Account:** ngwuayteck
**Boards:** TDL (Eisenhower Matrix), Kanban Template
**Base:** `https://api.trello.com/1/`
Key ops: GET boards (`/members/me/boards`), lists (`/boards/{id}/lists`), cards (`/lists/{id}/cards`), POST cards, PUT move cards.

---

## Dashboard (GitHub Pages)

**Repo:** damontay043/damon-dashboard | **URL:** https://damontay043.github.io/damon-dashboard/
Update DASHBOARD.md → regenerate HTML → git push.
