# NOW.md — Current Task

DOING: idle

## Just Completed (last 1-3 items)
- ✅ TrainingPeaks FULLY AUTOMATED — cookie-based token refresh working end-to-end
- ✅ 7:30 AM wellness cron updated with PMC data analysis
- ✅ All docs updated (TOOLS.md, README, memory, credentials)

## Context
- TP auth uses Production_tpAuth session cookie (lasts weeks/months)
- Cookie stored in credentials file, used by tp-login.js to fetch fresh access_tokens
- Full pipeline: cookie → /users/v3/token → access_token → PMC API → CTL/ATL/TSB data
