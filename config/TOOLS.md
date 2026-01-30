# TOOLS.md - Local Notes

Skills define *how* tools work. This file is for *your* specifics — the stuff unique to your setup.

**This is my cheat sheet of superpowers. If I forget something exists, check here first!**

---

## 🖥️ Node Host: Scarlet2023

A paired node host runs on bro's home PC (Windows via WSL2). Gives access to his local files.

**How to access user files:**
- Use exec with `host=node` to run commands on Scarlet2023
- Files are at: `/mnt/c/pj/clawdbot-shared/`

### Read commands (use freely):
`cat`, `ls`, `head`, `tail`, `wc`, `grep`, `rg`, `find`, `stat`, `file`

### Write commands (allowed, but NOTIFY BRO of any changes):
`cp`, `tee`, `mkdir`
- **Scope:** ONLY write to `/mnt/c/pj/clawdbot-shared/` — nowhere else
- **Config sync:** When you modify your own workspace files (SOUL.md, USER.md, IDENTITY.md, TOOLS.md, AGENTS.md, HEARTBEAT.md), sync the updated copy to `/mnt/c/pj/clawdbot-shared/vps-config/`
- **Always tell bro** what you changed and why, either immediately or in the next morning briefing

### NOT allowed:
`rm`, `mv` (to destinations outside shared), any write outside `clawdbot-shared/`

**Important:**
- The node may be offline (bro's PC is off). If exec fails with connection error, tell bro his PC needs to be on.
- Only files in `clawdbot-shared/` are accessible — you cannot access the rest of the vault.

### Key Files on Node

| File | Purpose | When to read |
|------|---------|--------------|
| `aboutme_redacted.md` | Comprehensive user profile (health, wealth, wisdom, relationships) | Start of main sessions, or when you need deep user context |
| `vps-config/` | Mirror of your workspace config files | Sync here after any changes to your core files |

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

## 🌐 Browser Relay (Tailscale)

**Control URL:** `https://scarlet2023.taile92d1e.ts.net/`
**Profile:** Uses Chrome extension relay on bro's PC

**What it does:**
- Control Chrome tabs on bro's PC remotely
- Take screenshots, click, type, navigate
- Access sites that need bro's login

**Requirements:**
- Bro's PC (Scarlet2023) must be ON
- Chrome extension must be active
- Tab must be attached (click toolbar icon)

**When to use:**
- Sites that block VPS IPs
- Need to see logged-in content
- Complex web automation

**Limitation:** Browser only, not full desktop. For full desktop, would need RDP.

**Discord Scroll Fix (2026-01-30, updated):**
- Discord uses a virtualized message list that doesn't respond to End/Ctrl+End or PageDown reliably
- **BEST METHOD:** Look for "Jump to last unread message" button in snapshot, then CLICK it
- This button appears when there are unread messages and jumps directly to current
- After clicking, take another snapshot — messages will be current
- Fallback: If no jump button exists, messages may already be current

**⚠️ CRITICAL: Use Existing Tabs, Never Open New (2026-01-30):**
- `browser action=open` creates a NEW tab WITHOUT relay attached — DON'T USE for monitoring!
- **Correct approach:**
  1. `browser action=tabs target=host profile=chrome` — list existing tabs
  2. Find the tab you need (e.g., URL contains 'discord.com')
  3. Note its `targetId`
  4. `browser action=snapshot targetId=[id]` — use that specific tab
- This ensures you're using bro's tab that already has relay enabled

**Discord Sentiment Report Format (2026-01-30):**
Must follow this format — no generic "Topics: X, Y" allowed:
```
💬 *Discord Update ([HH:MM]-[HH:MM] SGT)*

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
**Voice:** en-SG-WayneNeural (Singapore English)

**What it does:**
- Convert text to speech audio
- Returns audio file that can be sent via WhatsApp

**When to use:**
- Bro requests audio
- Storytelling / summaries that work better as audio
- Accessibility

**Note:** Auto-TTS is OFF. Only triggers when explicitly requested or using `tts` tool.

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
| Read tweets | Bird CLI (`dbird`) | ✅ Active |
| Search Twitter | Bird CLI / xAI | ✅ Active |
| Web search | Brave API | ✅ Active |
| Read bro's files | Node host | 🟡 When PC on |
| Browser control | Tailscale relay | 🟡 When PC on |
| Calendar write | Google Calendar | ✅ Active |
| Paradex data | REST API | ✅ Active |
| Voice output | Edge TTS | ✅ Active |
| Scheduled tasks | Cron jobs | ✅ Active |
| Memory | File-based | ✅ Active |

---

*Add whatever helps you do your job. This is your cheat sheet.*
