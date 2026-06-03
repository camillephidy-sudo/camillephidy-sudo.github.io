---
title: "Soil Salinization Detection and Monitoring System"
collection: patent
type: "Patent"
permalink: /patent/2023-soil-salinization-detection-monitoring-system
excerpt: "General On-Orbit Earth Surface Soil Salinization Detection (GOESSD)—a dual-branch monitoring system for regional soil salinity in irrigated arid basins."
date: 2023-07-01
patent_number: "CN 2023SR0815282"
inventors: "D. Chen, L. Li, J. Yang, Y. Xu"
venue: "The Institute of Soil Science, Chinese Academy of Sciences (ISSAS)"
location: "Hetao Irrigation District, Inner Mongolia, China"
header:
  teaser: /images/goessd/fig4.png
---

## Overview

**General On-Orbit Earth Surface Soil Salinization Detection and Monitoring System (GOESSD)** is a regional-scale soil salinization monitoring system developed between May 2022 and July 2023 at the Institute of Soil Science, Chinese Academy of Sciences (ISSAS). The system recasts salinity monitoring as an **unsupervised anomaly detection** problem over multi-temporal Earth observation, rather than relying solely on regression of soil salt content (SSC).

Awarded **CN Patent 2023SR0815282**, the platform processes heterogeneous Sentinel-1 (microwave backscatter and polarization indices) and Sentinel-2 (optical) data to generate soil salinity maps and automated alerts for stakeholders in irrigated arid and semi-arid regions (validation **R² = 0.81** for root-zone ion gradients).

| Attribute | Detail |
|-----------|--------|
| **Role** | Project Leader |
| **Period** | February 2022 – June 2023 |
| **Study area** | Hetao (河套) irrigation district, Inner Mongolia |
| **Ground reference** | ~64 field sampling sites with measured SSC (dS/m) |
| **Stakeholders served** | 4,000+ users receiving automated salinity alerts |

![Multi-scale map of the Hetao study area with field sampling points across Huinong, Pingluo, and Dawukou districts.](/images/goessd/study-area.png)
*Figure 1. Study area location (Ningxia) and distribution of SSC ground-truth sampling sites.*

![Field soil sampling and in-situ data logging at Hetao cropland sites.](/images/goessd/field-sampling-1.jpg)
*Figure 2. Field sampling campaign supporting GOESSD validation.*

## Dual-Branch Architecture

A **sensor-agnostic anomaly framework** separates sensor physics from detection logic, enabling fair comparison and optional late fusion of microwave-native and optical foundation-model features.

![GOESSD dual-branch system architecture for regional soil salinization monitoring.](/images/goessd/fig2.png)
*Figure 3. Dual-branch GOESSD architecture integrating microwave–texture and optical sensing streams.*

### Branch A — Microwave / texture (primary)

- **Inputs:** Sentinel-1 VV/VH backscatter, Gamma0, derived indices (RVI, DVI, SDI), and GLCM texture statistics
- **Encoder:** Stacked multi-band representations, PCA + GMM, or SAR-specific models—**not** pseudo-RGB fed into vision models
- **Role:** Primary scientific path for salinity-relevant anomaly detection

### Branch B — Optical (supplementary)

- **Inputs:** Sentinel-2 true RGB surface reflectance composites
- **Encoder:** Segment Anything Model (SAM) ViT-H frozen encoder
- **Prior:** Gaussian Mixture Models with BIC component selection on baseline (`pre`) embeddings only
- **Role:** Captures vegetation stress, moisture, and surface albedo anomalies co-occurring with salinity

![SAM encoder workflow with dictionary look-up anomaly scoring and connectivity analysis.](/images/goessd/fig3.png)
*Figure 4. Optical-branch processing chain: SAM encoding, prior-based scoring, and anomaly object extraction.*

Both branches share the same **baseline regime → prior → anomaly score** pipeline derived from ESAD (General On-Orbit Earth Surface Anomaly Detection): fit priors on low-salinity `pre` conditions, score `post` departures via negative log-likelihood.

## Key Technical Contributions

- Architected **GOESSD**, a dual-branch unsupervised anomaly detection system integrating Sentinel-1 SAR–GLCM and Sentinel-2 optical streams for regional salinity mapping.
- Integrated **SAM ViT-H** within the optical branch; compressed high-dimensional embeddings with **BIC-selected GMM priors** achieving a **5:1 compression ratio** without compromising detection accuracy.
- Optimized anomaly scoring via Manhattan-distance dictionary look-up and efficient prior storage, reducing inference latency by **~40%** for regional-scale detection.
- Built a **reproducible, YAML-driven MLOps pipeline** with Git-hashed run logs, ROC-AUC/PR-AUC evaluation, and config-driven experiment orchestration (`soil-esad-prepare`, `soil-esad-run`, `soil-esad-report`).
- Formalized three anomaly modes: **temporal change** (pre/post composites), **absolute severity** (SSC ≥ 4.0 dS/m threshold), and **embedding outlier** (low density under normals-only priors).

![Prior compression comparison showing the proposed method achieves the highest compress rate with competitive AP.](/images/goessd/fig8.png)
*Figure 5. Prior storage compression benchmark—the proposed GOESSD prior achieves ~5:1 compression with preserved detection accuracy.*

## Anomaly Definition and Baseline Regime

| Mode | Definition | ESAD mapping |
|------|------------|--------------|
| Temporal change | Departure from low-salinity baseline to stressed state | `pre.tif` vs `post.tif` |
| Absolute severity | Sites exceeding SSC threshold (default ≥ 4.0 dS/m; severe ≥ 8.0 dS/m) | `label.png` from thresholded SSC |
| Embedding outlier | Low density under prior fitted on normal GLCM/SAR features only | Prior on normal subset; score all sites |

Priors are fit on **normal (low-salinity) baseline epochs only**, mirroring ESAD convention that the pre-event image defines the normal feature distribution.

## Baseline Methods and Benchmarks

Site-level experiments on Hetao tabular GLCM+SAR features (seed 42):

| Method | ROC-AUC | PR-AUC | Best F1 |
|--------|---------|--------|---------|
| Isolation Forest | 0.587 | 0.336 | 0.429 |
| PCA + GMM | 0.552 | 0.374 | 0.435 |
| CNN residual proxy | 0.352 | 0.200 | 0.405 |

Unsupervised anomaly framing outperformed regression-residual scoring on sparse field samples, supporting the GOESSD design choice.

![Algorithm precision comparison (AP, P_opt, R_opt, F1_opt) across benchmark events.](/images/goessd/fig5.png)
*Figure 6. GOESSD (“Ours”) compared with traditional change/anomaly detection and deep embedding baselines.*

![Cross-event ablation over fires, volcanoes, floods, and landslides (average F1 = 0.852 for GOESSD).](/images/goessd/fig6.png)
*Figure 7. Multi-event ablation study demonstrating robust generalization of the proposed detector.*

## Detection Results and Visualization

![Multi-temporal anomaly detection comparison: normal vs. anomalous imagery and method masks (CBL, PCA, CEMB, EEMB, Ours).](/images/goessd/fig4.png)
*Figure 8. Visual comparison of GOESSD against baseline detectors on representative pre/post image pairs (red outline = reference).*

![False-color composites and segmentation of detected salinization-related surface anomalies.](/images/goessd/fig10.png)
*Figure 9. Regional-scale visualization of satellite composites and GOESSD anomaly boundaries.*

## Operational Impact

The system was scaled from research prototypes into a **real-world monitoring product** providing:

- Automated salinity alerts to **4,000+ stakeholders**
- Lightweight deployment suitable for regional irrigation management
- Sensor-agnostic extensibility for future SAR foundation-model embeddings and late fusion of Branch A and B score maps

## Inventors

D. Chen, L. Li, J. Yang, Y. Xu

## Related Work

This patent builds on ESAD (GFSAIT / TGRS benchmark) prior–score machinery, field SSC validation in the Hetao basin, and the reproducible `research_soil_esad` pipeline for soil salinity anomaly research.
