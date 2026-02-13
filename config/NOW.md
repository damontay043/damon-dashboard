# NOW.md — Current Task

DOING: Google auth re-auth — waiting for bro to paste OAuth code

## Just Completed (last 1-3 items)
- ✅ Liquidation price calculator built + added to hourly pulse (working in production)
- ✅ HL OI Top 10 upgraded with 24h avg funding + ×8760 fix
- ✅ 5pm wind-down cron updated — focus points now contextual, not generic

## Blocked On (if any)
- [ ] **Google auth re-auth — IN PROGRESS** — OAuth URL sent to bro, waiting for auth code paste
- [ ] Headlines source config — bro hasn't replied (nagged 5x/day since ~Feb 9)
- [ ] Twitter cookie refresh — bro hasn't replied
- [ ] Hourly-pulse screenshot reliability — need to fix cron prompt (cron ID: 91f41b4b)

## Paused (by bro's instruction)
- paradex-liquidity-monitor — paused per bro

## Google Auth Re-auth Steps (resume after compaction)
1. ✅ Generated OAuth URL, sent to bro
2. ⏳ Bro opens URL, signs in as damontay043@gmail.com, pastes auth code
3. Exchange code: `curl -X POST https://oauth2.googleapis.com/token` with client_id, client_secret, code, redirect_uri=http://localhost, grant_type=authorization_code
4. Save tokens to `/home/pujing/.openclaw/credentials/google-token.json`
5. Verify: run `gcal-token.sh` and `gsheets-token.sh`
6. If working: disable nag-google-reauth cron (ID: 78f36a81-632e-4f4b-9b61-90b23d71da83)

## Context
- Feb 13 morning, BTC ~$66.2k (down from $68k yesterday)
- Funding rates flipped positive (~10% APR across venues)
- 21 crons healthy (deep-self-review self-healed timeout to 300s overnight)
- Aboutme.md front-load-verdict — bro declined ("no need")
- Biz idea cron updated to calibrate to bro's technical level
