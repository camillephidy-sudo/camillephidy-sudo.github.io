---
title: "Prescribed-Burn Effectiveness under Hydroclimatic Dynamics"
collection: portfolio
date: 2026-08-01
venue: "Independent research — GlobalRx space-for-time meta-analysis"
excerpt: "GlobalRx prescribed-burn records matched ambient-vs-warmer: burned area rises with warming (+113% at +5 °C) while opportunity-normalized delivery falls at +2…+3 °C—a program-feasibility tension under warmer climate analogs.<br/><img src='/images/portfolio/prescribed-burn/Fig2_pct_response_ladder.png'>"
header:
  teaser: /images/portfolio/prescribed-burn/Fig2_pct_response_ladder.png
redirect_from:
  - /portfolio/2023-12-01-soc-warming-meta-analysis-pipeline/
tags:
  - prescribed-burning
  - wildfire
  - hydroclimate
  - meta-analysis
  - globalrx
  - air-quality
---

## Overview

This project asks how **prescribed-burn effectiveness** responds to **warming × precipitation windows**, stratified by biome, fuelbed, and landform. Using **GlobalRx** event records, I built a reproducible R pipeline that constructs ambient-vs-warmer space-for-time pairs, then fits hierarchical `metafor::rma.mv` models on log response ratios (lnRR) for burned area, emission-relevant activity, and burn-window feasibility.

The modular architecture reuses the targets-style pipeline originally developed for the 2023 BNU Graduate Student Talent Program, retargeted from soil carbon to fire–climate synthesis.

| Attribute | Detail |
|-----------|--------|
| **Role** | Project lead & pipeline architect |
| **Period** | 2026 |
| **Question** | How do Rx area, emission activity, and realizable burn windows change under warmer hydroclimate analogs? |
| **Data** | GlobalRx events + WorldClim climate, landform, fuelbed, precip seasonality; FINN/GFED PM₂.₅ calibration; Open-Meteo + CEMS FFDI opportunity days |
| **Stack** | R, `{targets}`, `metafor`, `terra`, `sf`, `ggplot2`, YAML-driven config |
| **Effect metric** | lnRR (warmer / ambient); % change = \((e^{\ln RR}-1)\times 100\) |

## Problem & Scope

Prescribed fire is only feasible inside meteorological **prescription windows**. Warming can enlarge individual burns while shrinking the number of safe operating days—so **extent** and **program delivery** can move in opposite directions. Absolute emission inventories also miss many Rx-scale fires, so relative activity proxies and inventory masses are not interchangeable.

The pipeline supplies observational fire–climate constraints (area, emission-weighted activity, window feasibility) before chemistry–transport modelling.

## Pipeline Architecture

```text
GlobalRx events → climate/landform covariates → feature engineering
      → ambient–warmer pair matching (fuelbed, landform, precip seasonality, ΔT)
      → hierarchical meta-analysis (lnRR)
      → parallel branches (fuel-reduction, weather-normalized, EmisProxy,
         FINN/GFED PM₂.₅, window activity, safe-FFDI share)
      → hydroclimate sensitivity + publication figures
```

**Matching** holds fuelbed, landform, and precip seasonality within pairs under configurable temperature and precipitation windows (`config/paths.yml`). Pair-level variance uses winsorized area and assumed CV (`vi = 2·CV²`).

### Parallel endpoints

1. **Primary area** — burned-area lnRR (treatment intensity)
2. **Fuel-reduction** — `Burn Objective` restricted to fuel / hazardous-fuels reduction
3. **Weather-normalized** — log-area residuals after FFDI/FWI/VPD (+ fuelbed/landform); Australia-only
4. **EmisProxy** — relative PM₂.₅ activity `Area × fuel load × FFMC combustion × EF`
5. **FINN / GFED PM₂.₅** — absolute inventory calibration
6. **Burn-window activity** — burns per OpportunityDay (hybrid Open-Meteo + CEMS FFDI)
7. **Safe-FFDI share** — share of burns in FFDI [1, 12]

## Key Results

Three compartments diverge under the same matching grammar:

| Compartment | Pattern along +1…+5 °C | Implication |
|-------------|------------------------|-------------|
| **Extent** (primary / fuel-reduction area) | Linear rise, **+20.6% → +113.4%** (24.3% °C⁻¹; *n* = 3991 → 2464) | Warmer analogs host larger burns |
| **Delivery** (burns / OpportunityDay) | U-shape: **−26% / −31% at +2 / +3 °C** | Fewer operations per open day |
| **Emissions** (EmisProxy vs FINN/GFED) | Relative proxy **+78% at +5 °C**; inventories near null | Fuel × combustion does not scale 1:1 with hectares |

Figures below are the production `soc_style` pack (Wang et al. 2022 grammar).

{% include field-log-figure.html class="portfolio-figure" src="/images/portfolio/prescribed-burn/Fig2_pct_response_ladder.png" alt="Prescribed-burn percentage responses versus warming for extent, delivery, and emission endpoints." caption="Figure 2. Extent climbs; window activity does not. EmisProxy rises more slowly than area; FINN/GFED stay near null." %}

{% include field-log-figure.html class="portfolio-figure" src="/images/portfolio/prescribed-burn/Fig2c_window_compartments.png" alt="Burn-window activity density versus safe FFDI share across warming levels." caption="Figure 2c. Quantity vs quality inside the window: delivery drops at moderate warming; safe-FFDI share is stable to mildly positive." %}

{% include field-log-figure.html class="portfolio-figure" src="/images/portfolio/prescribed-burn/Fig3_moderators_primary.png" alt="Moderator lollipop plots of residual burned-area responses by fuelbed, landform, precip seasonality, and biome." caption="Figure 3. Residual warming responses concentrate in plains, winter-seasonality burns, Mediterranean woodland/scrub, and broadleaf evergreen forest." %}

{% include field-log-figure.html class="portfolio-figure" src="/images/portfolio/prescribed-burn/Fig3b_metaforest_importance.png" alt="MetaForest variable importance and observed versus predicted lnRR for primary burned area." caption="Figure 3b. Baseline burned area dominates %-change, then climate (MAT, MAP); landform is weakest (validation R² = 0.56)." %}

{% include field-log-figure.html class="portfolio-figure" src="/images/portfolio/prescribed-burn/Fig4_inventory_validation.png" alt="Relative emission proxy compared with FINN and GFED absolute PM2.5 inventories." caption="Figure 4. EmisProxy is the narrative endpoint; absolute inventories are under-powered calibration, not interchangeable masses." %}

{% include field-log-figure.html class="portfolio-figure" src="/images/portfolio/prescribed-burn/Fig5b_rx_cells.png" alt="World maps of predicted prescribed-burn area percent change at 0.25 degree cells with GlobalRx events under plus 2 and plus 5 degrees C." caption="Figure 5b. Program geography (0.25° cells that already report Rx): predicted % intensifies from +2 to +5 °C, mainly in the US and SE Australia." %}

{% include field-log-figure.html class="portfolio-figure" src="/images/portfolio/prescribed-burn/Fig5_global_vs_australia.png" alt="Global versus Australia warming ladders for prescribed-burn endpoints." caption="Figure 5. Geography is first-order: AU area at +5 °C is far steeper (lnRR 2.67 vs Global 0.76)." %}

{% include field-log-figure.html class="portfolio-figure" src="/images/portfolio/prescribed-burn/Fig6_implied_extra_hectares.png" alt="Implied extra prescribed-burn hectares under warming from historical stock times predicted percent change." caption="Figure 6. Stock × predicted % is not the pair-count ladder: implied extra hectares turn positive only above +3 °C." %}

{% include field-log-figure.html class="portfolio-figure" src="/images/portfolio/prescribed-burn/Fig2_all_branches_ladders.png" alt="Percentage-change warming ladders for all GlobalRx meta-analysis branches." caption="Figure S. Branch pack: fuel-reduction tracks area; weather-normalized (AU) is an order of magnitude larger; inventories and window stay near the origin." %}

## Deliverables

- Hierarchical meta tables by warming level and branch (`outputs/tables/meta_effectiveness_by_warming*.csv`)
- Wang-style results narrative and branch comparison reports
- Publication figure pack (ladders, moderator lollipops, Global vs Australia, 0.25° Rx-cell maps)
- Hydroclimate sensitivity grids that do not overwrite the primary run
- Stage summary logs under `outputs/logs/`

## Reproducibility

- Raw inputs treated as read-only under `datasets/`
- Generated artifacts isolated in `data/` and `outputs/`
- YAML-driven paths, matching windows, emission proxy, and opportunity meteorology
- One-command full run: `Rscript scripts/run_all.R`

Partial runners cover matching + meta, parallel branches, opportunity-day counting, hydroclimate sensitivity, and figure regeneration.

## Skills Demonstrated

Space-for-time causal design · hierarchical meta-analysis · fire-weather opportunity windows · emission-activity proxies vs inventory calibration · reproducible R/`targets` pipelines · manuscript-grade figure grammar
