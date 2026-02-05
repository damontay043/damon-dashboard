# TOOLS.md - Local Notes

Skills define *how* tools work. This file is for *your* specifics — the stuff unique to your setup.

**This is my cheat sheet of superpowers. If I forget something exists, check here first!**

---

## ⚠️ DISCORD MONITORING — IMPORTANT!

**Use Chrome Relay via browser tool. DO NOT use `openclaw` CLI commands (PATH mismatch).**

```
browser action=tabs profile=chrome target=host     # Find Discord tab
browser action=act ... request={"kind":"press","key":"End"}  # Scroll to bottom
browser action=screenshot profile=chrome target=host targetId=...  # Capture
```

If you get "no tab connected" → tell bro: "Chrome tab detached, please reattach the OpenClaw extension to Discord tab"

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
| `/mnt/c/.../clawdbot-shared/aboutme_redacted.md` | Comprehensive user profile (health, wealth, wisdom, relationships) | Start of main sessions, or when you need deep user context |
| `/mnt/c/.../clawdbot-shared/vps-config/` | Mirror of my workspace config files | Sync here after any changes to core files |

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

## 🐦 Bird CLI (Twitter/X) — PRIMARY TWITTER ACCESS

**Account:** @realdamontay (Damon Tay)
**User ID:** 2017069604896722944
**Wrapper:** `/home/pujing/dbird` (auto-injects credentials)

**Credentials stored in:** `/home/pujing/dbird` wrapper script
- Uses env vars AUTH_TOKEN + CT0
- Cookies from bro's browser (updated 2026-02-03)
- May expire in ~2-4 weeks, re-grab if auth fails

**Common commands:**
```bash
/home/pujing/dbird whoami                    # Check auth status
/home/pujing/dbird read <tweet-url>          # Read a specific tweet
/home/pujing/dbird search "query" -n 10      # Search tweets
/home/pujing/dbird user-tweets @handle -n 20 # Get user's timeline
/home/pujing/dbird mentions                  # Check @realdamontay mentions
/home/pujing/dbird news --ai-only -n 10      # Trending/news
/home/pujing/dbird thread <url>              # Get full thread + replies
```

**Use cases:**
- Bro shares a tweet link → `dbird read <url>` to get full content
- Monitor Claude Code community → `dbird search "moltbot OR claude code" -n 20`
- Track specific people → `dbird user-tweets @steipete -n 10`
- Get AI/tech news → `dbird news --ai-only -n 10`

**⚠️ Do NOT tweet** — Bird CLI warns X blocks bot accounts quickly. Use for **reading only**.

**If cookies expire:** Ask bro to re-grab `auth_token` + `ct0` from x.com DevTools → Application → Cookies.

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

**Note:** Bird CLI is now primary for Twitter. Use xAI as backup or for semantic search.

---

## 📊 Paradex API

**Credentials:** `/root/clawd/.paradex-credentials.json`
- Read-only JWT token
- Can access: account info, positions, funding payments
- Cannot: place trades (read-only)

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

**Search strategy:**
1. `memory_search` now hits QMD BM25 (keyword matching) as primary — fast, local, free
2. BM25 is keyword-based: searches for exact tokens, not semantic meaning
3. **If QMD returns 0 results or low-quality matches:**
   - Try 2-3 keyword variations (synonyms, abbreviations, related terms)
   - If still nothing, try `exec` with `qmd search` directly for more control
   - As last resort: temporarily switch query to different keywords that might be in the actual text

**Limitations vs semantic search:**
- "funding rate" won't find "APR" (different words, same concept)
- Works great for: names, dates, specific terms, file paths, tool names
- Weak for: conceptual/abstract queries ("how did bro feel about X")

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
| `morning-briefing` | 7:00 AM SGT | Opus | Weather, AQI, ST headlines, calendar, market ratios (BTC/Gold/S&P500), crypto, DeFi pulse, Discord, joke |
| `morning-wellness-deep-analysis` | 7:30 AM SGT | Opus | Garmin + TrainingPeaks PMC + Google Sheets wellness data → deep health report |
| `hourly-pulse` | :45 past hour (6am-10pm) | Opus | AQI, BTC basis (4 exchanges), funding rates, price arb, Discord sentiment + screenshot |
| `trello-q1-helper` | 1:00 PM SGT | Sonnet | Fetch Trello Q1 tasks, suggest how I can help |
| `hl-oi-top10` | 2:00 PM SGT | Sonnet | Hyperliquid top 10 open interest |
| `afternoon-research` | 3:00 PM SGT | Opus | Alternating: Twitter trends (even dates) / deep-dive (odd dates) |
| `aboutme-enrichment` | 4:00 PM SGT | Opus | Learn something new about bro, suggest additions to aboutme |
| `uv-index-and-stories` | 5:00 PM SGT | Opus | UV index, curated story, 3 evening focus points |
| `token-usage-reminder` | 6:00 PM SGT | Sonnet | Daily token cost tracking (7-day cycle) |
| `daily-business-idea` | 6:00 PM SGT | Opus | Solopreneur business idea with analysis |
| `daily-calibration-interview` | 7:00 PM SGT | Opus | Personality/capability calibration question |
| `evening-funding-briefing` | 9:00 PM SGT | Sonnet | Detailed funding rates across venues |
| `deep-self-review` | Every 3h | Opus | Self-assessment, MISS/HIT logging |
| `nag-undone-tasks` | Every 3h | Sonnet | Check for blocked/forgotten tasks |
| `workstream-gemini-cli` (×2) | Every 3h | Sonnet | Track Gemini CLI setup progress |

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
| Read tweets | Bird CLI (`/home/pujing/dbird`) | ✅ Active (cookies updated 2026-02-03) |
| Search Twitter | Bird CLI + xAI/Grok backup | ✅ Active |
| Web search | Brave API (`web_search` tool) | ✅ Active |
| Read bro's files | Direct `/mnt/c/` access | ✅ Always available |
| Browser control | Chrome Relay | ✅ Active (verified 2026-01-31) |
| Discord monitoring | Chrome Relay + peterpoon | ✅ Active |
| Windows screenshots | PowerShell | ✅ Active (2026-01-31) |
| Windows clipboard | PowerShell | ✅ Active (2026-01-31) |
| Launch Windows apps | PowerShell | ✅ Active (2026-01-31) |
| Calendar write | Google Calendar API | ✅ Active (auto-refresh via gcal-token.sh) |
| **Google Sheets read** | Sheets API | ✅ Active (auto-refresh via gsheets-token.sh) |
| **Garmin health data** | garminconnect Python | ✅ Active (sleep, HRV, HR, stress) |
| **TrainingPeaks PMC** | REST API + cookie auth | ✅ Active (CTL/ATL/TSB/TSS) |
| Paradex data | REST API | ✅ Active |
| Lighter data | REST + WebSocket | ✅ Active (2026-02-03) |
| Variational data | REST API | ✅ Active (2026-02-03) |
| Trello tasks | REST API | ✅ Active |
| Voice output | Edge TTS | ✅ Active |
| Scheduled tasks | 15+ cron jobs | ✅ Active |
| Memory | File-based + memory_search | ✅ Active |
| Codex CLI | Coding delegation | ✅ Active (2026-02-02) |

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

*Add whatever helps you do your job. This is your cheat sheet.*

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
