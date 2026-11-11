# High-resolution evapotranspiration dataset for the Aconcagua river basin

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.18603159.svg)](https://doi.org/10.5281/zenodo.18603159)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/lvsantarosa/HRes_ETa_SAFER_Aconcagua/blob/main/SAFER_ACONCAGUA_DEMO_0_1.ipynb)

**Authors:**
* **Lucas Vituri Santarosa** (Pontificia Universidad Católica de Valparaíso - PUCV)
* **Cesar de Oliveira Ferreira Silva** (Universidade Estadual Paulista - UNESP)

---

## Overview

This repository contains the source code and operational workflow developed for the study **"Assessment of structural water stress in semi-arid agricultural basins using high-resolution evapotranspiration mapping"**, accepted for publication in *Ecohydrology & Hydrobiology* (2026).

We present an automated, cloud-based framework (Google Earth Engine) to estimate **Actual Evapotranspiration ($ET_{a}$)** at high spatial resolution (30 m) in the Aconcagua River Basin, Chile. The model integrates **Landsat 8/9** imagery with **ERA5-Land** meteorological reanalysis data to overcome the scarcity of ground-based telemetry stations.

### Key Highlights
*	Stable calibration strategy ensures temporal consistency for basin-scale evapotranspiration estimation
*	Remote sensing data integration enabling automated cloud-based evapotranspiration monitoring
*	High-resolution mapping exposes the basin-scale structural decoupling between supply and demand.

---

## Methodology & Parameters

The framework utilizes the **SAFER** (Simple Algorithm for Evapotranspiration Retrieving) algorithm. Unlike image-specific parameterization (which introduces noise in heterogeneous landscapes), this study validated the use of global coefficients derived from a 10-year historical analysis (2014–2024).

$$ET_{a} = ET_{r} \times ET_{0}, \qquad ET_{r} = \exp\left(a + b \cdot \frac{T_{s}}{\alpha_{0} \cdot NDVI}\right)$$

where $ET_{r}$ is the relative (evaporative fraction) term retrieved from Landsat, $T_{s}$ the land surface temperature (°C), $\alpha_{0}$ the surface albedo, $NDVI$ the normalized difference vegetation index, and $ET_{0}$ the daily reference evapotranspiration (FAO-56 Penman–Monteith) computed from ERA5-Land.

| Parameter | Value | Notes |
| :--- | :--- | :--- |
| SAFER coefficient `a` | `0.3684` | Fixed (stable calibration, 2014–2024) |
| SAFER coefficient `b` | `-0.0012` | Fixed (stable calibration, 2014–2024) |
| $ET_{r}$ valid range | `0.0 – 1.2` | Clamped to suppress outliers |
| Spatial resolution | `30 m` | Landsat native grid |
| Temporal resolution | `daily` | Aggregated to monthly and annual sums |
| Target year (demo) | `2025` | Set via the `YEAR` variable |

### Input datasets

| Dataset | Earth Engine ID | Use |
| :--- | :--- | :--- |
| Landsat 9 C02 L2 | `LANDSAT/LC09/C02/T1_L2` | Albedo, NDVI, LST → $ET_{r}$ |
| Landsat 8 C02 L2 | `LANDSAT/LC08/C02/T1_L2` | Albedo, NDVI, LST → $ET_{r}$ |
| ERA5-Land daily | `ECMWF/ERA5_LAND/DAILY_AGGR` | $ET_{0}$ (Penman–Monteith) |
| SRTM DEM | `USGS/SRTMGL1_003` | Atmospheric pressure for $ET_{0}$ |
| Basin boundary | `projects/ee-lucasviturisantarosa/assets/ACG` | Processing/clipping region |

### Gap-filling strategy

Clouds are masked using the Landsat `QA_PIXEL` band (dilated cloud, cloud, cloud shadow). For each month, $ET_{r}$ is taken as the monthly median of the available scenes; where a month has no valid observation, the annual median is used as a fallback. Daily $ET_{a}$ is then obtained by multiplying the daily ERA5-Land $ET_{0}$ by this representative $ET_{r}$, which preserves day-to-day meteorological variability while keeping the surface term continuous in time.

---

## Repository contents

| File | Description |
| :--- | :--- |
| `SAFER_ACONCAGUA_DEMO_0_1.ipynb` | Main notebook: parameters, pre-processing, SAFER processing, export and visualization |
| `requirements.txt` | Python dependencies |
| `CITATION.cff` | Machine-readable citation metadata (GitHub "Cite this repository") |
| `CITATION.bib` | BibTeX entries, ready to import into Zotero/Mendeley |
| `.zenodo.json` | Metadata used by Zenodo when archiving a release |
| `LICENSE` | MIT license |

---

## Requirements & usage

### Requirements
* A Google account with **Google Earth Engine** access and a registered cloud project ([sign up](https://code.earthengine.google.com/register)).
* Python 3.9+ with `earthengine-api` and `geemap` (`pip install -r requirements.txt`), or simply run the notebook in **Google Colab** (badge above — no local installation needed).
* The basin boundary as an Earth Engine asset. The notebook points to the authors' asset; replace `geometry` with your own asset or geometry to run it over a different area.

### Usage
1. **Authenticate.** In cell 1, set `ee.Initialize(project="<your-ee-project>")` to your own Earth Engine project id.
2. **Configure.** In cell 2, adjust `YEAR`, `SCALE` and the export folder names.
3. **Visualize.** Run all cells to render daily and annual $ET_{a}$ on the interactive `geemap` map.
4. **Export.** In section 6, uncomment the `task.start()` lines to submit the export tasks; results are written as GeoTIFFs to the configured Google Drive folders.

### Outputs

| Product | Band / file naming | Units |
| :--- | :--- | :--- |
| Daily $ET_{a}$ stacks (one file per month) | `SAFER_Daily_Stack_<YEAR>_<MM>` | mm day⁻¹ |
| Monthly $ET_{a}$ sums (12-band stack) | `SAFER_Monthly_Aggregated_<YEAR>` | mm month⁻¹ |
| Annual $ET_{a}$ sum | `SAFER_Annual_Sum_<YEAR>` | mm year⁻¹ |

All rasters are 32-bit float, 30 m, clipped to the basin boundary, in the Landsat native projection.

> **Note on export size.** Daily stacks at 30 m over the whole basin are large; exports are therefore batched by month. If a task fails with a memory or timeout error, increase `tileScale` in the `Export.image.toDrive` call or export sub-regions.

---

## How to cite

Please cite **both** the software/dataset and the associated article.

**Software / dataset (Zenodo):**

> Santarosa, L. V., & Silva, C. O. F. (2026). *High-resolution evapotranspiration dataset for the Aconcagua River Basin (SAFER / Landsat / ERA5-Land)* (Version 0.1.0) [Software]. Zenodo. https://doi.org/10.5281/zenodo.18603159

**Associated article:**

> Santarosa, L. V., & Silva, C. O. F. (2026). Assessment of structural water stress in semi-arid agricultural basins using high-resolution evapotranspiration mapping. *Ecohydrology & Hydrobiology*. (In press; volume, pages and DOI to be added on publication.)

BibTeX entries for both are provided in [`CITATION.bib`](CITATION.bib) — download the file and use **File → Import…** in Zotero, or drag it into your library. GitHub's *Cite this repository* button (powered by [`CITATION.cff`](CITATION.cff)) also exports APA and BibTeX directly.

---

## Reference

* Teixeira, A. H. de C. (2010). Determining regional actual evapotranspiration of irrigated crops and natural vegetation in the São Francisco river basin (Brazil) using remote sensing and Penman-Monteith equation. *Remote Sensing*, 2(5), 1287–1319. https://doi.org/10.3390/rs2051287
* Allen, R. G., Pereira, L. S., Raes, D., & Smith, M. (1998). *Crop evapotranspiration: Guidelines for computing crop water requirements*. FAO Irrigation and Drainage Paper 56. FAO, Rome.
* Muñoz-Sabater, J., Dutra, E., Agustí-Panareda, A., et al. (2021). ERA5-Land: A state-of-the-art global reanalysis dataset for land applications. *Earth System Science Data*, 13(9), 4349–4383. https://doi.org/10.5194/essd-13-4349-2021
* Gorelick, N., Hancher, M., Dixon, M., Ilyushchenko, S., Thau, D., & Moore, R. (2017). Google Earth Engine: Planetary-scale geospatial analysis for everyone. *Remote Sensing of Environment*, 202, 18–27. https://doi.org/10.1016/j.rse.2017.06.031

---

## License

Released under the [MIT License](LICENSE). Landsat and ERA5-Land source data remain subject to the terms of their respective providers (USGS/NASA and ECMWF/Copernicus).

## Contact

Lucas Vituri Santarosa — Water Resources and Geotechnology Laboratory, Pontificia Universidad Católica de Valparaíso (PUCV).
