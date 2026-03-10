# NOW.md — What Am I Doing?
*Updated: 2026-03-10 15:24 SGT*

## Active
- **Pulse infrastructure overhaul** — pre-compute script + current rates section + corrected annualization
- **Proposed: current-rates.js** — move annualization out of LLM into script. Bro receptive, pending go-ahead.
- **Monitor test pulse run** — may still have old Paradex formula. Next scheduled: 18:45 SGT.
- **Lighter collateral deficit** — equity $256K vs needed $302K = -$45K deficit. Flagged.

## Just Completed
- Flipped 5 crons to `delivery: none` (relay via heartbeat now)
- Added current funding rates to pulse (HL + Paradex + Variational + Lighter)
- Fixed Paradex annualization (8hr not hourly) — 3rd instance of this MISS class
- Added funding rate reference table to TOOLS.md
- Logged MISS to self-review.md

## Blockers
- None

## Key Context
- BTC current HL rate +4.9% (vs 1d avg -4.9%) — regime may be reverting
- Context was 92% pre-compaction
- All 5 report crons now `delivery: none` → heartbeat relay handles full reports
