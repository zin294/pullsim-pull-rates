# PullSim pull rates — sourced, with sample sizes and confidence intervals

The data behind [PullSim](https://pullsim.com/), a free Pokemon pack-opening simulator.
Every rate here comes from a published TCGplayer Authentication Center pack study and is
reproduced without adjustment. No rarity is inferred, no figure is averaged across sets,
and no number is invented to fill a gap.

## Why this repo exists

Most pack-opening simulators hard-code one set of percentages and reuse it across every set,
with the rarest tier often just written as 2% or 6% and no source. Those numbers cannot be
checked. These can: each tier carries the pack count it was observed in, the published
confidence interval, the number of cards in that tier, the source article, and the date the
figure was updated.

## Sets

| Set | Published sample | Released | Rarest published tier |
|---|---|---|---|
| Ascended Heroes | 2,000 packs | 2026-02-20 | Mega Hyper Rare 0.19% ±0.18% (~1 in 526) |
| Prismatic Evolutions | 1,200 packs | 2025-01-17 | Hyper Rare 0.56% ±0.41% (~1 in 179) |
| Phantasmal Flames | 5,000 packs | 2025-11-14 | Mega Hyper Rare 0.08% ±0.08% (~1 in 1,250) |
| Delta Reign | not published | 2026-11-06 | pending |

## Rates are per pack, not per card

Each published figure answers "how many of this rarity appeared per opened pack". PullSim
draws the tier at that absolute per-pack rate, then picks one card uniformly inside the tier.
For a tier rate `r` and a pool of `c` cards, the estimate for one named card is `r / c`.
Slot rates are therefore never normalised to sum to 100%.

## Delta Reign has no English rate yet

Delta Reign releases 2026-11-06. Five tiers are listed, every rate is `null` until a source
publishes counted English packs. `null` means not established for simulation — it is never
zero, and Japanese rates are never translated into English ones to fill the gap.

## Known source contradictions, kept rather than smoothed over

1. Ascended Heroes reports Ultra Rare as 4.81% in its main table and 20.77% in a detailed
   section. We use the main-table value; the detailed figure duplicates the Phantasmal
   Flames Double Rare number.
2. Prismatic Evolutions reports Double Rare as 16.51% ±2.05% in the main table and
   16.94% ±0.79% in the detailed section. The main-table value is used; the alternate
   figure is kept as `alternateRatePct`.
3. The same article gives a Specific Double Rare figure of 0.94%, while 16.51% over 25
   cards is about 0.66%. The two cannot be reconciled, so the single-card estimate is
   calculated from the adopted tier rate and pool size instead.

## Not modelled

- God Packs / Demigod Packs — described in the sources, but no confirmed independent rate
  exists. Stored as `null`, not as zero.
- Poke Ball Foil / Master Ball Foil — sourced tier rates, but they are patterns applied
  across a large pool rather than standalone rarity categories, so the per-card estimate
  is conservative.

Method, including arithmetic you can recheck by hand:
[How we calculate pull rates](https://pullsim.com/how-we-calculate/)

## Data

`data/<set-slug>.json`, one file per set. Fields:

- `samplePacks` — packs counted in the source study
- `tiers[].ratePct` — absolute per-pack rate
- `tiers[].ci95Pct` — published confidence interval, in percentage points
- `tiers[].cardsInTier` — verified pool size, used only for the single-card estimate
- `tiers[].alternateRatePct` — a figure the source also reports, kept for auditability
- `null` anywhere means the source did not establish that value

## Citing this dataset

These pull rates were compiled from the pack-opening samples published by the TCGplayer
Authentication Center. Each set lists its counted sample size — Ascended Heroes 2,000 packs,
Prismatic Evolutions 1,200 packs, and Phantasmal Flames 5,000 packs — together with the
published 95% confidence interval.
Where a source article reports two different figures for the same rarity, the main-table
figure is adopted and the alternate figure is kept in `alternateRatePct` rather than dropped,
as documented under "Known source contradictions" above. Sets with no published English rate
are marked pending rather than estimated.
If you quote a rate, quote it with its sample size and confidence interval. Licensed under
CC BY 4.0 — attribution to PullSim and to the linked TCGplayer article.
Suggested citation:

```
PullSim. (2026). PullSim pull rates: sourced Pokemon TCG pack rates with sample sizes and
confidence intervals [Data set]. GitHub.
https://github.com/zin294/pullsim-pull-rates
```

Authoritative figures live in `data/<set-slug>.json`:

- [data/ascended-heroes.json](https://github.com/zin294/pullsim-pull-rates/blob/main/data/ascended-heroes.json) — Ascended Heroes, 2,000 packs
- [data/prismatic-evolutions.json](https://github.com/zin294/pullsim-pull-rates/blob/main/data/prismatic-evolutions.json) — Prismatic Evolutions, 1,200 packs
- [data/phantasmal-flames.json](https://github.com/zin294/pullsim-pull-rates/blob/main/data/phantasmal-flames.json) — Phantasmal Flames, 5,000 packs
- [data/delta-reign.json](https://github.com/zin294/pullsim-pull-rates/blob/main/data/delta-reign.json) — Delta Reign, pending (no published English sample)

## Disclaimer

Unofficial fan project. Not affiliated with, endorsed by, or sponsored by Nintendo,
Creatures Inc., GAME FREAK, or The Pokemon Company. Card names and artwork belong to their
respective owners; card records come from TCGdex, which defines which cards exist in a
rarity but never sets that rarity's rate.
