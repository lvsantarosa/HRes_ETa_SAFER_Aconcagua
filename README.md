# High-resolution evapotranspiration dataset for the Aconcagua river basin

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.18603159.svg)](https://doi.org/10.5281/zenodo.18603159)

**Authors:**
* **Lucas Vituri Santarosa** (Pontificia Universidad Católica de Valparaíso - PUCV)
* **Cesar de Oliveira Ferreira Silva** (Universidade Estadual Paulista - UNESP)

---

## Overview

This repository contains the source code and operational workflow developed for the study **"An operational satellite-based framework for high-resolution evapotranspiration monitoring in data-scarce Mediterranean basins"**.

We present an automated, cloud-based framework (Google Earth Engine) to estimate **Actual Evapotranspiration ($ET_{act}$)** at high spatial resolution (30 m) in the Aconcagua River Basin, Chile. [cite_start]The model integrates **Landsat 8/9** imagery with **ERA5-Land** meteorological reanalysis data to overcome the scarcity of ground-based telemetry stations.

### Key Highlights
* **High Resolution:** 30-meter mapping allows for the differentiation of water consumption among specific orchard types (e.g., Avocado vs. Table Grapes).
* **ERA5-Land Integration:** Utilizes global reanalysis data to substitute local meteorological stations, maintaining high correlation ($R^2 > 0.80$) with observed data.
* **Stable Calibration:** Implementation of a fixed-coefficient strategy, statistically robust and temporally consistent, which outperformed traditional dynamic calibration methods.

---

## Methodology & Parameters

The framework utilizes the **SAFER** (Simple Algorithm for Evapotranspiration Retrieving) algorithm. Unlike image-specific parameterization (which introduces noise in heterogeneous landscapes), this study validated the use of global coefficients derived from a 10-year historical analysis (2014–2024).

## Reference
