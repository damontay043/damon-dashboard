# CRONS.md — Active Scheduled Jobs

*Auto-synced from OpenClaw gateway. Last updated: 2026-02-01*

## Daily Schedule (SGT)

| Time | Job | Description |
|------|-----|-------------|
| 07:00 | morning-briefing | Weather, AQI, headlines, market ratios, crypto movers |
| :45 (6am-10pm) | hourly-pulse | AQI, BTC basis, funding rates, Discord sentiment |
| 14:00 | hl-oi-top10 | Hyperliquid top 10 OI report |
| 15:00 | afternoon-research | AI/crypto news deep-dive |
| 16:00 | aboutme-enrichment | Suggest additions to aboutme file |
| 17:00 | uv-index-and-stories | UV index, curated story, evening focus points |
| 21:00 | evening-funding-briefing | Detailed funding rates across venues |
| Every 3h | nag-undone-tasks | Check for stuck/forgotten tasks |

---

## Job Details

### morning-briefing
- **Schedule:** 7:00 AM daily
- **Content:** Weather, AQI (3 regions), calendar, ST headlines, market ratios (BTC/Gold), crypto overnight movers, Discord sentiment, fun fact
- **Timeout:** 300s

### hourly-pulse (formerly 30min-pulse)
- **Schedule:** Every hour at :45, 6am-10pm SGT
- **Content:** AQI, BTC basis spread (HL + Paradex), funding rates, Discord sentiment
- **Includes:** Discord screenshot attachment for verification
- **Timeout:** 180s

### hl-oi-top10
- **Schedule:** 2:00 PM daily
- **Content:** Top 10 Hyperliquid perps by open interest (main + HIP-3)
- **Timeout:** default

### afternoon-research
- **Schedule:** 3:00 PM daily
- **Content:** AI/crypto news, Twitter trends, one deep-dive topic
- **Timeout:** 180s
- **Delivery:** Best-effort (skip if nothing notable)

### aboutme-enrichment
- **Schedule:** 4:00 PM daily
- **Content:** Suggest one thing learned about bro to add to aboutme file, or ask one question
- **Timeout:** 120s

### uv-index-and-stories
- **Schedule:** 5:00 PM daily
- **Content:** UV index, curated story, 3 evening focus points
- **Timeout:** 180s

### evening-funding-briefing
- **Schedule:** 9:00 PM daily
- **Content:** Funding rates (BTC/ETH/SOL) across HL, Paradex, Binance; spreads; arb opportunities
- **Timeout:** 180s

### nag-undone-tasks
- **Schedule:** Every 3 hours
- **Content:** Check NOW.md and daily memory for stuck tasks
- **Timeout:** 60s
- **Delivery:** Best-effort (silent if nothing blocked)

---

## Notes

- All times are Singapore Time (UTC+8)
- Hourly pulse pauses overnight (11pm-6am) per bro's request
- Discord sentiment checks use Chrome Relay (browser automation)
- Jobs run in isolated sessions to avoid polluting main chat history
