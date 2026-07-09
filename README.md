# Bitcoin Node Cost Tracker (2009–2040)

Interactive single-page dashboard exploring how affordable it has been to run a Bitcoin node over time — storage, hardware, internet, and electricity — with projections through 2040. A **Tech Deflation** tab puts those costs in multi-decade context (storage, bandwidth, and compute unit prices fell by orders of magnitude, despite periodic spikes).

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
- **Tabs** — **Charts** (node cost history), **Tech Deflation** (multi-decade unit costs — see below), **Data Table**, **Trade-offs**
- **Charts** — chain size, storage $/TB (effective + HDD/SSD), cost breakdown, annual cost, year pie (archival also shows one-time hardware chart; hidden in pruned mode)
- **Data table** — year-by-year need GB, buy TB, media, $/TB, enclosure, storage $, setup, OpEx; CSV export
- **Share link** — copies URL with year, mode, indexes, scenario, and active tab params

## Tech Deflation tab

Educational context for the node cost story: over decades, **unit costs** of storage, bandwidth, and compute fell by orders of magnitude, even when prices spike for a while (Thailand floods, AI/NAND demand). Open via the tab bar or `?tab=deflation`.

| Piece | What it shows |
|-------|----------------|
| **KPI cards** | Headline facts for storage (~10⁹× cheaper/TB), bandwidth (~−80% real $/Mbps home), compute (~10× FLOPS/$ every 4–8 yr) — colors match the chart series |
| **Unit costs · log index (1980–2026)** | Three series only — **Storage**, **Bandwidth**, **Compute** — as an index (**2009 = 100**). Lower = cheaper. Storage = era media (HDD through 2020, SSD/NVMe from 2021, same as Auto mode). History only; click the legend to hide a series. |
| **Bitcoin callout** | Short “so what for nodes”: unit costs fell faster than chain growth for most of history; 2026 flash spike is a temporary setback |
| **Storage $/TB · linear (2009–2040)** | Node-era HDD + SSD prices; dashed **Baseline** vs **AI-Shock** NVMe projections (same scenario math as the node model). Spikes (2011, 2026) are intentional. |
| **Moments on the curve** | Click a year to jump the dashboard year (2009+) and highlight the linear chart |
| **What ~$100 bought** | Illustrative HDD capacity by era |
| **Bridge** | Jump to Charts at 2026 with live node-model framing |

Long-run series live in `TECH_DEFLATION` inside `index.html` (not used by the node cost formulas). Methodology and sources: **[ASSUMPTIONS.md](./ASSUMPTIONS.md)** § Tech Deflation.

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

All model data lives in `index.html` inside the `<script>` block:

| Constant | Purpose |
|----------|---------|
| `RAW` | Node cost anchors: chain size, HDD/SSD $/TB, kit costs, internet, electricity (2009–2030 window + lerp) |
| `TECH_DEFLATION` | Multi-decade storage / bandwidth / compute series for the Tech Deflation tab |
| `PRUNED_SIZE_ANCHORS` | Pruned on-disk footprint by year |
| `CORE_INDEX_OPTIONS` / `ELECTRUM_OPTIONS` | Optional index size models |
| Projection sliders / `getScenarioModifiers` | 2027–2040 paths (also drive Tech Deflation’s Baseline vs AI-Shock SSD lines) |

Edit nominal $/TB, kit costs, chain size anchors, long-run deflation series, and projection defaults there.

## Model sense-check

See **[ASSUMPTIONS.md](./ASSUMPTIONS.md)** for the 2026 ground-truth sheet (chain size, NVMe, kit, EIA electricity, BPI broadband), Tech Deflation series notes, and a re-verify checklist. Anchors last reviewed **2026-07-09**.

## Sources

**Node costs:** Blockchain.com, YCharts, Backblaze, [Bitcoin.org](https://bitcoin.org/en/full-node), 2026 NVMe retail estimates (Tom's Hardware, TrendForce), EIA Electric Power Monthly, USTelecom BPI.

**Tech Deflation (long-run):** John C. McCallum disk/memory tables (via Our World in Data), Matt Komorowski hard-drive cost history, Backblaze cost-per-GB, AI Impacts–class compute price-performance trends, USTelecom BPI $/Mbps.

Approximations for illustration — not financial advice.

## License

MIT (Bitcoin.org project lineage for educational use).