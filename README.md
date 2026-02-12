# High-resolution evapotranspiration dataset for the Aconcagua river basin

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.18603159.svg)](https://doi.org/10.5281/zenodo.18603159)

**Authors:**
* **Lucas Vituri Santarosa** (Pontificia Universidad Católica de Valparaíso - PUCV)
* **Cesar de Oliveira Ferreira Silva** (Universidade Estadual Paulista - UNESP)

---

## Overview

We present an automated, cloud-based framework (Google Earth Engine) to estimate **Actual Evapotranspiration ($ET_{a}$)** at high spatial resolution (30 m) in the Aconcagua River Basin, Chile. The model integrates **Landsat 8/9** imagery with **ERA5-Land** meteorological reanalysis data to overcome the scarcity of ground-based telemetry stations.

### Key Highlights
*	Stable calibration strategy ensures temporal consistency for basin-scale evapotranspiration estimation
*	Remote sensing data integration enabling automated cloud-based evapotranspiration monitoring
*	High-resolution mapping exposes the basin-scale structural decoupling between supply and demand. 


---

## Methodology & Parameters

The framework utilizes the **SAFER** (Simple Algorithm for Evapotranspiration Retrieving) algorithm. Unlike image-specific parameterization (which introduces noise in heterogeneous landscapes), this study validated the use of global coefficients derived from a 10-year historical analysis (2014–2024).

## Reference
