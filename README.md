# Bitcoin Node Cost Tracker (2009–2040)

Interactive single-page dashboard exploring how affordable it has been to run a Bitcoin node over time — storage, hardware, internet, and electricity — with projections through 2040.

**Archival** models buying dedicated node hardware (Pi, NUC, mini PC) plus a drive sized to the full chain. **Pruned** assumes you run Bitcoin Core on a laptop or desktop you already own: **$0 one-time setup** by default (uses free disk space); real cost is **annual OpEx**.

## Quick start

No build step. Open `index.html` in a browser, or serve the folder locally:

```bash
python -m http.server 8080
# open http://localhost:8080
```

## Features

- **Year slider (2009–2040)** — scrub history and projected years; hero banner and hardware recommendations follow the era
- **Archival / Pruned toggle** — compare full-history nodes vs pruned validating nodes on an existing PC
- **Optional indexes** — Bitcoin Core (`txindex`, `coinstatsindex`, `blockfilterindex`) and Electrum server modes (`electrs`, Fulcrum Standard/Full). Indexes are archival-only; Fulcrum auto-enables `txindex`
- **Projections to 2040** — always on; adjust scenario (baseline, AI-shock, optimistic, pessimistic) and projection sliders from the panel
- **KPIs** — disk footprint, one-time setup, annual OpEx, hobbyist affordability score
- **Charts** — chain size, storage $/TB (effective + HDD/SSD), cost breakdown, annual cost, year pie (archival also shows one-time hardware chart; hidden in pruned mode)
- **Tech Deflation** — streamlined unit-cost index (Storage / Bandwidth / Compute; log, 2009=100), linear storage $/TB with Baseline vs AI-Shock projections to 2040, milestones, “what $100 bought,” and bridge back to the node cost model
- **Data table** — year-by-year need GB, buy TB, media, $/TB, enclosure, storage $, setup, OpEx; CSV export
- **Share link** — copies URL with year, mode, indexes, scenario, and active tab params

## Cost models

### Archival (default)

| Component | Model |
|-----------|--------|
| **Storage** | `(blocks + UTXO) × 1.15` headroom; whole-drive SKUs with era mins (0.5 → 1 TB from 2015 → 2 TB from 2019). Auto: HDD through 2020, SSD from 2021. USB enclosure only 2012–2023. 2026 NVMe spike ~$130/TB |
| **Hardware** | Era kits (`RAW.basePCCost`): spare PC → budget laptop → NUC → Pi → N100; matches hero + HW cards |
| **Indexes** | Marginal storage on top of base chain size (archival only; disabled when pruned) |
| **OpEx** | Internet attribution (% or fixed $/mo) + electricity (era-auto watts by default, or manual slider; EIA $/kWh) |

### Pruned

| Component | Model |
|-----------|--------|
| **Disk footprint** | Piecewise anchors — not a purchase cost. `<1 GB` (2010–12) → ~2.5 GB (2015) → ~5 GB (2018) → ~10 GB (2022) → ~18 GB (2026); post-2026 tracks UTXO growth |
| **Hardware** | **$0** — existing laptop/desktop |
| **Storage cost** | **$0 marginal** (default) — uses free space on your drive. Optional **USB/SD reference** in Assumptions if disk is full |
| **Indexes** | Disabled (require full block files) |
| **OpEx** | Same as archival — this is where pruned cost actually lives |

First sync still downloads the full chain (~753 GB bandwidth in 2026) in both modes.

## Projections

Open **Projections to 2040** (navbar) to tune:

- Scenario presets (Baseline, AI-Shock, Optimistic, Pessimistic) — rewrite **2027–2040 only**
- Blockchain growth (GB/yr; scenarios also scale this: optimistic ×0.85, pessimistic ×1.25, AI-shock ×1.05)
- Storage, hardware, internet, and electricity deflation
- SSD premium and Electrum index multiplier

Historical years (≤2026) are fixed. Charts shade the projected band and update cost **and** chain-size paths when you change scenario.

## URL parameters / Share view

**Share view** copies a deep link that restores the current year, archival/pruned mode, projection scenario, indexes, and any non-default assumption sliders. The address bar also stays in sync via `history.replaceState`.

| Param | Example | Description |
|-------|---------|-------------|
| `year` | `2030` | Selected year (always included when sharing) |
| `scenario` | `optimistic` | Projection scenario: `baseline`, `ai_shock`, `optimistic`, `pessimistic` |
| `tab` | `deflation` | Active tab: `table`, `charts` (default), `deflation`, `tradeoffs` |
| `pruned` | `true` | Pruned mode |
| `prunedStorage` | `usb` | Pruned storage: `marginal` (default) or `usb` |
| `electrum` | `electrs` | Electrum mode: `electrs`, `fulcrum`, `fulcrum_full` |
| `txindex`, `coinstats`, `blockfilter` | `true` | Core index toggles |
| `from`, `to` | `2015`, `2040` | Chart/table year window |
| `storage` | `hdd` | `auto` (default), `hdd`, `ssd`, `hybrid` |
| `storageMult` | `1.2` | Storage $/TB multiplier |
| `basePC` | `200` | Kit cost override ($) |
| `netMode`, `net` | `percent`, `15` | Internet attribution mode + value |
| `elec` | `0.20` | Electricity $/kWh override |
| `watts` | `40` | Manual node watts (omit = era auto) |
| `growth`, `storageDefl`, `hwDefl`, `netTrend`, `ssdPrem`, `elecDefl`, `electrumMult` | | Projection slider overrides |

## Customization

All model data lives in `index.html` inside the `RAW`, `TECH_DEFLATION`, `PRUNED_SIZE_ANCHORS`, `CORE_INDEX_OPTIONS`, and `ELECTRUM_OPTIONS` arrays at the top of the `<script>` block. Edit nominal $/TB, kit costs, chain size anchors, long-run deflation series, and projection defaults there.

## Model sense-check

See **[ASSUMPTIONS.md](./ASSUMPTIONS.md)** for the 2026 ground-truth sheet (chain size, NVMe, kit, EIA electricity, BPI broadband) and a re-verify checklist. Anchors last reviewed **2026-07-09**.

## Sources

Data synthesized from Blockchain.com, YCharts, Backblaze, [Bitcoin.org](https://bitcoin.org/en/full-node), historical HDD/SSD pricing (John C. McCallum / Our World in Data, Matt Komorowski), 2026 NVMe retail estimates (Tom's Hardware, TrendForce), EIA Electric Power Monthly, and USTelecom BPI. Approximations for illustration — not financial advice.

## License

MIT (Bitcoin.org project lineage for educational use).