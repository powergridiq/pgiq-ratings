---
license: cc-by-4.0
language:
  - en
pretty_name: PowerGridIQ Rating dataset (v2, with reliability overlay)
tags:
  - energy
  - electricity
  - power-grid
  - data-center
  - reliability
  - carbon
  - cost
  - ai-infrastructure
  - site-selection
size_categories:
  - n<1K
configs:
  - config_name: default
    data_files: pgiq-ratings.csv
---

# PowerGridIQ Rating dataset (v2)

Tiered ratings and five-pillar scores, now with a **reliability overlay**, for **72 global power markets and sub-regions**, for deciding where to site or when to schedule a large new electricity load (data centers, AI clusters, industrial load).

This is the open dataset behind [PowerGridIQ](https://powergridiq.com). The rating is a proprietary, directional screen; pillar evidence is drawn from public operator and regulator sources, and some inputs are modelled.

## v2: reliability overlay and two scores

- `fundamentals` is the five-pillar score (Access, Availability, Cost, Momentum, Carbon).
- `pgiq_rating` is the reliability-adjusted rating: `fundamentals` minus a cited reliability dock for delivery-reliability and forward firm-capacity risk.
- Tier 1 (Prime) is relative. Current Prime markets: **Sweden North, United Arab Emirates, Norway.**
- Sub-regions are rated where a country's internal divergence is decision-relevant (Germany, Japan, Sweden, Australia); country overviews are excluded.

## Columns

`id`, `name`, `group`, `parent`, `tier`, `tier_name`, `pgiq_rating`, `fundamentals`, `reliability_dock`, `reliability_assessed`, `reliability_confidence`, `delivery_transmission`, `delivery_distribution`, `forward_capacity`, `outlook`, `under_review`, `access`, `availability`, `cost`, `momentum`, `carbon`, `report_url`, `as_of`.

## Load it

```python
from datasets import load_dataset
ds = load_dataset("PGIQ/pgiq-ratings")
df = ds["train"].to_pandas()
print(df[df.tier == 1][["name", "pgiq_rating", "fundamentals", "reliability_dock"]])
```

## License and attribution

Creative Commons Attribution 4.0 (CC BY 4.0). Free to use, share, and adapt, including commercially, with credit to **PowerGridIQ Rating (https://powergridiq.com)**.

## Source and updates

Maintained from [powergridiq.com](https://powergridiq.com). Live JSON: `https://powergridiq.com/data/pgiq-ratings.json`. Methodology: [powergridiq.com/methodology](https://powergridiq.com/methodology). The `as_of` field records the refresh date.
