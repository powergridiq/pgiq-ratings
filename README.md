# PowerGridIQ Rating dataset

Tiered ratings and five-pillar scores for **67 global power markets**, for deciding where to site or when to schedule a large new electricity load (data centers, AI clusters, industrial load).

This is the open dataset behind [PowerGridIQ](https://powergridiq.com). The rating is a proprietary, directional screen; the underlying pillar evidence is drawn from public operator and regulator sources, and some inputs are modelled.

## Files

- `pgiq-ratings.csv` — one row per market, flat columns (easiest for spreadsheets and pandas).
- `pgiq-ratings.json` — the same data with nested pillar scores and dataset metadata (name, as_of, updated, license, count).

## Schema

Each market has:

| Field | Meaning |
| --- | --- |
| `id` | Stable market slug (e.g. `ercot`, `quebec`, `sweden`). |
| `name` | Display name. |
| `group` | Region group (`us`, `canada`, `europe`, `middle_east`, `latam`, `asia`, `oceania`, `africa`). |
| `tier` | 1 (Prime) to 5 (Largely closed). |
| `tier_name` | Text label for the tier. |
| `score` | Overall PGIQ score, 0 to 100. |
| `outlook` | Positive, Stable, or Negative. |
| `confidence` | Confidence in the read (Low to High). |
| `under_review` | Whether the market is currently under rating review. |
| `access`, `availability`, `cost`, `momentum`, `carbon` | The five pillar scores, 0 to 100. |
| `report_url` | Link to the full market report with evidence and sources. |

Default pillar weights: Access 30, Availability 25, Cost 25, Momentum 15, Carbon 5. A higher pillar score is better (cheaper power, more available capacity, easier access, more revealed build, cleaner grid).

## License

Released under **Creative Commons Attribution 4.0 (CC BY 4.0)**. You may use, share, and adapt the data, including commercially, as long as you credit the source.

Attribute as: **PowerGridIQ Rating (https://powergridiq.com)**.

## Example

```python
import pandas as pd
df = pd.read_csv("pgiq-ratings.csv")

# Best carbon-light markets: high carbon pillar, workable tier or better
clean = df[(df.tier <= 2)].sort_values("carbon", ascending=False)
print(clean[["name", "tier", "score", "carbon"]].head(10))
```

## Source and updates

Maintained from [powergridiq.com](https://powergridiq.com). A live JSON copy is served at `https://powergridiq.com/data/pgiq-ratings.json`, and a full read-only API (ratings, live grid signals, best-market decisions) is documented at [powergridiq.com/build](https://powergridiq.com/build). Ratings change over time; the `updated` field in the JSON records the last refresh.
