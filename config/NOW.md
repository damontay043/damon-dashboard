# NOW.md — Current Task

DOING: Pulse cron upgrade — fixing basis data + adding price arb section

## Just Completed (last 1-3 items)
- ✅ Fixed `variational-prices.js` — now uses Variational's OWN mark_price + fixed Paradex parsing (results[])
- ✅ Updated pulse cron to include Lighter basis (WS script) + price arb section
- ✅ Diagnosed issues: Lighter was skipped, Variational was using wrong mark source

## In Progress
- [ ] Cross-check with Momo's funding tracker HTML (waiting for OneDrive sync)
- [ ] Update TOOLS.md with corrected Variational methodology

## Blocked On (bro owes me)
- [ ] **Momo's funding tracker HTML** — copied to OneDrive but not yet synced
- [ ] **Momo's delegation guidelines** — to review and potentially incorporate
- [ ] **Gemini Pro default decision** — shell alias approach? Or other method?

## Context
- Variational mark is genuinely ~$200-270 below other exchanges (real price discrepancy, not a bug)
- Next pulse at 08:45 SGT will be first with new format
- Scripts: workspace/scripts/funding/{lighter-prices.js, variational-prices.js, fetch-all.js}
- Refer to wife as "the wife" (no specific name/initial known)
