---
layout: page
title: How to Comp Cards
permalink: /guides/comping/
---

Aim: find fair market value using recent sold listings and similar conditions.

Comp method (fast)
- Start with solds: use eBay “Sold items” + “Completed” filters.
- Exact match: include Year + Brand + Player + Parallel (e.g., “2024 Topps Elly De La Cruz Rainbow Foil”).
- Filter condition: Raw vs PSA 10/9, or exclude graded with “-PSA -BGS -SGC” for raw comps.
- Sort recent: newest completed first to reflect current market.
- Exclude outliers: auctions with shill signs, lots, mislabels.
- Take a range: the middle cluster of 3–10 sales is your comp band.

Helpful filters/terms
- Sold/Completed params: `LH_Sold=1&LH_Complete=1`
- Raw only: add `-PSA -BGS -SGC` to the search query
- Graded only (PSA 10): add `PSA 10 -BGS -SGC`
- Auction only: `_sacat=0&LH_Auction=1`
- BIN only: `LH_BIN=1`

Cross‑checks
- Look for 130point screenshots or verify with multiple sales dates.
- Check active listings (ask vs comp spread).

Decision
- If grading: use PSA 9/10 comps minus fees to decide submission.
- If selling raw: price slightly above comp midpoint; watch best offers.

Try these Tools
- [Comp Builder]({{ '/tools/comps/' | relative_url }}) — one‑click eBay searches (sold/active, filters).
- [Grade ROI Calculator]({{ '/tools/grade-roi/' | relative_url }}) — EV of grading vs selling raw.
