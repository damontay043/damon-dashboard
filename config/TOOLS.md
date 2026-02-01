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
**Wrapper:** `dbird` (auto-injects credentials)

**Credentials stored in:** `/usr/local/bin/dbird` wrapper script
- Uses env vars AUTH_TOKEN + CT0
- Cookies from bro's browser (may expire, re-grab if needed)

**Common commands:**
```bash
dbird whoami                    # Check auth status
dbird read <tweet-url>          # Read a specific tweet
dbird search "query" -n 10      # Search tweets
dbird user-tweets @handle -n 20 # Get user's timeline
dbird mentions                  # Check @realdamontay mentions
dbird news --ai-only -n 10      # Trending/news
dbird thread <url>              # Get full thread + replies
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

## 📅 Google Calendar (Write Access)

**Credentials:** `/root/clawd/.google-calendar-token.json`
**Config:** `/root/clawd/.damon-calendar-config.json`

**Shared calendar:** "Damon" (357e0589...@group.calendar.google.com)
- Bro created it, shared with damontay043@gmail.com
- I have writer access — can create/edit/delete events
- Events appear on bro's calendar instantly, no approval needed

**Use cases:**
- Add reminders/events for bro
- Block time for tasks
- Track important dates (TGE, snapshots, etc.)

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

## ⏰ Cron Jobs

**Active crons (use `cron list` to check):**
- `morning-briefing` — 7am SGT: Weather, headlines, calendar
- `30min-pulse` — Every 30 min: AQI, BTC basis, Discord sentiment
- `uv-index-and-stories` — 5pm SGT: UV + curated stories
- `evening-funding-briefing` — 9pm SGT: Funding rates
- `afternoon-research` — 3pm SGT: Research report
- `nag-undone-tasks` — Every 3h: Blocked tasks reminder

**Manage via:**
- `cron list` — See all jobs
- `cron add` — Add new job
- `cron update` — Modify job
- `cron remove` — Delete job

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

## Summary: My Superpowers

| Power | Tool | Status |
|-------|------|--------|
| Read tweets | fxtwitter API | ✅ Active (use `api.fxtwitter.com`) |
| Search Twitter | ❌ Need Brave API key | 🔧 Not configured yet |
| Web search | Brave API | 🔧 Need API key configured |
| Read bro's files | Direct `/mnt/c/` access | ✅ Always available |
| Browser control | Chrome Relay | ✅ Active (verified 2026-01-31) |
| Discord monitoring | Chrome Relay + peterpoon | ✅ Active |
| **Windows screenshots** | PowerShell | ✅ NEW! (2026-01-31) |
| **Windows clipboard** | PowerShell | ✅ NEW! (2026-01-31) |
| **Launch Windows apps** | PowerShell | ✅ NEW! (2026-01-31) |
| **Process monitoring** | PowerShell | ✅ NEW! (2026-01-31) |
| Calendar write | Google Calendar | 🔧 Check if migrated |
| Paradex data | REST API | 🔧 Check if migrated |
| Voice output | Edge TTS | ✅ Active |
| Scheduled tasks | Cron jobs | ✅ Active |
| Memory | File-based | ✅ Active |

---

*Add whatever helps you do your job. This is your cheat sheet.*
