# 2026 Ground Truth Sheet

Sense-check of dashboard anchors vs external sources for **calendar year 2026** (model “present”).  
**Sheet date:** 2026-07-09 · **Model file:** `index.html` (`RAW` + constants)  
**Model refresh:** 2026-07-09 — applied follow-ups from this sheet (blocks, electricity, broadband)

**Pass rule:** if a real “buy a node this month” total is within **~±20%** of the dashboard’s 2026 archival setup, the product story holds.

---

## Executive summary

| Area | Model (2026, post-refresh) | External check | Status |
|------|----------------------------|----------------|--------|
| Block files | **753 GB** | YCharts **~752.6 GB** (2026-07-07) | **Pass** |
| UTXO / chainstate | **12.5 GB** | Field / docs **~10–15 GB** | **Pass** |
| Archival min storage (×1.15) | **~880 GB** | → **2 TB** SKU | **Pass** |
| Consumer NVMe | **$130/TB** (~$260 / 2 TB) | Retail mid-tier **~$250–320** / 2 TB | **Pass** |
| Kit (Pi / mini-PC, no data drive) | **$165** | N100 / Pi 5 kits **~$150–250** | **Pass** |
| Archival one-time setup | **~$425** (2 TB × $130 + $165 kit; no USB enclosure from 2024+) | Cart **~$400–550** | **Pass** |
| Electricity | **18.8 ¢/kWh** | EIA residential **18.83 ¢** (Apr 2026) | **Pass** |
| Broadband plan | **$70 / mo** | USTelecom BPI-Speed nominal **~$69–73** | **Pass** |
| Default watts (2026) | **15 W** era-auto | Pi 5 / N100 idle–light **~8–20 W** | **Pass** (era-auto; higher for pre-Pi years) |
| Internet attribution | **15%** of plan | Judgment, not measured | **Policy** |
| Pruned disk | **~18 GB** | **~15–25 GB** field | **Pass** |
| Pruned setup | **$0** | Existing PC + free space | **Pass** (by design) |

**Bottom line:** 2026 anchors pass after aligning electricity and broadband to latest EIA / BPI and bumping blocks to mid-2026 chain size. OpEx knobs (15% internet, 15 W) remain intentional.

---

## Changes applied 2026-07-09

| Parameter | Before | After | Rationale |
|-----------|--------|-------|-----------|
| `BLOCKS_ANCHOR_2026` / `blocksGB[2026]` | 750 | **753** | YCharts ~752.62 GB on 2026-07-07 |
| `elecPerKwh[2026]` / `US_RATES.elec` | 0.1775 (17.75¢) | **0.188 (18.8¢)** | EIA Table 5.6.A residential Apr 2026 **18.83¢** |
| Near-term elec path 2027–30 | 0.180…0.189 | **0.191…0.200** | Smooth upward from new 2026 level |
| `internetMonthly[2025–26]` | 74 / 75 | **70 / 70** | BPI-Speed nominal ~$69–70 (2025 BPI) |
| Copy / footer | Jun 2026 750 GB, ~17.8¢ | Jul 2026 **753 GB**, **~18.8¢**, link to this file | Keep UI honest |

Storage kit and NVMe left unchanged (already within ±20% of street).

---

## 1. Chain size & disk footprint

### 1.1 Block files

| | Value |
|--|--------|
| **Model** | `BLOCKS_ANCHOR_2026 = 753` · `RAW.blocksGB[2026] = 753` |
| **External** | YCharts Bitcoin Blockchain Size: **752.62 GB** (2026-07-07) |
| **Source** | [YCharts I:BBS](https://ycharts.com/indicators/bitcoin_blockchain_size) |
| **Verdict** | **Pass.** |

### 1.2 UTXO / chainstate

| | Value |
|--|--------|
| **Model** | `UTXO_ANCHOR_2026 = 12.5` GB |
| **How to re-verify** | `du -sh ~/.bitcoin/chainstate` on a synced archival node |
| **Verdict** | **Pass** as mid-range (~10–15 GB). |

### 1.3 Archival storage formula

| | Value |
|--|--------|
| **Model** | `(blocks + UTXO) × 1.15` → **(753 + 12.5) × 1.15 ≈ 880 GB** |
| **Drive SKU** | **2 TB** (era min since 2019) |
| **Verdict** | **Pass.** |

### 1.4 Pruned footprint

| | Value |
|--|--------|
| **Model** | **18 GB** in 2026 (`PRUNED_SIZE_ANCHORS`) |
| **Verdict** | **Pass.** |

---

## 2. Storage $/TB

| | Value |
|--|--------|
| **Model** | `ssdPerTB[2026] = 130` → **$260 / 2 TB** (USB enclosure not charged from 2024+; NVMe HAT/internal in kit) |
| **External** | Mid-tier 2 TB often **~$250–320** (Tom's Hardware tracking / deals, mid-2026) |
| **Verdict** | **Pass** for mid-tier Gen4, not floor deals. |

---

## 3. Hardware kit

| | Value |
|--|--------|
| **Model** | `basePCCost[2026] = 165` |
| **Verdict** | **Pass** (~$150–250 street for N100 / Pi 5 class kit without data drive). |

---

## 4. One-time archival setup

| Component | Model $ |
|-----------|--------|
| Storage (2 TB × $130; no USB enclosure from 2024+) | **260** |
| Base PC kit | **165** |
| **Total setup (no optional indexes)** | **425** |

±20% band: **$340 – $510**. Realistic carts still land here. **Pass.**

---

## 5. Annual OpEx (post-refresh)

### 5.1 Electricity

| | Value |
|--|--------|
| **Model** | **0.188 $/kWh** (18.8 ¢) |
| **EIA** | **18.83 ¢** residential (April 2026, Table 5.6.A) |
| **Source** | [EIA EPM 5.6.A](https://www.eia.gov/electricity/monthly/epm_table_grapher.php?t=epmt_5_6_a) |
| **At 15 W** | **~(0.015 × 24 × 365 × 0.188) ≈ $24.7 / yr** |
| **Verdict** | **Pass.** |

### 5.2 Watts

| | Value |
|--|--------|
| **Default** | **Era-auto:** ~55 W (2009–11 desktop) → ~40 W (2012–14) → ~25 W (NUC) → ~12 W (Pi) → **15 W** (2024+ mini PC). Slider overrides. |
| **Verdict** | **Pass** for 2026; historical OpEx now tracks era hardware better. |

### 5.3 Internet

| | Value |
|--|--------|
| **Model plan** | **$70 / mo** (2025–26) |
| **BPI-Speed** | ~$72.58 (2024) → ~$69.33 (2025) nominal |
| **Sources** | [USTelecom 2026 BPI](https://ustelecom.org/research/2026-bpi/) |
| **15% attribution** | **$10.50 / mo** · **$126 / yr** |
| **Verdict** | **Pass** (plan). Attribution remains a **policy knob**. |

### 5.4 Combined annual OpEx (defaults)

| Component | Model |
|-----------|--------|
| Internet (15% × $70 × 12) | **$126** |
| Electricity (15 W × 18.8 ¢) | **~$25** |
| **Total annual OpEx** | **~$151** |

---

## 6. Indexes (spot-check)

| Index | Model basis | ~2026 size @ 753 GB blocks |
|-------|-------------|----------------------------|
| txindex | 44 / 554 of blocks | **~60 GB** |
| blockfilterindex | 9.4 / 554 | **~13 GB** |
| coinstatsindex | ~3 GB | **~3 GB** |

```bash
du -sh ~/.bitcoin/blocks ~/.bitcoin/chainstate ~/.bitcoin/indexes
```

Re-`du` before treating index costs as precise.

---

## 7. Tech Deflation tab (`TECH_DEFLATION`)

Educational multi-decade series (not used by the node cost formulas). Lives in `index.html` as `TECH_DEFLATION`.

| Series | Range | Units | Notes |
|--------|-------|-------|--------|
| `hddPerTB` | 1980–2026 | Nominal $/TB | Pre-2009: representative HDD retail from Komorowski / McCallum lineage ($/GB × 1000). 2009+: aligns with `RAW.hddPerTB`; **2011 set to 100** (vs smoothed 80) so the Thailand flood spike is visible on the linear chart. |
| `ssdPerTB` | sparse → 2026 | Nominal $/TB | Null until ~2008; 2009+ matches `RAW.ssdPerTB` (includes 2026 AI/NAND spike at 130). |
| `bandwidthUsdPerMbps` | 1998–2026 | Nominal $/Mbps/mo | Residential effective rate (early broadband → BPI-class 2015 $0.87 / 2023 $0.18 → ~$0.12 by 2026). Sparse; log-interpolated on the chart. |
| `computeCostIndex` | 1980–2026 | Index, 2009 = 100 | Illustrative $ for a fixed unit of performance (inverse FLOPS/$ ~10× / 4–8 yr). Not a BLS PPI. |
| Projections 2027–40 | dashed | $/TB → index | Same math as node model via `getSsdPricePerTBForScenario` / `getHddPricePerTBForScenario`: always plots **Baseline** and **AI-Shock** SSD paths for comparison. |
| KPIs | — | Facts | Storage ~10⁹×; compute ~10× FLOPS/$ per 4–8 yr; bandwidth real $/Mbps ~−80% BPI 2015→2023. |

**Charts:** multi-series log **index (2009 = 100)** with series toggles; linear storage $/TB 2009–2040 with dashed projections. Bitcoin node era band = 2009–2026. Share URL supports `?tab=deflation`.

**Not claimed:** matched-model PPI, CPI-adjusted series on the chart, or live TeleGeography feeds.

---

## 8. Not validated by this sheet

- Historical years 2009–2025 detail  
- Projection deflation paths 2027–2040  
- Non-US rates · real (CPI) dollars · first-sync bandwidth  
- That 15% internet share matches any one household  
- Pre-2009 storage anchors beyond order-of-magnitude (Tech Deflation tab)

---

## 9. Re-verify checklist

- [x] Blocks within ±5% of YCharts  
- [x] Chainstate in 10–15 GB band  
- [x] 2 TB NVMe within ±20% of $260  
- [x] Kit within ±25% of $165  
- [x] Archival setup within ±20% of $425  
- [x] EIA residential ¢/kWh within ±10% of model  
- [x] BPI / plan level aligned (~$70)  
- [x] Policy knobs (15% net, era-auto watts, ×1.15, 3-yr amort, SSD from 2021, 2 TB min from 2019) documented  

**Next full review:** after major Core release, large NAND price move, or new EIA/BPI annuals — target within one quarter.

---

## Sources (accessed ~2026-07-09)

- YCharts — Bitcoin Blockchain Size  
- EIA — Electric Power Monthly Tables 5.3 / 5.6.A (April 2026)  
- USTelecom — Broadband Pricing Index 2024–2026  
- Tom's Hardware — SSD price tracking & deals 2026  
- John C. McCallum disk/memory tables · Our World in Data historical storage costs  
- Matt Komorowski — cost per gigabyte (HDD history)  
- Backblaze — hard drive cost per gigabyte  
- Dashboard: `RAW`, `TECH_DEFLATION`, anchors, `US_RATES`
