---
title: "Prescribed-Burn Effectiveness under Hydroclimatic Dynamics"
collection: portfolio
date: 2026-08-01
venue: "Independent research — GlobalRx space-for-time meta-analysis"
excerpt: "GlobalRx prescribed-burn records matched ambient-vs-warmer: burned area rises with warming (+113% at +5 °C) while opportunity-normalized delivery falls at +2…+3 °C—a program-feasibility tension under warmer climate analogs.<br/><img src='/images/portfolio/prescribed-burn/fig-pct-response-ladder.png'>"
header:
  teaser: /images/portfolio/prescribed-burn/fig-pct-response-ladder.png
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

Under warmer climate analogs, prescribed-burn **extent** and **relative emission-activity** increase; **opportunity-normalized delivery** does not. Absolute inventories stay near null.

### Primary burned area (Global)

| ΔT (°C) | *n* pairs | lnRR | 95% CI | % change |
|--------:|----------:|-----:|--------|---------:|
| +1 | 3991 | 0.187 | [−0.151, 0.525] | +20.6% |
| +2 | 3738 | 0.215 | [−0.165, 0.595] | +24.0% |
| +3 | 3291 | 0.415 | [−0.035, 0.865] | +51.5% |
| +4 | 2959 | 0.593 | [0.082, 1.103] | +80.9% |
| +5 | 2464 | 0.758 | [0.188, 1.328] | **+113.4%** |

The area ladder is approximately linear (**24.3% °C⁻¹**, R² = 0.95). Pair counts taper as matching becomes harder at high ΔT.

{% include field-log-figure.html src="/images/portfolio/prescribed-burn/fig-pct-response-ladder.png" alt="Percentage response of prescribed-burn endpoints versus warming from +1 to +5 degrees C." caption="Figure 1. Warming ladder: primary area and related endpoints versus +1…+5 °C (95% CI)." %}

### Parallel Global endpoints at +5 °C

| Branch | *n* | lnRR | % |
|--------|----:|-----:|--:|
| EmisProxy | 2535 | 0.574 | +77.6% |
| FINN PM₂.₅ | 377 | 0.102 | +10.7% |
| GFED PM₂.₅ | 433 | −0.035 | −3.4% |
| Window activity | 732 | 0.073 | +7.5% |
| Safe-FFDI share | 732 | 0.188 | +20.7% |

EmisProxy rises more modestly than area (18.2% °C⁻¹ vs 24.3% °C⁻¹): fuel × combustion weighting does not amplify hectares one-for-one.

{% include field-log-figure.html src="/images/portfolio/prescribed-burn/fig-all-branches-ladders.png" alt="Warming ladders for all prescribed-burn meta-analysis branches." caption="Figure 2. Parallel-branch warming ladders (area, emission proxies, inventories, burn window)." %}

### Program-feasibility tension

Window activity density (burns per OpportunityDay) is a **U-shape**, not a line: near-null at +1 °C, **significantly negative at +2…+3 °C** (lnRR −0.299 / −0.376; about −26% / −31%), then recovering with wide CIs at +5 °C. Warmer analogs can host **larger individual burns** even as **burns per open day fall**.

Safe-FFDI share is near-null to mildly positive—little evidence that operations systematically leave the Rx-safe band once seasons and cells are matched.

{% include field-log-figure.html src="/images/portfolio/prescribed-burn/fig-window-compartments.png" alt="Burn-window activity density versus safe FFDI share across warming levels." caption="Figure 3. Window compartments: activity density (quantity) vs. safe-FFDI share (quality)." %}

{% include field-log-figure.html src="/images/portfolio/prescribed-burn/fig-inventory-validation.png" alt="Relative emission proxy compared with FINN and GFED absolute PM2.5 inventories." caption="Figure 4. External check: relative EmisProxy vs. absolute FINN/GFED PM₂.₅." %}

{% include field-log-figure.html src="/images/portfolio/prescribed-burn/fig-global-vs-australia.png" alt="Global versus Australia warming ladders for prescribed-burn endpoints." caption="Figure 5. Domain pattern: Global vs. Australia ladders (same matching grammar)." %}

### Heterogeneity and domain

- **MetaForest** (primary area): baseline burned area dominates %-change, then climate (MAT, MAP); coarse landform is weakest (calibration R² = 0.90; held-out R² = 0.56).
- **Australia** area responses are much steeper than the global mean (e.g. +5 °C AU lnRR 2.67 vs Global 0.76). Season-aligned AU window activity is increasingly negative at high ΔT.
- **Precip-window sensitivity** (25–100 mm): direction of the area ladder is stable; the production 100 mm window is the most conservative at high ΔT.

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
