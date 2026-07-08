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
- **Charts** — chain size, cost breakdown, annual cost, year pie (archival also shows one-time hardware chart; hidden in pruned mode)
- **Data table** — year-by-year costs with CSV export
- **Share link** — copies URL with year, mode, indexes, and scenario params

## Cost models

### Archival (default)

| Component | Model |
|-----------|--------|
| **Storage** | `(blocks + UTXO) × 1.15` headroom; buy whole drive SKUs (0.5–4+ TB by era). HDD pricing before 2018, SSD from 2018 (auto mode). 2026 NVMe spike calibrated ~$130/TB retail |
| **Hardware** | Pi / mini-PC kit costs by year (`RAW.basePCCost`) |
| **Indexes** | Marginal storage on top of base chain size |
| **OpEx** | Internet attribution (% or fixed $/mo) + electricity at configurable watts and $/kWh |

### Pruned

| Component | Model |
|-----------|--------|
| **Disk footprint** | Piecewise anchors — not a purchase cost. `<1 GB` (2010–12) → ~2.5 GB (2015) → ~5 GB (2018) → ~10 GB (2022) → ~18 GB (2026); post-2026 tracks UTXO growth |
| **Hardware** | **$0** — existing laptop/desktop |
| **Storage cost** | **$0 marginal** (default) — uses free space on your drive. Optional **USB/SD reference** in Assumptions if disk is full |
| **Indexes** | Disabled (require full block files) |
| **OpEx** | Same as archival — this is where pruned cost actually lives |

First sync still downloads the full chain (~740 GB bandwidth) in both modes.

## Projections

Open **Projections to 2040** to tune:

- Blockchain growth (GB/yr)
- Storage, hardware, internet, and electricity deflation
- SSD premium and Electrum index multiplier
- Scenario presets

Historical data runs through 2026; 2027–2040 uses the selected scenario.

## URL parameters

| Param | Example | Description |
|-------|---------|-------------|
| `year` | `2026` | Selected year |
| `pruned` | `true` | Pruned mode |
| `prunedStorage` | `marginal` or `usb` | Pruned storage assumption |
| `scenario` | `baseline` | Projection scenario |
| `electrum` | `electrs` | Electrum server mode |
| `txindex`, `coinstats`, `blockfilter` | `true` | Core index toggles |

## Customization

All model data lives in `index.html` inside the `RAW`, `PRUNED_SIZE_ANCHORS`, `CORE_INDEX_OPTIONS`, and `ELECTRUM_OPTIONS` arrays at the top of the `<script>` block. Edit nominal $/TB, kit costs, chain size anchors, and projection defaults there.

## Model sense-check

See **[ASSUMPTIONS.md](./ASSUMPTIONS.md)** for the 2026 ground-truth sheet (chain size, NVMe, kit, EIA electricity, BPI broadband) and a re-verify checklist. Anchors last reviewed **2026-07-09**.

## Sources

Data synthesized from Blockchain.com, YCharts, Backblaze, [Bitcoin.org](https://bitcoin.org/en/full-node), historical HDD/SSD pricing, 2026 NVMe retail estimates (Tom's Hardware, TrendForce), EIA Electric Power Monthly, and USTelecom BPI. Approximations for illustration — not financial advice.

## License

MIT (Bitcoin.org project lineage for educational use).