# PowerGridIQ Rating dataset (headline)

Free, redistributable **headline ratings** for **72 global power markets and sub-regions**, for deciding where to site or when to schedule a large new electricity load (data centers, AI compute clusters, heavy industry).

This is the open headline slice of the ratings behind [PowerGridIQ](https://powergridiq.com). Each market carries a tier, a reliability-adjusted PGIQ Rating, a five-pillar Fundamentals score, an outlook, a confidence level, and an overall reliability level. The rating is a proprietary, directional screen; evidence is drawn from public operator and regulator sources, and some inputs are modelled.

## What this file is, and is not

This file contains the **headline** ratings only, and you are free to redistribute it under CC BY 4.0. It deliberately does **not** contain:

- the five individual pillar sub-scores (access, availability, cost, momentum, carbon),
- the detailed reliability reads (the per-flag transmission, distribution and forward-capacity assessments with their cited sources),
- the point-in-time history,
- the interconnection-queue detail.

Those are proprietary. They are available through the API for evaluation (not for redistribution) and under a commercial license for production use. Contact hello@powergridiq.com.

## The rating, in one line

`fundamentals` is the five-pillar score. `pgiq_rating` is the reliability-adjusted rating (`fundamentals` minus a reliability dock). Tier 1 (Prime) is relative: assessed, no severe reliability flag, and within a few points of the current leaders. Current Tier 1: **Sweden North, United Arab Emirates, Norway.** Country overviews are excluded; sub-regions are the rated rows.

## Files

- `pgiq-ratings.csv`: one row per rated market, flat headline columns.
- `pgiq-ratings.json`: the same, with dataset metadata and the full license text.

## Columns

`id`, `name`, `group`, `parent` (blank unless a sub-region), `tier` (1 Prime to 5 Largely closed), `tier_name`, `pgiq_rating` (reliability-adjusted, 0-100), `fundamentals` (five-pillar score, 0-100), `reliability_level` (overall: none / watch / elevated / severe / not_assessed), `reliability_dock` (points removed by the reliability overlay), `reliability_assessed`, `reliability_confidence`, `outlook`, `under_review`, `report_url`, `as_of`.

## Load it

```python
import pandas as pd
df = pd.read_csv("pgiq-ratings.csv")

# Prime markets, reliability-adjusted
print(df[df.tier == 1][["name", "pgiq_rating", "fundamentals", "reliability_level"]])

# Biggest reliability adjustments
print(df.sort_values("reliability_dock", ascending=False)[["name", "fundamentals", "pgiq_rating", "reliability_level"]].head())
```

## License and attribution

The headline ratings in this file (tier, PGIQ Rating, Fundamentals score, outlook, confidence, overall reliability level) are licensed **Creative Commons Attribution 4.0 (CC BY 4.0)**: free to use, share, adapt and redistribute, including commercially, with credit to **PowerGridIQ Rating (https://powergridiq.com)**.

The pillar sub-scores, detailed reliability reads, point-in-time history and interconnection-queue detail are **not** covered by this license. They are proprietary and available via the API under a separate commercial license (evaluation use only, not for redistribution).

## Note for earlier users

Earlier versions of this dataset included the five pillar sub-scores and the full reliability reads. As of this version the freely-redistributable file is scoped to the headline fields above. If your code read the individual pillar columns from this file, those now come from the API under the commercial license rather than from the open file.

## Source and updates

Maintained from [powergridiq.com](https://powergridiq.com). Live JSON: `https://powergridiq.com/data/pgiq-ratings.json`. Methodology: [powergridiq.com/methodology](https://powergridiq.com/methodology). Full API and MCP server: [powergridiq.com/build](https://powergridiq.com/build). Ratings change over time; the `as_of` field records the refresh date.
