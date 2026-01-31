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
