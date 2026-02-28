# TOOLS.md - Local Notes

Skills define *how* tools work. This file is for *your* specifics — the stuff unique to your setup.

**This is my cheat sheet of superpowers. If I forget something exists, check here first!**

---

## ⚠️ DISCORD MONITORING — IMPORTANT!

**PRIMARY METHOD: JavaScript DOM extraction (discovered 2026-02-08)**

Extract messages directly as structured JSON — no screenshots needed for data:
```
# 1. Find Discord tab
browser action=tabs profile=chrome target=host

# 2. Scroll to bottom
browser action=act ... request={"kind":"press","key":"End"}

# 3. Wait 3 seconds, then extract messages via JS
browser action=act ... request={"kind":"evaluate","fn":"() => { const msgs = document.querySelectorAll('[id^=\"chat-messages-\"]'); const results = []; for (const msg of msgs) { const header = msg.querySelector('[class*=\"headerText\"]'); const content = msg.querySelector('[id^=\"message-content-\"]'); const timestamp = msg.querySelector('time'); if (content || header) { results.push({ user: header ? header.textContent.trim().split('\\n')[0] : '(continued)', text: content ? content.textContent.trim() : '', time: timestamp ? timestamp.getAttribute('datetime') : '' }); } } return JSON.stringify(results.slice(-30)); }"}

# Returns: JSON array of {user, text, time(ISO UTC)} — add 8h for SGT
# ~2KB vs ~100KB screenshot. Exact timestamps, zero fabrication risk.
```

**SUPPLEMENTARY:** Still take screenshots for visual context, but text data comes from JS extraction.

**FALLBACK:** If JS extraction fails, use screenshot method: PageUp 3x → screenshot → analyze visually.

**MULTI-CHANNEL MONITORING (discovered 2026-02-08):**
Navigate between Discord servers/channels in same tab, extract, navigate back:
```
# Channel URLs (Discord SPA, instant navigation):
Paradex #general:  https://discord.com/channels/1107916848193863740/1107916848193863743
Lighter #general:  https://discord.com/channels/987399783985590322/1263245127674105877
Lighter #trading:  https://discord.com/channels/987399783985590322/1343587329666711573

# Navigate: browser action=act ... request={"kind":"evaluate","fn":"() => { window.location.href = '<URL>'; return 'navigating'; }"}
# Wait 4s for load, then run JS extraction
# Navigate back to default channel after extraction
```

**Chrome Relay Retry Protocol (learned 2026-02-12):**
If `browser action=tabs profile=chrome` returns empty:
1. **Wait 5s, retry up to 3 times** — WebSocket handshake can take time after extension click
2. **CLI diagnostic:** `openclaw browser --browser-profile chrome tabs` via exec — CLI may see tabs before tool does
3. **Only THEN ask bro** to re-click extension (only if both tool AND CLI return empty)
4. Don't pass `target=host` — omit it, the built-in chrome profile routes correctly without it

If genuinely disconnected → tell bro: "Chrome tab detached, please reattach the OpenClaw extension to Discord tab"

**Chrome Relay "Ghost Tab" Fix (learned 2026-02-24):**
Sometimes a tab shows up in `tabs` list but ALL actions on it fail with "tab not found". The tab is "ghost" — relay sees it but can't interact. Navigating within the tab (via evaluate/navigate) can cause this.
**Fix:** Bro must:
1. Close the dead/ghost tab entirely
2. Open a brand new tab
3. Navigate to the Discord channel URL
4. Click the OpenClaw extension icon on the new tab
Re-clicking the extension on the ghost tab does NOT work — must be a fresh tab.
**Do NOT try to navigate between Discord servers in the same tab** — this breaks the relay connection. Each Discord server should have its own dedicated tab.

---

## 🖥️ Local Setup: WSL2 on Home PC (DESKTOP-HT67ISQ)

**Migrated 2026-01-31** from Hetzner VPS to spare Windows PC at home, running WSL2 Ubuntu 24.04.

**Direct file access:**
- Shared folder: `/mnt/c/Users/pujing/OneDrive/clawdbot-shared/` (OneDrive synced)
- **No more `host=node` needed!** I can access files directly via `/mnt/c/`
- systemd auto-starts OpenClaw on boot

### Read commands (use freely):
`cat`, `ls`, `head`, `tail`, `wc`, `grep`, `rg`, `find`, `stat`, `file`

### Write commands (allowed, but NOTIFY BRO of any changes):
`cp`, `tee`, `mkdir`
- **Scope:** ONLY write to `/mnt/c/Users/pujing/OneDrive/clawdbot-shared/` — nowhere else
- **Config sync:** When you modify your own workspace files (SOUL.md, USER.md, IDENTITY.md, TOOLS.md, AGENTS.md, HEARTBEAT.md), sync the updated copy to `/mnt/c/Users/pujing/OneDrive/clawdbot-shared/vps-config/`
- **Always tell bro** what you changed and why, either immediately or in the next morning briefing

### NOT allowed:
`rm`, `mv` (to destinations outside shared), any write outside `clawdbot-shared/`

### Key Files

| File | Purpose | When to read |
|------|---------|--------------|
| `/mnt/c/.../clawdbot-shared/aboutme.md` | Full unredacted profile (health, wealth, wisdom, relationships) | Start of main sessions, or when you need deep user context |
| `/mnt/c/.../clawdbot-shared/wisdom.md` | Life principles, mental models, PVRR protocol | When you need deep context on bro's philosophy |
| `/mnt/c/.../clawdbot-shared/vps-config/` | Mirror of my workspace config files | Sync here after any changes to core files |
| `/mnt/c/.../clawdbot-shared/archive chatlogs/` | Momo/Cody archived session logs (YYMMDD.md) | Session start — read only NEW files since last marker (see below) |

**Momo Archive Read Protocol:**
- Last read marker: `260226.md` (all 16 files 260110–260226 available as of Feb 28)
- At session start: `ls` the directory, check for files newer than marker
- If new files → read only the delta, update marker here
- If no new files → skip. Zero token cost.
- Do NOT read all archives on every session — read only the delta

### 🖥️ PowerShell Integration (WSL2 → Windows)

**Since I'm running on WSL2, I can execute PowerShell commands directly on Windows!**

**Path:** `/mnt/c/Windows/System32/WindowsPowerShell/v1.0/powershell.exe`

**Verified capabilities (2026-01-31):**

| Capability | How | Use Case |
|------------|-----|----------|
| **Screenshot** | `System.Drawing` + `CopyFromScreen` | Capture desktop state |
| **Clipboard** | `System.Windows.Forms.Clipboard` | Read/write clipboard |
| **Launch apps** | `Start-Process` | Open Notepad, Chrome, etc. |
| **Process list** | `Get-Process` | See what's running/hogging memory |
| **System info** | `Get-CimInstance`, `$env:` | PC name, user, OS details |

**Example - Take screenshot:**
```powershell
/mnt/c/Windows/.../powershell.exe -Command "
Add-Type -AssemblyName System.Windows.Forms
\$screen = [System.Windows.Forms.Screen]::PrimaryScreen.Bounds
\$bitmap = New-Object System.Drawing.Bitmap(\$screen.Width, \$screen.Height)
\$graphics = [System.Drawing.Graphics]::FromImage(\$bitmap)
\$graphics.CopyFromScreen(\$screen.Location, [System.Drawing.Point]::Empty, \$screen.Size)
\$bitmap.Save('C:\Users\pujing\OneDrive\clawdbot-shared\screenshot.png')
"
```

**NOT useful:** Windows toast notifications (bro not watching spare PC screen — use WhatsApp instead)

---

## 🐦 Twitter/X Access — MANAGED BROWSER + xAI

**Account:** @realdamontay (Damon Tay — MY account) — ✅ RESTORED (Feb 19). Was suspended Feb 13 due to auto-like cron.
**User ID:** 2017069604896722944
**Bro's account:** @realpujing (Pu Jing)
**Auto-like:** ❌ PERMANENTLY DISABLED — caused the Feb 13 suspension. X explicitly cited "aggressive/random liking" as violation. Repeated violations = permanent ban. Cron `twitter-auto-like` must NEVER be re-enabled.

### ❌ Bird CLI — DEAD (2026-02-08)
steipete took down Bird CLI — X killed the Web API it relied on (pay-per-use enforcement).
**Do NOT attempt to use `dbird` — it will always return 401.**

### ✅ PRIMARY: Managed Browser (profile=openclaw)
Already logged into x.com as @realdamontay via cookie injection. Can still READ tweets even though account suspended.
```
# Read a tweet
browser action=navigate profile=openclaw targetUrl="https://x.com/steipete/status/XXX"
# Wait 5s, then extract text
browser action=act profile=openclaw request={"kind":"evaluate","fn":"() => { ... }"}
```
**Use for:** Reading tweets, FUD searches, profile checks. NO automated liking/posting (caused suspension, will cause permanent ban if repeated).

### ✅ BACKUP: xAI/Grok API
Semantic Twitter search when browser is unavailable.
See xAI section below for details.

---

## 🤖 xAI API (Grok) — BACKUP TWITTER + AI SEARCH

**Config:** `~/.config/last30days/.env`
**Console:** https://console.x.ai/
**Skill:** `/last30days`

**What it does:**
- Search X/Twitter via Grok AI (semantic search, not just keyword)
- Can find tweets even when Bird CLI fails
- Provides AI-summarized results

**When to use:**
- Bird CLI fails or cookies expired
- Need semantic search ("tweets about AI agents being helpful")
- Want AI-curated results rather than raw tweets

**Commands:**
```bash
# Via the /last30days skill
/last30days "moltbot tips" --limit 10
```

**Cost:** Uses prepaid xAI credits. Monitor at console.x.ai. Don't overuse.

**Note:** Bird CLI is DEAD. Managed browser is primary for Twitter reading. Use xAI as backup for semantic search.

---

## ⚠️ Price Data Reliability

**Lesson learned (2026-02-06):** Not all price APIs are created equal.

| Source | Reliability | Best For |
|--------|-------------|----------|
| **Binance API** | ✅ High | Direct pairs (WBTC/BTC, BTC/USDT) — liquid, real-time |
| **Paraswap/1inch** | ✅ High | On-chain swap quotes — reflects actual DEX liquidity |
| **DeFiLlama** | 🟡 Medium | Aggregated prices — generally good but can lag |
| **CoinGecko** | 🔴 Low | Can be stale/wrong, especially for wrapped tokens — avoid for accuracy-critical checks |

**Rule:** For wrapped token pegs (WBTC, cbBTC), use on-chain swap quotes (Paraswap) or CEX direct pairs (Binance), NOT aggregator prices like CoinGecko.

---

## 📊 Perp Dashboard API ✅ WORKING (2026-02-21)

**Built by Momo.** JSON API for bro's perp positions dashboard.
**Dashboard UI:** `https://perp-dashboard.pages.dev/` (password: partyhat2026)
**Worker Proxy (API):** `https://perp-funding-proxy.wuayteck.workers.dev`
**Direct VPS:** `http://46.225.5.248:3001` (port 3001, NOT 3000)
**Auth:** None required

**Endpoints:**
| Endpoint | What |
|----------|------|
| `/api/positions` | All perp positions across HL/Lighter/Paradex (entry, live price, PnL, liq) |
| `/api/collateral?leverage=10` | Equity, collateral needed, surplus per exchange |
| `/api/aave` | All 4 Aave wallet cards with HF, collateral, debt |
| `/api/perp-funding` | Funding rates (existing, blended + timeframes) |

**Quick fetch:**
```bash
# Collateral health (for monitoring)
curl -s "https://perp-funding-proxy.wuayteck.workers.dev/api/collateral?leverage=10"

# Aave health
curl -s "https://perp-funding-proxy.wuayteck.workers.dev/api/aave"

# All positions
curl -s "https://perp-funding-proxy.wuayteck.workers.dev/api/positions"

# Net P&L (funding earned - Aave borrow cost + Aave supply interest)
curl -s "https://perp-funding-proxy.wuayteck.workers.dev/api/net-pnl"
```

**`/api/net-pnl` schema (added 2026-02-22):**
- `dailyFundingEarned` — net funding from all perp positions (negative = shorts paying)
- `dailyBorrowCost` — Aave stablecoin borrow interest
- `dailySupplyEarning` — Aave collateral supply interest (offsets borrow cost)
- `dailyNetAaveCost` — borrow - supply = true Aave cost
- `dailyNetPnl` — funding - netAaveCost = TRUE daily P&L
- `annualizedNetPnl` — dailyNetPnl × 365
- `totalPerpNotional` / `fundingAnnualizedRate` / `netPnlAnnualizedRate` — rates as % of notional
- `assets` — per-asset breakdown (BTC/ETH/PAXG) with `fundingByExchange`, `notionalWeight`
- `meta.positionsError` — check for empty string; if non-empty, per-asset data unavailable (Lighter rate limit)
- `meta.aaveError` — check for empty string; if non-empty, Aave cost data unavailable
- **Graceful degradation:** 15s per-upstream timeout. If positions times out (Lighter rate limit), returns aggregate funding + Aave costs with empty `assets`. Cache warms from successful positions fetch → subsequent calls instant.

**Note:** `/api/positions` currently missing `funding24h` field — requested from Momo. Once added, will integrate into hourly pulse for per-instrument funding earned reporting.

---

## 📊 Paradex API

**Credentials:** ⚠️ OLD PATH `/root/clawd/.paradex-credentials.json` — NOT on current WSL2 system. No Paradex API creds migrated.
- Public endpoints work without auth (market data, funding rates)
- Bro withdrew funds from Paradex (Feb 16) — account monitoring paused

**Base URL:** `https://api.prod.paradex.trade/v1/`
**Auth header:** `Authorization: Bearer <jwt>`

**Useful endpoints:**
- `/account` — Account value, margin, collateral
- `/markets/summary?market=BTC-USD-PERP` — Current funding rate, OI, prices
- `/account/funding?market=X` — Funding payments (requires open positions)

**Note:** Historical funding rates not available via REST API. Need to collect snapshots over time to build history.

---

## 📊 Lighter API ✅ WORKING (2026-02-03)

**Base URL:** `https://mainnet.zklighter.elliot.ai`
**Auth:** Not required for public endpoints
**SDK:** `zklighter-sdk` on npm

**Useful endpoints:**
- `/api/v1/orderBooks` — List all markets
- `/api/v1/orderBookDetails?market_id=1` — BTC market details (market_id=1)
- `/api/v1/funding-rates` — Current funding rates for all markets
- `/api/v1/exchangeStats` — Volume, OI, price stats

**Funding Rate Calculation:**
- API returns rate on **8-hour basis** (even though Lighter pays hourly!)
- APR formula: `rate × 3 × 365 × 100`
- Example: rate 0.000096 → 0.000096 × 3 × 365 × 100 = 10.51% APR

**Get BTC funding:**
```bash
curl -s "https://mainnet.zklighter.elliot.ai/api/v1/funding-rates" | jq '.funding_rates[] | select(.symbol == "BTC" and .exchange == "lighter")'
```

**Mark/Index prices:** Only available via WebSocket (REST has no prices)
- Endpoint: `wss://mainnet.zklighter.elliot.ai/stream`
- Subscribe: `{"type": "subscribe", "channel": "market_stats/all"}`
- BTC is market_id=1
- Script: `workspace/scripts/funding/lighter-prices.js`

---

## 📊 Variational API ✅ WORKING (2026-02-03)

**Base URL:** `https://omni-client-api.prod.ap-northeast-1.variational.io`
**Auth:** Not required for public endpoints

**Useful endpoints:**
- `/metadata/stats` — All market listings with mark_price, funding_rate, volume

**Get BTC data:**
```bash
curl -s "https://omni-client-api.prod.ap-northeast-1.variational.io/metadata/stats" | jq '.listings[] | select(.ticker == "BTC")'
```

**Funding Rate:**
- API returns `funding_rate` already as APR in decimal form
- APR formula: `funding_rate × 100`
- Example: 0.0416 → 4.16% APR
- Funding interval: 8 hours (`funding_interval_s: 28800`)

**Mark Price:** Available via API (`mark_price` field in /metadata/stats)
**Index Price:** NOT available via API
- **Solution:** Use Variational's OWN mark_price + CROSS-VENUE MEDIAN of other exchanges' INDEX prices
- Script fetches Variational mark from their API, then gets index from HL, Binance, Paradex → median of indexes
- Script: `workspace/scripts/funding/variational-prices.js`
- Run: `node workspace/scripts/funding/variational-prices.js`
- Note: Variational's mark often deviates from other exchanges (can be $200+ below) — this is REAL, not a bug

---

## 🏃 TrainingPeaks API ✅ WORKING (2026-02-04)

**Account:** wuayteck@gmail.com (bro's TrainingPeaks account)
**Athlete ID:** 5177589
**Credentials:** `/home/pujing/.openclaw/credentials/trainingpeaks.json`
**Script:** `/home/pujing/.openclaw/workspace/scripts/trainingpeaks/fetch-pmc.sh`
**Docs:** `/home/pujing/.openclaw/workspace/scripts/trainingpeaks/README.md`

**API Base:** `https://tpapi.trainingpeaks.com`
**OAuth Base:** `https://oauth.trainingpeaks.com`

**What it provides:**
- PMC (Performance Management Chart) data: CTL, ATL, TSB, TSS, IF
- Daily training load history going back 2+ years

**Quick fetch (last 180 days):**
```bash
/home/pujing/.openclaw/workspace/scripts/trainingpeaks/fetch-pmc.sh 180 /tmp/tp-pmc.json
```

**PMC Endpoint:**
```
POST https://tpapi.trainingpeaks.com/fitness/v1/athletes/5177589/reporting/performancedata/{startDate}/{endDate}
Authorization: Bearer <access_token>
Content-Type: application/json
Body: {"workoutTypes":[],"ctlConstant":42,"ctlStart":0,"atlConstant":7,"atlStart":0}
```

**Token Lifecycle:**
- Access token: expires in 1 hour (3600s)
- **Production_tpAuth cookie:** long-lived session cookie (weeks/months) — this is the key!
- Script uses cookie to fetch fresh access_token via `GET /users/v3/token`
- Fully automated: fetch-pmc.sh auto-refreshes token on 401

**Token refresh (automated):**
```bash
node /home/pujing/.openclaw/workspace/scripts/trainingpeaks/tp-login.js
# Uses Production_tpAuth cookie → fetches fresh access_token → saves to creds file
```

**If cookie expires (every few weeks/months):**
1. Bro goes to https://app.trainingpeaks.com, logs in
2. Opens DevTools (F12) → Application tab → Cookies → https://app.trainingpeaks.com
3. Finds `Production_tpAuth` cookie → double-clicks Value → copies
4. Sends to Damon → update credentials file

**Used in:** morning-wellness-deep-analysis cron (7:30 AM SGT)

---

## 📅 Google Calendar (Write Access) ✅ WORKING

**Credentials:** `/home/pujing/.openclaw/credentials/google-calendar-client.json` (OAuth client)
**Tokens:** `/home/pujing/.openclaw/credentials/google-calendar-token.json` (access + refresh tokens)
**Auto-refresh script:** `/home/pujing/.openclaw/scripts/gcal-token.sh`
**Calendar:** `damontay043@gmail.com` (primary calendar)

**Setup (2026-02-02):**
- Fresh OAuth setup via Google Cloud Console (project: damon-calendar)
- damontay043@gmail.com account owns the calendar
- Bro subscribed to damontay043@gmail.com calendar — events I create show up on his calendar

**API Usage:**
```bash
# Get fresh token (auto-refreshes if expired)
ACCESS_TOKEN=$(/home/pujing/.openclaw/scripts/gcal-token.sh)
curl -H "Authorization: Bearer $ACCESS_TOKEN" "https://www.googleapis.com/calendar/v3/calendars/primary/events"
```

**Use cases:**
- Add reminders/events for bro
- Block time for tasks
- Track important dates (TGE, snapshots, etc.)

**Note:** Access token expires in 1 hour. The gcal-token.sh script auto-refreshes using the refresh_token.

---

## 📊 Google Sheets (Read Access) ✅ WORKING (2026-02-04)

**Credentials:** Same OAuth client as Calendar (`google-calendar-client.json`)
**Tokens:** `/home/pujing/.openclaw/credentials/google-token.json` (has both `spreadsheets.readonly` + `calendar` scopes)
**Auto-refresh script:** `/home/pujing/.openclaw/scripts/gsheets-token.sh`

**Key difference from Calendar tokens:**
- `google-calendar-token.json` = calendar scope only
- `google-token.json` = calendar + spreadsheets.readonly scopes ← **use this for Sheets**

**Spreadsheet:** TDL (ID: `1Asq3P2Nb54FR_0odDavw9kPIO0s8_ITX27dZzFmGyic`)
**Sheets available:** Workouts, Wellness, Journal, TDL, Sleep, Omron, Master, Books, Supplements, KIV, Archive, Night Schedule, Trips, AI Prompts, Links for Kindle, Cardio Time

**API Usage:**
```bash
# Get fresh token (auto-refreshes if expired)
SHEETS_TOKEN=$(/home/pujing/.openclaw/scripts/gsheets-token.sh)

# Read a range from Wellness tab
SHEET_ID="1Asq3P2Nb54FR_0odDavw9kPIO0s8_ITX27dZzFmGyic"
curl -s -H "Authorization: Bearer $SHEETS_TOKEN" \
  "https://sheets.googleapis.com/v4/spreadsheets/${SHEET_ID}/values/Wellness!A1:BZ5"
```

**Use cases:**
- Morning wellness analysis: Kubios HRV, sleep scores, BP, SpO2 data
- Workout tracking: EF (Efficiency Factor) trends
- Any TDL spreadsheet data

**Used in:** morning-wellness-deep-analysis cron (7:30 AM SGT)

**If refresh_token expires (rare, ~6 months):**
1. Need to re-authorize via OAuth flow
2. Run: `python3 scripts/google_oauth.py` (or re-authorize via browser)
3. Grant both `calendar` + `spreadsheets.readonly` scopes

---

## 🏃 Garmin Connect API ✅ WORKING (2026-02-04)

**Account:** wuayteck@gmail.com (bro's Garmin account)
**Library:** `garminconnect` Python package (pip installed)
**Tokens:** `~/.garminconnect/oauth1_token.json` + `~/.garminconnect/oauth2_token.json`
**Token auto-refresh:** garminconnect library handles OAuth2 refresh automatically on login

**Python Usage:**
```python
from garminconnect import Garmin
from datetime import date, timedelta
import os

client = Garmin()
client.login(tokenstore=os.path.expanduser("~/.garminconnect/"))

yesterday = (date.today() - timedelta(days=1)).isoformat()

# Sleep data
sleep = client.get_sleep_data(yesterday)

# HRV data
hrv = client.get_hrv_data(yesterday)

# Resting heart rate
rhr = client.get_rhr_day(yesterday)

# Stress
stress = client.get_stress_data(yesterday)

# Respiratory rate
resp = client.get_respiration_data(yesterday)
```

**Available data:**
- Sleep duration, WASO, sleep score
- HRV (weekly avg, last night avg)
- Resting heart rate
- Stress scores
- Respiratory rate
- Activities/workouts

**Token lifecycle:**
- OAuth2 tokens stored in `~/.garminconnect/`
- garminconnect lib auto-refreshes on `client.login(tokenstore=...)`
- Tokens updated today (07:32 SGT) — confirmed working

**If tokens expire completely:**
- Need Garmin username + password to re-authenticate
- Ask bro for credentials, run: `Garmin(email, password).login()` then `client.garth.dump(tokenstore_path)`

**Used in:** morning-wellness-deep-analysis cron (7:30 AM SGT)

---

## 🔍 Web Search (Brave API)

**Built into Clawdbot** — use `web_search` tool

**What it does:**
- Search the web via Brave Search API
- Returns titles, URLs, snippets
- Supports region/language filters

**When to use:**
- Research questions
- Find documentation
- News/current events (non-Twitter)
- Fact-checking

**Example:**
```
web_search("moltbot timezone configuration")
web_search("Singapore AQI today")
```

---

## 🌐 Browser Automation (Chrome Relay)

**Status:** ✅ ACTIVE & VERIFIED (2026-01-31)

**What it does:**
- Control Chrome tabs on Windows via relay extension
- Take screenshots, press keys, navigate
- Discord logged in with peterpoon account

**How to use (via browser tool, NOT openclaw CLI):**
```
# 1. Find Discord tab
browser action=tabs profile=chrome target=host

# 2. Scroll to latest (End key works!)
browser action=act profile=chrome target=host targetId=[ID] request={"kind":"press","key":"End"}

# 3. Take screenshot
browser action=screenshot profile=chrome target=host targetId=[ID]

# 4. Scroll up for more context
browser action=act ... request={"kind":"press","key":"PageUp"}
```

**Requirements:**
- Chrome open on Windows with OpenClaw extension installed
- Extension attached to Discord tab (click toolbar icon, badge shows ON)
- If "no tab connected" error → ask bro to reattach extension to Discord tab

**Discord Monitoring (verified working 2026-01-31):**
- Paradex #general: `https://discord.com/channels/1107916848193863740/1107916848193863743`
- Account: peterpoon
- End key scrolls to latest messages ✅
- PageUp scrolls back for more context ✅
- Screenshots capture full chat area ✅

**When to use:**
- Discord sentiment checks (30-min pulse cron)
- Need to see logged-in content
- Any browser-based monitoring

**⚠️ CRITICAL: Use Existing Tabs, Never Open New:**
- `browser action=open` creates a NEW tab WITHOUT relay attached — DON'T USE!
- **Correct approach:**
  1. `browser action=tabs target=host profile=chrome` — list existing tabs
  2. Find the tab you need (URL contains 'discord.com')
  3. Note its `targetId`
  4. Use that `targetId` for all subsequent actions
- This ensures you're using bro's tab that already has relay enabled

**Discord Sentiment Report Format:**
```
💬 *Discord Sentiment ([HH:MM]-[HH:MM] SGT)*

*Score: XX/100 (Level)*

*Key conversations:*
• *[HH:MM]* — [Username]: "[actual quote]"
• *[HH:MM]-[HH:MM]* — [User] asks "[quote]" → [Responder]: "[quote]"
[5-10 conversations with timestamps, names, quotes]

*Vibe:* [2-3 sentences on mood/energy]
```

---

## 🗣️ TTS (Text-to-Speech)

**Provider:** Edge TTS
**Voice:** en-SG-WayneNeural (Singapore English male)

**What it does:**
- Convert text to speech audio
- Returns MEDIA: path

**How to use (CORRECT METHOD):**
1. Call `tts` tool with text → get file path
2. Call `message` tool with:
   - `filePath`: the audio path
   - `message`: emoji only (e.g. "🎤") — **NOT text, or it gets TTS'd as female voice!**
3. Reply with `NO_REPLY`

**Why emoji-only message:**
- Text captions get auto-TTS'd (female voice) = duplicate clips
- Emoji can't be TTS'd = single clip, correct voice

**When to use:**
- Bro requests audio
- Storytelling / summaries that work better as audio
- Accessibility

**Status (2026-01-31):** ✅ Verified working! Male voice (WayneNeural) confirmed correct. Use emoji-only captions to avoid duplicate clips.

---

## 📈 Dashboard (GitHub Pages)

**Repo:** damontay043/damon-dashboard
**URL:** https://damontay043.github.io/damon-dashboard/
**Local:** `/tmp/damon-dashboard/`

**What it does:**
- Public HTML dashboard showing my status, crons, capabilities
- Auto-synced from DASHBOARD.md when changed

**Workflow:**
1. Update `DASHBOARD.md` in workspace
2. Regenerate HTML: `node /root/clawd/scripts/generate-dashboard.js`
3. Git commit + push to GitHub
4. GitHub Pages serves updated dashboard

---

## 🧠 QMD Memory Search (updated 2026-02-05)

**Backend:** QMD (local BM25 text search via wrapper script)
**Wrapper:** `/home/pujing/qmd-wrapper.sh` — redirects `qmd query` → `qmd search` (BM25 only)
**Why:** Full `qmd query` uses 3 GGUF models (5.7GB RAM) — too heavy for this PC. BM25 is 0.25s.
**Index:** 212 files, 682 vectors, 4 collections (memory-root, memory-dir, sessions)

**Search strategy (escalation ladder):**
1. **`memory_search`** → QMD BM25 (keyword matching), 0.25s, local, free
2. **If 0 results:** Retry with 2-3 keyword variations (synonyms, abbreviations, related terms)
   - e.g., "funding rate" → try "APR", "funding", "annualized"
   - e.g., "TrainingPeaks" → try "TP", "PMC", "CTL"
3. **If still 0 after keyword retries:** Use `exec` to run local semantic vector search:
   ```bash
   /home/pujing/.bun/bin/qmd vsearch "semantic query here" --json -n 5
   ```
   This is slow (~12s, loads 300M embedding model) but does TRUE semantic matching.
4. **If vsearch also fails or is too slow:** Fall back to OpenAI embeddings — we have paid credits, USE THEM.
   OpenClaw's built-in OpenAI fallback can be triggered by running memory_search after temporarily erroring QMD,
   or just accept that the info isn't in memory and move on.
5. **Only after exhausting all above:** Accept "not found in memory" — don't fabricate

**IMPORTANT:** Steps 2-4 are MANUAL — I decide when to escalate based on how critical the search is. Don't auto-escalate for every search, only when I genuinely need the info. But DO use OpenAI when QMD can't deliver — we paid for those credits.

**Limitations of BM25 (step 1):**
- "funding rate" won't find "APR" (different words, same concept)
- Works great for: names, dates, specific terms, file paths, tool names
- Weak for: conceptual/abstract queries ("how did bro feel about X")
- When in doubt about keywords, try the EXACT words bro would have used

**Direct CLI access (for debugging or advanced queries):**
```bash
/home/pujing/.bun/bin/qmd search "exact keywords" --json -n 8  # BM25 (fast)
/home/pujing/.bun/bin/qmd vsearch "semantic query" --json -n 5  # Vector (slow, 12s)
/home/pujing/.bun/bin/qmd status  # Check index health
```

## ⏰ Cron Jobs (updated 2026-02-05)

**Active crons (use `cron list` to check):**

| Name | Schedule | Model | What it does |
|------|----------|-------|-------------|
| `aave-health-alert` | Every 2 min (6am-11pm) | Sonnet | Real-time health factor monitoring, 3 wallets |
| `morning-briefing` | 7:00 AM SGT | Opus | Weather, AQI, ST headlines, calendar, market ratios, Mayer Multiple, DeFi pulse, Discord, joke |
| `wallet-spy` | 7am/1pm/7pm SGT | Opus | Forensic monitoring of 12 wallets (ETH+ARB+HL) |
| `variational-funding-rates` | 7:40am/3:40pm SGT | Sonnet | ETC/ETH/SOL funding + extreme outliers on Variational |
| `hourly-pulse` | :45 past (6,10,14,18,20) | Opus | AQI, BTC+PAXG basis (4 exchanges), funding rates, Aave, peg, FUD, gold spread, Discord (Paradex+Variational) |
| `defidojo-day` | :20 past (6,10,14,18,21) | Opus | DeFi Dojo #dojo-chat daytime intel extraction |
| `nag-headline-inputs` | 9am/12/3/6/9pm SGT | Sonnet | Nag bro for headline source preferences |
| `nag-undone-tasks` | 9am/12/3/6/9pm SGT | Sonnet | Check for blocked/forgotten tasks |
| `deep-self-review` | 12am/9am/12/3/6/9pm SGT | Opus | Config compliance audit + MISS/HIT logging + cron watchdog |
| `refresh-rules-nudge` | 10am/2/6/10pm SGT | main/systemEvent | Re-read self-review rules, check compliance drift |
| `trello-q1-helper` | 1:00 PM SGT | Opus | Fetch Trello Q1 tasks, suggest how I can help |
| `hl-oi-top10` | 2:00 PM SGT | Sonnet | Hyperliquid top 10 open interest (standalone script) |
| `afternoon-research` | 3:00 PM SGT | Opus | Alternating: Twitter trends (even) / deep-dive (odd) |
| `aboutme-enrichment` | 4:00 PM SGT | Opus | Learn something new about bro, log to interview file |
| `defidojo-daily-channels` | 4:20 PM SGT | Opus | DeFi Dojo #hedge-arb + #hl-and-frenemies daily deep scan |
| `uv-index-and-stories` | 5:00 PM SGT | Opus | UV index, curated story, 3 contextual evening focus points |
| `daily-business-idea` | 6:00 PM SGT | Opus | Solopreneur business idea with analysis |
| `morning-wellness-deep-analysis` | 6:30 PM SGT | Opus | Garmin + TrainingPeaks PMC + Sheets → deep health report |
| `daily-calibration-interview` | 7:00 PM SGT | Opus | Personality/capability calibration question |
| `evening-funding-briefing` | 9:00 PM SGT | Opus | Detailed funding rates across all venues |
| `defidojo-night` | :20 past (22-5) hourly | Opus | DeFi Dojo #dojo-chat overnight intensive scan |

**Manage via:**
- `cron list` — See all jobs
- `cron add` — Add new job
- `cron update` — Modify job
- `cron remove` — Delete job
- `cron runs <jobId>` — Check recent run history

---

## 🧠 Memory System

**Files:**
- `MEMORY.md` — Long-term curated memory (main sessions only!)
- `memory/YYYY-MM-DD.md` — Daily raw notes
- `NOW.md` — Current task sticky note (survives compaction)
- `memory/heartbeat-state.json` — Track periodic check timestamps

**Rules:**
- Always search memory before answering questions about past work
- Write important things DOWN — mental notes don't survive
- Update MEMORY.md periodically with distilled insights

---

## 📱 Messaging

**WhatsApp:** Primary channel with bro
- DM policy: allowlist (only bro's number)
- Can send text, images, audio, documents
- Reactions supported

**For other platforms:** Use `message` tool with appropriate channel parameter.

---

## Summary: My Superpowers (updated 2026-02-04)

| Power | Tool | Status |
|-------|------|--------|
| Read tweets | Managed browser (profile=openclaw) | ✅ Active (account suspended but can still read) |
| Search Twitter | xAI/Grok API + Brave Search | ✅ Active |
| Web search | Brave API (`web_search` tool) | ✅ Active |
| Read bro's files | Direct `/mnt/c/` access | ✅ Always available |
| Browser control | Chrome Relay | ✅ Active |
| Discord monitoring | Chrome Relay (Paradex + Variational + DeFi Dojo) | ✅ Active |
| DeFi Dojo intel | Chrome Relay JS extraction (3 channels) | ✅ Active (2026-02-15) |
| Windows screenshots | PowerShell | ✅ Active |
| Windows clipboard | PowerShell | ✅ Active |
| Calendar write | Google Calendar API | ✅ Active (auto-refresh) |
| Google Sheets read | Sheets API | ✅ Active (auto-refresh) |
| Garmin health data | garminconnect Python | ✅ Active (sleep, HRV, HR, stress) |
| TrainingPeaks PMC | REST API + cookie auth | ✅ Active (CTL/ATL/TSB/TSS) |
| Wallet spy | Node.js script (12 wallets, ETH+ARB+HL) | ✅ Active (2026-02-08) |
| Gold token spread | PAXG (Binance) vs XAUT (Bybit) script | ✅ Active (2026-02-18) |
| Mayer Multiple | Binance 200d klines script | ✅ Active (2026-02-18) |
| Paradex data | REST API (public, no auth) | ✅ Active (funds withdrawn Feb 16) |
| Lighter data | REST + WebSocket | ✅ Active |
| Variational data | REST API | ✅ Active |
| Aave health monitoring | Node.js script (3 wallets, ETH+ARB) | ✅ Active (every 2 min) |
| BTC peg monitoring | Paraswap + Binance script | ✅ Active |
| Trello tasks | REST API | ✅ Active |
| Voice output | Edge TTS | ✅ Active |
| Scheduled tasks | 22 active cron jobs | ✅ Active |
| Memory | File-based + QMD BM25 search | ✅ Active |
| Codex CLI | Coding delegation | ✅ Active |
| Gemini CLI | Large context coding/research | ✅ Active |

---

## 🧑‍💻 Codex CLI — Delegation & Review Process

**Version:** codex-cli 0.93.0
**Path:** `/home/pujing/.npm-global/bin/codex`
**Model:** gpt-5.2-codex (default)
**Note:** Reasoning effort is NOT configurable on ChatGPT Pro — always shows "none" regardless of config

---

### Core Policies

| Policy | Rule |
|--------|------|
| **Codex-first** | Default to Codex for coding tasks; reserve Opus for tasks needing memory/context |
| **3-pass cap** | Max 3 Codex executions per task. Still broken after 3? Reassess spec/approach |
| **No secrets** | Never include secrets in prompts. Limit Codex to minimal file set |
| **Acceptance criteria** | Every delegation must include verifiable success criteria |

**Definition:** One **pass** = one Codex execution + local review/test cycle.

---

### SECTION 1: Coding Tasks

#### Decision Tree (follow in order)

1. **Security-sensitive?** (auth, secrets, crypto, permissions) → Handle directly. See Security Checklist.
2. **Needs OpenClaw tool execution?** (browser, message, cron, nodes must *run*) → Handle directly. (Codex CAN draft code that *references* these if execution isn't needed)
3. **Ambiguous spec?** → Clarify with bro first, document clarification, then delegate.
4. **Quick edit?** (<10 lines, exact location known) → Handle directly (overhead exceeds benefit).
5. **Heavy context?** (>50 lines explanation needed) → Split task or summarize context. See Prompt Hygiene.
6. **None of above?** → Delegate to Codex ✅

#### Security Checklist (for security-sensitive tasks)
- [ ] No hardcoded secrets in code
- [ ] Credentials loaded from env/config, not inline
- [ ] Auth flows use established patterns (no custom crypto)
- [ ] Input validation present
- [ ] Error messages don't leak sensitive info

#### Prompt Hygiene (for heavy context)
- **Summarize** background instead of pasting full history
- **Excerpt** only relevant portions of large files
- **Reference** file paths instead of inlining entire files
- **Split** into smaller independent tasks if possible

#### Delegation Workflow

```
1. PREFLIGHT
   - Identify target files + files to protect
   - Write acceptance criteria (REQUIRED — no delegation without this)
   - Check for secrets in target files → exclude or redact
   - Mode: --full-auto (default), or omit for interactive if complex/uncertain

2. RUN CODEX
   codex exec "<prompt>" --full-auto   # or without --full-auto for interactive
   
3. QA LOOP (max 3 passes)
   a. Review: git diff, run tests (or lint if no tests), spot-check logic
   b. Handle test issues:
      - Test fails due to Codex change → fix required
      - Test fails for unrelated reason → note it, proceed if Codex changes are correct
   c. Decide:
      - ALL GOOD → Exit to step 4
      - MINOR (<3 issues, <10 lines fix) → Fix locally, verify, exit
      - SUBSTANTIAL → Compose iteration prompt, run next pass
   d. Pass 3 still broken → Stop. Reassess spec/approach.

4. FINAL REVIEW
   - [ ] No unrelated file changes?
   - [ ] No new lint errors?
   - [ ] Acceptance criteria met?
   - [ ] Changes summarized for handoff?

5. PRESENT to bro (polished output only)
```

**Rollback:** If changes are bad: `git checkout -- <files>` or `git stash`.

#### Prompt Templates

**Initial:**
```
Create [thing] in [location].

Context: [brief summary — avoid pasting >50 lines]

Requirements:
- [functional requirements]

Constraints:
- [what NOT to do]

Acceptance criteria:
- [how to verify success — REQUIRED]

Output files:
- path/to/file.ext — [purpose]

Do NOT modify: [protected paths]

Test plan: [which tests to run, or "lint only" if no tests exist]
```

**Iteration (Pass 2+):**
```
Review your output. Issues found:
1. [Issue 1]
2. [Issue 2]

Original acceptance criteria: [copy from initial]

Fix these issues. You may add new files if needed (list them).
Only modify files related to this task.
```

#### QA Checklist
- [ ] Missing imports
- [ ] Incorrect path assumptions  
- [ ] Hardcoded values → should be parameters
- [ ] Silent failures (errors swallowed)
- [ ] Incomplete error handling
- [ ] Off-by-one errors
- [ ] Unrelated file changes
- [ ] Tests documented if skipped

---

### SECTION 2: Non-Coding Review Loop

**When to use:** Documentation, policies, analysis reports, process designs, substantial structured writing.

**Skip for:** Quick responses, real-time conversation, content requiring external/live data.

#### Review Workflow

```
1. Draft deliverable

2. Qualifies for review? (see "when to use")
   NO → Present directly
   YES → Continue

3. REVIEW LOOP (max 3 passes)
   a. Send FULL content to Codex (it can't access external files)
   b. Evaluate each feedback point:
      ✅ Accept: Factual errors, logical contradictions, missing edge cases
      ❌ Reject: Over-engineering, style-only changes, changing user conventions
   c. Revise + verify changes don't break anything
   d. Major issues remain & <3 passes? → Loop
   e. Pass 3 still problematic? → Rethink approach

4. VERIFICATION (for factual/policy claims)
   - Cross-check key claims against primary source
   - Don't rely solely on Codex review for accuracy

5. Present to bro
```

#### Review Prompt Template
```
Review the following [document type]:

---
[Full content - include everything, Codex can't access files]
---

Context: [Purpose, audience, constraints]

Evaluate:
1. COMPLETENESS — Any gaps?
2. ACCURACY — Any wrong/overstated claims?
3. CLARITY — Any ambiguous parts?
4. STRUCTURE — Logical flow?
5. ACTIONABILITY — Can reader act without confusion?

Format: Issue [#]: [Section] — [Problem] — [Fix]
```

---

### Quick Reference

| Situation | Action |
|-----------|--------|
| New script/tool | Delegate |
| Refactor existing code | Delegate |
| Security/auth code | Handle directly (use checklist) |
| Code that *references* OpenClaw tools | Delegate (if no execution needed) |
| Code that *executes* OpenClaw tools | Handle directly |
| Spec unclear | Clarify first, document, then delegate |
| Edit <10 lines | Handle directly |
| Heavy context (>50 lines) | Split/summarize, then delegate |
| Document/policy review | Use review loop |
| Quick chat response | Skip review |

---

## 🤖 Gemini CLI — Google's Coding Agent

**Version:** gemini-cli v0.26.0
**Path:** `/home/pujing/.npm-global/bin/gemini`
**Models:** Gemini 2.5 Flash (default), Gemini 2.5 Pro (preview)
**Auth:** Google OAuth (Ultra subscription)
**Cost:** No marginal cost with Google One AI Ultra

**Common commands:**
```bash
gemini                           # Interactive mode
gemini -p "prompt"               # One-shot prompt
gemini -p "prompt" -m pro        # Use Pro model
gemini /settings                 # Configure (enable preview features for Pro)
```

**Strengths:**
- 1M+ token context window (largest of the three agents)
- Strong at large codebase comprehension and cross-file analysis
- Good at multi-file refactoring where seeing whole changeset matters
- No marginal cost if you already have Google One AI Ultra

**Limitations:**
- Newer CLI, less battle-tested than Codex
- Preview features required for Pro model
- Current CLI lacks iterative test-validate loop (manual approval for writes)

---

## 🧭 Multi-Agent Routing — Codex / Gemini / Claude Code

**Finalized 2026-02-06** with inputs from Momo (Claude Code on bro's PC)

### Agent Reliability Ratings

| Agent | Reliability | Notes |
|-------|-------------|-------|
| **Codex CLI** | 85/100 | Battle-tested, iterate-test loop |
| **Gemini CLI** | 70/100 | Large context, newer CLI |
| **Claude Code** | N/A | That's me — routing to myself isn't delegation |

### Quota Hierarchy
**Preserve Claude quota > Use Codex freely > Use Gemini for specific advantages**

### Decision Tree (follow in order)

```
START — Can I delegate this?
│
├─ 1. SECURITY-SENSITIVE? (auth, crypto, secrets)
│      YES → Handle directly (no agent)
│
├─ 2. Needs OPENCLAW TOOL EXECUTION? (browser, message, cron)
│      YES → Handle directly (agents can't run OpenClaw tools)
│
├─ 3. Quick edit? (<10 lines, exact location known)
│      YES → Handle directly (overhead exceeds benefit)
│
├─ 4. Spec AMBIGUOUS?
│      YES → Clarify with bro first, then delegate
│
└─ 5. DELEGATE — Two-filter approach:

       FILTER 1: Does task require large context? (>100K tokens, many files)
       │
       ├─ YES → Gemini (can see entire changeset)
       │        BUT if iterative testing crucial → Codex may still win
       │
       └─ NO → Continue to Filter 2

       FILTER 2: What's the primary need?

       a. CORRECTNESS-CRITICAL + needs iteration/testing?
          → Codex (disciplined loop, local tool execution)
          
       b. BROAD UNDERSTANDING needed? (architecture, cross-file analysis)
          → Gemini (context window advantage)

       c. STRAIGHTFORWARD + low risk? (CRUD, boilerplate)
          → Gemini Flash (fast, no marginal cost)

       d. UNCERTAIN?
          → Codex (most battle-tested, safe default)
```

### Quick Reference Task Table

| Task Type | Agent | Reason |
|-----------|-------|--------|
| Large codebase exploration | Gemini | 1M context |
| Complex refactor (few files) | Codex | Precision + iteration |
| Complex refactor (many files) | Gemini | Cross-file visibility |
| Bug fix (localized) | Codex | Iteration + edge cases |
| Bug fix (non-local cause) | Gemini | Trace across codebase |
| Writing tests | Codex | Good at edge cases |
| Boilerplate generation | Gemini Flash | Fast, low risk |
| API integration | Codex | Error handling, retries |
| Documentation (code-tied) | Codex | Accuracy to behavior |
| Quick factual research | Gemini | Preserves Claude quota |

### Failure Escalation Path

```
AGENT FAILED 3 PASSES →
│
├─ 1. Re-evaluate the spec — well-defined? Break it down?
│
├─ 2. Analyze the failure mode:
│      ├─ Illogical output?      → Try Codex (stronger reasoning)
│      ├─ Missing context?       → Try Gemini (larger context)
│      └─ Misunderstood intent?  → Handle directly
│
├─ 3. Still failing? → Escalate to bro
│
└─ 4. Document the failure in memory/agent-failures.md
```

### Cross-Review Pattern (for high-stakes tasks)

```
1. Agent A produces initial output
2. Agent B reviews/critiques A's output
3. Revise based on B's feedback
4. Damon synthesizes and resolves conflicts
5. Present final version to bro
```

---

*Add whatever helps you do your job. This is your cheat sheet.*

## 📅 Google Calendar — Dual Calendar Setup

**Two calendars to query for complete view:**
1. `damontay043@gmail.com` (primary, owner) — events I create
2. `wuayteck@gmail.com` (reader) — bro's personal calendar, shared Feb 28

Morning briefing + any calendar queries must check BOTH calendars and merge results.

---

## 📋 Trello API

**Credentials:** `/home/pujing/.openclaw/credentials/trello.json`
**Account:** ngwuayteck (Ng Wuay Teck)

**Boards:**
- TDL (Eisenhower Matrix task board)
- Kanban Template

**API Base:** `https://api.trello.com/1/`

**Common operations:**
```bash
# Get boards
curl "https://api.trello.com/1/members/me/boards?key=${API_KEY}&token=${TOKEN}"

# Get lists on a board
curl "https://api.trello.com/1/boards/{boardId}/lists?key=${API_KEY}&token=${TOKEN}"

# Get cards on a list
curl "https://api.trello.com/1/lists/{listId}/cards?key=${API_KEY}&token=${TOKEN}"

# Create a card
curl -X POST "https://api.trello.com/1/cards?key=${API_KEY}&token=${TOKEN}&idList={listId}&name=Task Name&desc=Description"

# Move a card to another list
curl -X PUT "https://api.trello.com/1/cards/{cardId}?key=${API_KEY}&token=${TOKEN}&idList={newListId}"
```

**Use cases:**
- Add tasks to TDL board
- Move cards between Q1-Q4 based on priority
- Track completed tasks
- Create reminders
