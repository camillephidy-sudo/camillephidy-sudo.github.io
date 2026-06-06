---
title: "Soil Organic Carbon Under Warming — Reproducible Meta-Analysis Pipeline"
collection: portfolio
date: 2023-12-01
venue: "2023 Graduate Student Talent Program Research Project, Beijing Normal University"
excerpt: "Modular R pipeline from WoSIS profiles to matched-pair meta-analysis, forest modeling, and paper-parity figures quantifying depth-dependent SOC responses to +1–+5 °C warming.<br/><img src='/images/portfolio/soc-warming/fig-paper-parity-soc.png'>"
header:
  teaser: /images/portfolio/soc-warming/fig-paper-parity-soc.png
link: https://github.com/camillephidy-sudo/soil-organic-carbon
tags:
  - soil-carbon
  - meta-analysis
  - climate-change
  - reproducible-research
  - r-targets
---

## Overview

This **2023 Graduate Student Talent Program** project delivers a reproducible, modular workflow for quantifying **soil organic carbon (SOC)** responses to warming—from harmonized WoSIS profile data and environmental covariates through matched-pair inference, tuned forest modeling, dual-branch meta-analysis, spatial projection, and publication-ready outputs.

Using a targets-style R pipeline, I rebuilt a global soil–climate matching workflow through tuned forest modeling, dual-branch meta-analysis, and paper-parity figure generation. After resolving key method-alignment issues (temperature scaling, predictor parity, and a true SOC-content effect-size branch), the production model achieved stronger predictive skill and delivered stable warming-level results across **+1 to +5 °C** for portfolio-ready reporting.

| Attribute | Detail |
|-----------|--------|
| **Role** | Project lead & pipeline architect |
| **Period** | 2023 |
| **Program** | BNU Graduate Student Talent Program |
| **Data** | WoSIS soil profiles + WorldClim, landform, soil order, biome covariates |
| **Stack** | R, `{targets}`, `metafor`, `ranger`, `metaforest`, `terra`, `sf`, `ggplot2` |
| **Repository** | [camillephidy-sudo/soil-organic-carbon](https://github.com/camillephidy-sudo/soil-organic-carbon) |

## Problem & Scope

Climate warming threatens soil carbon storage across depth layers, but global inference requires harmonized profile data, defensible ambient–warming pair matching, and separate treatment of **SOC stock** vs. **SOC content** effect sizes. This pipeline addresses that gap with explicit stage outputs, diagnostics, and manuscript-ready tables.

## Pipeline Architecture

```text
WoSIS profiles → covariate extraction → feature engineering
      → ambient–warming pair matching → tuned metaforest (ranger)
      → meta_stock + meta_content branches → spatial projection
      → absolute-loss proxies → paper-parity figures
```

**Implemented stages** include profile preprocessing, covariate joins, generalized pair matching, `metaforest` training (with `baseline_soc` in production features), independent random-effects meta-analysis for stock and content, sensitivity analysis, spatial mapping, and automated report generation.

## Key Results

Using matched ambient-versus-warming soil profile pairs, random-effects meta-analysis indicates that **SOC stock responses are strongly depth-dependent**, with the clearest declines in surface soils and weaker or uncertain responses at depth.

| Finding | Detail |
|---------|--------|
| Strongest decline | **0–0.3 m** at **+5 °C**: **−45.72%** (95% CI −60.22 to −25.94) |
| Surface signal at +1 °C | **0–0.3 m**: **+22.39%** (95% CI 6.47 to 40.70; *p* = 0.004) |
| Deep-layer uncertainty | **1–2 m** at +1 °C: **−0.13%** (95% CI −20.40 to 25.30; *p* = 0.991) |
| Sample support | All reported strata have *n* ≥ 10 matched-effect records |

{% include field-log-figure.html src="/images/portfolio/soc-warming/fig-meta-pct-change.png" alt="Meta-analysis percentage change in SOC stock by soil layer and warming level." caption="Figure 1. SOC stock percentage change by depth layer and warming scenario (+1 to +5 °C)." %}

{% include field-log-figure.html src="/images/portfolio/soc-warming/fig-model-validation.png" alt="Observed versus predicted SOC from the tuned forest model." caption="Figure 2. Tuned metaforest model validation—observed vs. predicted SOC." %}

{% include field-log-figure.html src="/images/portfolio/soc-warming/fig-paper-parity-soc.png" alt="Paper-parity figure comparing SOC stock and content responses." caption="Figure 3. Paper-parity SOC stock and content comparison." %}

{% include field-log-figure.html src="/images/portfolio/soc-warming/fig-absolute-loss.png" alt="Summed absolute SOC loss proxy by soil layer and warming level." caption="Figure 4. Spatial projection—summed absolute SOC loss proxy by layer and warming." %}

## Deliverables

- Integrated results table (`final_results_main_table.csv`)
- Stock and content meta-analysis tables by layer and warming level
- Auto-generated meta-analysis and projection/loss reports
- Standard and **paper-parity** figure sets for manuscript submission
- Full run audit trail via stage summary logs

## Reproducibility

- Raw inputs treated as read-only under `datasets/`
- Generated artifacts isolated in `data/` and `outputs/`
- YAML-driven paths and matching constraints in `config/paths.yml`
- One-command full run: `Rscript scripts/run_all.R`

## Source Code

Full pipeline source, configuration, and reproducibility notes: **[github.com/camillephidy-sudo/soil-organic-carbon](https://github.com/camillephidy-sudo/soil-organic-carbon)**

## Skills Demonstrated

Reproducible research design · global soil–climate data harmonization · random-effects meta-analysis · spatial projection · publication figure automation · method parity auditing
