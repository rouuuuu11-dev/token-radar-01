# token-radar-01

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/rouuuuu11-dev/token-radar-01/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/rouuuuu11-dev/token-radar-01/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

A radar for pre-token protocols, ranked by **implied value**: what a token would be worth if it traded at the multiple its own category's live tokens trade at. The board covers all 40 watchlist protocols plus 186 early pre-token rows below the $50,000,000 floor (226 rows), each with a 7-step farm plan with costs and times. Live-token protocols are kept apart and shown at their market cap, never a model value.

## The model

```
implied value = TVL × category multiple × growth × maturity × chain breadth
```

Every factor comes from the snapshot's own figures. No protocol name is ever special-cased; the same function scores every row.

| Factor | Formula |
|---|---|
| Category multiple | Upper median of `mcap_usd / tvl_usd` over the snapshot's live-token protocols in the category (needs ≥ 3, else the global multiple). Clamped to [0.05, 1.50]; 0.25 when the snapshot carries no live-token multiple at all. |
| Growth | `1 + clamp(change_7d, −60%, +120%) × 0.5` — a hot week counts for half its face value. |
| Maturity | `0.6 + min(age_days, 1095) / 1095 × 0.4` — older with no token is more proven, but age alone never carries a row. Unknown age counts as 365 days. |
| Chain breadth | `1 + 0.05 × min(chains − 1, 4)` — more chains means more users to launch to. |

Category multiples are read off this snapshot's 128 live-token protocols with a usable ratio. Global multiple: **0.223×**. Farm plans derive their two concrete actions from the protocol's category words and chains (DEX: LP + swap; Lending: supply + borrow; Bridge: bridge both ways; Restaking: stake + restake; and so on for 20+ category families). Capital tiers by TVL ($250 / $150 / $100 / $50); hold period by age (60 / 45 / 30 days).

## The dashboard

`dashboard/` is a single-page app with no network: it reads `data.js` and recomputes every row's implied value and farm plan in the page, checking them against `board.json` (the detail drawer shows "recomputed in this page — matches board.json"). Sortable columns, filters (search, category, chain, minimum TVL, watchlist-only), a pre-token / live-token tab, a detail drawer per protocol, and a "How the model works" section with the formula and the category multiples table.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input ../input/snapshot.json
```

Node 22+ runs `main.ts` directly. This writes `out/board.json`, `out/data.js`, `out/dashboard/` (3 files), `out/screenshots/` (3 images) and `out/dossier.json`.

---

<sub>Entered on [make.fun](https://make.fun/?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
