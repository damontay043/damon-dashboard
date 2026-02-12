# NOW.md — Current Task

DOING: idle — approaching second compaction (~89%)

## Just Completed (last 1-3 items)
- ✅ HL OI Top 10 UPGRADED — added 24h avg funding (fundingHistory endpoint) + fixed ×8760 formula. Key: HIP-3 coins need `xyz:` prefix for fundingHistory.
- ✅ HL OI APR bug fixed — was 8x off (hourly vs 8-hour). MU was 8.7% → actual 69.6%.
- ✅ nag-twitter-cookies timeout bumped 30s → 60s

## Blocked On (if any)
- [ ] Headlines source config — bro hasn't replied (nagged 5x/day since ~Feb 9)
- [ ] Twitter cookie refresh — bro hasn't replied
- [ ] Google auth expired (Sheets + Calendar) — bro hasn't replied
- [ ] Aboutme.md front-load-verdict addition — offered, awaiting bro's ok
- [ ] Hourly-pulse screenshot reliability — need to fix cron prompt to prevent reuse of stale screenshot paths (cron ID: 91f41b4b)
- [ ] Damon audit with Momo — was scheduled today (Feb 12), bro didn't initiate

## Paused (by bro's instruction)
- paradex-liquidity-monitor — paused per bro

## Context
- Second compaction imminent (89%)
- 19 crons all healthy (nag-twitter-cookies timeout fixed by self-review cron)
- Paradex Discord sentiment spiked to 78/100 — SPOT TESTNET ANNOUNCED 🔥
- BTC ~$67.3k, HL funding negative all day (-2% to -17% APR), Paradex positive (~10%)
- CD2 longs still underwater (-$77.8k), fab3 PAXG short offsetting (+$87.5k)
- WhatsApp gateway had several brief disconnects today (all auto-reconnected)
