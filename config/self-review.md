# Self-Review Log

*Where I got it right (HIT) and wrong (MISS). Read on boot to avoid repeating mistakes.*

---

## 2026-01-31

### MISS | confidence
**What happened:** Reported Paradex basis as -$155 when it was actually -$17
**Root cause:** Used Binance spot as index instead of Paradex's own `underlying_price`
**Fix:** Always use each exchange's native index for basis calculation. Don't assume all perps track Binance.

### MISS | speed  
**What happened:** 30-min pulse cron wasn't delivering to WhatsApp for hours
**Root cause:** Updated cron payload but forgot to include `deliver: true, channel: "whatsapp"`
**Fix:** When updating cron payloads, always verify delivery settings are preserved. Check `cron runs` output if delivery seems broken.

### MISS | confidence
**What happened:** Reported Hyperliquid funding as "8hr rate" when it's actually 1hr
**Root cause:** Assumed all perp DEXs use 8hr funding like Binance
**Fix:** Don't assume funding intervals. HL = 1hr, Paradex = 8hr. Verify each venue's docs.

### HIT | depth
**What happened:** Correctly diagnosed cron delivery issue by checking run history
**Why it worked:** Used `cron runs` to see jobs were executing but not delivering. Led straight to the missing `deliver: true` fix.
**Lesson:** When crons seem broken, check run history first before assuming schedule issues.

### MISS | uncertainty
**What happened:** Asked bro for GitHub PAT instead of checking my own files first
**Root cause:** Assumed credentials were missing without doing `memory_search` or checking TOOLS.md, MEMORY.md, .credentials.json
**Fix:** Before asking bro for any credential/config info: 1) memory_search first 2) grep relevant config files 3) check .credentials.json. Only ask if genuinely not found.

### MISS | depth
**What happened:** 18:45 pulse cron showed "ok" but only output partial summary, never delivered full report
**Root cause:** Unknown — cron marked success but agent stopped mid-generation. Only got "Preparing pulse report..." then nothing.
**Fix:** Need to investigate further. May need to check cron agent logs or add explicit "report complete" marker. For now, monitor next few runs to see if pattern repeats.

---

## 2026-02-02

### MISS | speed
**What happened:** Missed 4 tweet links sent earlier; replied without reviewing them.
**Root cause:** Didn’t scan queued/backlog messages before responding; only saw latest message.
**Fix:** Before replying, always scan queued/backlog for additional links/tasks and acknowledge all items in one batch. If unsure, ask “any other links?” before closing.

### MISS | depth
**What happened:** Sent an ad-hoc Discord sentiment check without using the standard report format.
**Root cause:** Rushed the response and didn’t follow the format in TOOLS.md.
**Fix:** Always use the standard Discord Sentiment Report Format for any sentiment check, including ad-hoc tests.

### MISS | depth
**What happened:** Hourly pulse kept failing after 12:45 because the cron run hit missing dependencies (python) and timed out before delivery.
**Root cause:** Didn’t verify available tooling + timeout headroom after patching; failed to monitor the next run.
**Fix:** Avoid non-installed tools in cron instructions and increase timeouts when runs approach limits; always verify the next scheduled run after a fix.

### MISS | depth
**What happened:** 3pm research cron failed because dbird binary was missing.
**Root cause:** Assumed dbird still installed after migration; no dependency check.
**Fix:** Verify external CLI dependencies (dbird, python, rg) before relying on them; add fallback paths.

---

## Format Reference

```
### MISS | [confidence | uncertainty | speed | depth]
**What happened:** [brief description]
**Root cause:** [why it went wrong]
**Fix:** [what to do differently]

### HIT | [tag]
**What happened:** [brief description]
**Why it worked:** [what made it successful]
**Lesson:** [what to remember]
```
