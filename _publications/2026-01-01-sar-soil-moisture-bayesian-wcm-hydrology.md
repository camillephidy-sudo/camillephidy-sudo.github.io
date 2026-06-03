---
title: "Improve the Accuracy of SAR-Based Soil Moisture Retrieval by Coupling Bayesian Inference and Water Cloud Model"
collection: publications
category: manuscripts
permalink: /publication/2026-01-01-sar-soil-moisture-bayesian-wcm-hydrology
excerpt: 'Manuscript under review at Journal of Hydrology—Bayesian–Water Cloud coupling to improve SAR soil moisture retrieval accuracy with explicit uncertainty handling.'
date: 2026-01-01
venue: 'Journal of Hydrology'
citation: 'Chen, D.*, L. Chai, S. Liu, S. Zhao and Z. Jin (2026). &quot;Improve the accuracy of SAR-based soil moisture retrieval by coupling Bayesian inference and water cloud model.&quot; <i>Journal of Hydrology</i>. <i>Under review</i>.'
tags:
  - journal-of-hydrology
  - bayesian-inference
  - water-cloud-model
  - soil-moisture
  - sentinel-1
  - under-review
header:
  teaser: /images/goessd/field-sampling-1.jpg
---

**Status:** Under review at *Journal of Hydrology* (2026).

## Abstract

This manuscript couples the **Water Cloud Model (WCM)** with **Bayesian inference** to improve the accuracy and reliability of **SAR-based soil moisture retrieval**. The probabilistic framework addresses ill-posed inversion under variable vegetation opacity and propagates uncertainty through to soil moisture estimates—supporting hydrological applications that require defensible error bounds.

## Authors

**D. Chen** (corresponding author), L. Chai, S. Liu, S. Zhao, Z. Jin

## Motivation

Deterministic WCM inversions for soil moisture often degrade when:

- Vegetation water content and soil dielectric properties co-vary ambiguously.
- Observation noise is absorbed into point estimates without uncertainty reporting.
- Seasonal crop dynamics violate fixed-parameter assumptions.

Bayesian coupling provides a physically constrained yet statistically rigorous alternative.

## Approach

| Element | Role |
|---------|------|
| **WCM forward model** | Separates vegetation and soil contributions to C-band backscatter |
| **Bayesian inference** | Posterior estimation of soil moisture and vegetation parameters |
| **Multi-temporal SAR** | Time-series constraints reduce parameter ambiguity |
| **Uncertainty outputs** | Credible intervals for hydrological assimilation and validation |

## Expected contributions

- Improved **soil moisture retrieval accuracy** versus classical WCM least-squares solvers.
- Explicit **uncertainty quantification** aligned with hydrological modeling needs.
- Extension of the Bayesian–WCM thread from [IGARSS 2024 crop water content](/publication/2024-01-01-retrieving-corn-gravimetric-water-content) toward root-zone moisture monitoring.

## Related work

| Link | Connection |
|------|------------|
| [IGARSS 2024 (Chen*, Chai, Cui)](/publication/2024-01-01-retrieving-corn-gravimetric-water-content) | WCM + dual-platform SAR validation |
| [Topographic correction (Zhao et al., 2023)](/publication/2023-01-01-an-empirical-model-for-correction) | Terrain normalization for SAR stacks |
| [GOESSD patent](/patent/2023-soil-salinization-detection-monitoring-system/) | Regional soil-moisture/salinity monitoring context |

## Keywords

soil moisture · Bayesian inference · Water Cloud Model · SAR · Journal of Hydrology · uncertainty quantification
