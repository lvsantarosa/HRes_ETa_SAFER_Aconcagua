# High-resolution evapotranspiration dataset for the Aconcagua river basin

[![DOI](https://zenodo.org/badge/DOI/INSERT_YOUR_ZENODO_DOI_HERE.svg)](https://doi.org/INSERT_YOUR_ZENODO_DOI_HERE)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Google Earth Engine](https://img.shields.io/badge/Google%20Earth%20Engine-Python-green)](https://earthengine.google.com/)

**Authors:**
* **Lucas Vituri Santarosa** (Pontificia Universidad Católica de Valparaíso - PUCV)
* **Cesar de Oliveira Ferreira Silva** (Universidade Estadual Paulista - UNESP)

---

## 📌 Overview

This repository contains the source code and operational workflow developed for the study **"An operational satellite-based framework for high-resolution evapotranspiration monitoring in data-scarce Mediterranean basins"**.

We present an automated, cloud-based framework (Google Earth Engine) to estimate **Actual Evapotranspiration ($ET_{act}$)** at high spatial resolution (30 m) in the Aconcagua River Basin, Chile. [cite_start]The model integrates **Landsat 8/9** imagery with **ERA5-Land** meteorological reanalysis data to overcome the scarcity of ground-based telemetry stations[cite: 106].

### Key Highlights
* [cite_start]**High Resolution:** 30-meter mapping allows for the differentiation of water consumption among specific orchard types (e.g., Avocado vs. Table Grapes)[cite: 160].
* **ERA5-Land Integration:** Utilizes global reanalysis data to substitute local meteorological stations, maintaining high correlation ($R^2 > 0.80$) with observed data[cite: 108].
* [cite_start]**Stable Calibration:** Implementation of a fixed-coefficient strategy, statistically robust and temporally consistent, which outperformed traditional dynamic calibration methods[cite: 107].

---

## ⚙️ Methodology & Parameters

The framework utilizes the **SAFER** (Simple Algorithm for Evapotranspiration Retrieving) algorithm. [cite_start]Unlike image-specific parameterization (which introduces noise in heterogeneous landscapes), this study validated the use of global coefficients derived from a 10-year historical analysis (2014–2024)[cite: 216, 217].



### Operational Parameters (Stable Strategy)
[cite_start]To reproduce the datasets generated in this study, use the following regression coefficients calibrated for the region[cite: 255]:

| Parameter | Value | Description |
| :--- | :--- | :--- |
| **Slope ($a$)** | `0.3684` | Linear coefficient for the $ET_{fr}$ vs $X$ relationship |
| **Intercept ($b$)** | `-0.0012` | Angular coefficient for the $ET_{fr}$ vs $X$ relationship |

