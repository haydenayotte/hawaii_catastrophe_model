# Hawaii Flood Risk Analysis
### Catastrophe Modeling — Oahu & Maui | April 2026

![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python&logoColor=white)
![CLIMADA](https://img.shields.io/badge/Platform-CLIMADA%20v6-green)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

---

## Overview

On March 13, 2026, a powerful Kona Low storm struck the Hawaiian Islands with wind gusts of 70–100 mph, causing the worst flooding in Hawaii in over 20 years. Governor Josh Green estimated damages exceeding **$1 billion** across the public and private sectors. Hundreds of residents were rescued from floodwaters on Oahu's North Shore and Maui's South Coast, including a partial collapse of the Kihei Kai condominium building in Kihei, Maui.

This project uses the **CLIMADA open-source catastrophe modeling platform** (ETH Zurich) to quantify the broader flood risk exposure across Oahu and Maui — placing the March 2026 event in the context of longer return-period tail risk, and estimating the **insurance protection gap** facing the state.

---

## Key Findings

| Metric | Value |
|---|---|
| Total exposed asset value (Hawaii) | **$183.8 billion** |
| Assets in FEMA flood zones | **$131.5 billion** (71.5%) |
| Average Annual Loss (AAL) | **$260 million** |
| 100-year event gross loss | **$20.0 billion** |
| 500-year event gross loss | **$30.0 billion** |
| Estimated insured loss (100yr, ~20% NFIP) | **$4.0 billion** |
| **Insurance protection gap (100yr)** | **$16.0 billion (80%)** |

> The March 2026 Kona Low (~$1B reported damage) falls well below the modeled 100-year threshold — consistent with the governor's description of it as the "worst flooding in 20 years," implying a roughly 1-in-20 year frequency event. The 100-year tail risk is approximately 20× the March event.

---

## Methodology

This project follows the standard catastrophe modeling framework:

```
Hazard  ×  Exposure  ×  Vulnerability  =  Impact
```

### Hazard
Flood hazard is represented using **FEMA Flood Insurance Rate Map (FIRM)** zone classifications for Oahu and Maui. Each zone is assigned an inundation depth based on its regulatory classification (VE: 3.0m, AE: 1.5–2.5m, X shaded: 0.5–1.0m). Two return period scenarios are modeled: **100-year** (1% annual exceedance probability) and **500-year** (0.2% AEP).

*Note: CLIMADA's global ISIMIP river flood dataset was evaluated but produces no signal for Hawaii — a known limitation of global hydrological models that require large river basin drainage areas. Hawaii's steep, small watersheds fall below the resolution threshold of this framework. FEMA FIRM data is the regulatory standard used by the U.S. insurance industry.*

### Exposure
Economic exposure sourced from **CLIMADA LitPop** (produced capital, 2020 baseline), accessed via the CLIMADA Data API. LitPop estimates asset values spatially using nighttime light intensity and population data, calibrated to national GDP figures.

### Vulnerability
Depth-damage functions follow the **JRC Global Flood Depth-Damage Function for North America (RF4)**, from Huizinga et al. (2017). These map flood depth (meters) to damage fraction (0–100%).

### Platform
[CLIMADA](https://climada.ethz.ch/) (CLIMate ADAptation) — open-source catastrophe modeling platform developed by ETH Zurich's Weather and Climate Risks Group.

---

## Repository Structure

```
hawaii-flood-risk-analysis/
│
├── hawaii_flood_risk.ipynb          # Full analysis notebook (run in Colab)
├── hawaii_flood_risk_report.html    # Polished PDF-ready report
├── README.md                        # This file
│
└── outputs/                         # Generated figures (produced by notebook)
    ├── fig1_vulnerability_curve.png
    └── fig2_hawaii_flood_risk_dashboard.png
```

---

## Running the Notebook

The notebook is designed to run in **Google Colab** with no local setup required.

**1. Open in Colab**

Click the badge below or upload `hawaii_flood_risk.ipynb` directly to [colab.research.google.com](https://colab.research.google.com):

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)

**2. Run Cell 1**

Cell 1 installs all dependencies automatically:
```python
!pip install climada climada-petals geopandas -q
```

**3. Run all cells in order**

The notebook downloads exposure data from the CLIMADA Data API (~500MB, cached after first run). Expect 5–10 minutes on first run.

**Requirements (if running locally)**
```
python >= 3.10
climada >= 6.0
climada-petals >= 6.0
geopandas
scipy
matplotlib
numpy
pandas
```
> For local installation, use `conda install -c conda-forge geopandas climada` rather than pip to avoid GDAL dependency issues on Mac/Windows.

---

## Limitations

This is a **rapid ad-hoc assessment** — results are order-of-magnitude estimates, not production model outputs. Key limitations:

- **22 manually placed flood zone points** represent the hazard. A production model uses a continuous, physically simulated inundation surface.
- **Flood depths are assumed** by FEMA zone type, not derived from hydraulic simulation.
- **Peril mismatch:** The March 2026 Kona Low was driven by flash flooding from extreme rainfall on steep terrain — a different physical process than the slow-onset river flooding the JRC depth-damage functions were calibrated for.
- **LitPop is a GDP proxy**, not a property database. It does not distinguish building type, construction quality, or replacement cost.
- **EP curve has only two anchor points** (100yr and 500yr), interpolated log-linearly. A robust EP curve requires a full stochastic event set.
- **NFIP penetration rate** (20%) is an estimate from public FEMA statistics. Actual rates vary by zip code and property type.

---

## Tools & Data Sources

| Component | Source |
|---|---|
| Modeling platform | [CLIMADA v6](https://climada.ethz.ch/) — ETH Zurich |
| Economic exposure | CLIMADA LitPop v3 (produced capital, 2020) |
| Flood hazard zones | FEMA FIRM / DFIRM — National Flood Hazard Layer |
| Vulnerability functions | JRC RF4 North America — Huizinga et al. (2017) |
| Event context | Hawaii Emergency Management Agency, CNN, Civil Beat |

---

## References

- Aznar-Siguan, G. & Bresch, D. N. (2019). CLIMADA v1: a global weather and climate risk assessment platform. *Geosci. Model Dev.*, 12, 3085–3097.
- Bresch, D. N. & Aznar-Siguan, G. (2021). CLIMADA v1.4.1: towards a globally consistent adaptation options appraisal tool. *Geosci. Model Dev.*, 14, 351–363.
- Huizinga, J., Moel, H. de & Szewczyk, W. (2017). Global flood depth-damage functions: Methodology and the Database with Guidelines. JRC. doi: 10.2760/16510.
- FEMA (2021). Flood Hazard Areas (DFIRM) — State of Hawaii. National Flood Hazard Layer.
- Hawaii Emergency Management Agency (2026). March 2026 Kona Low Storm Recovery.

---

## Disclaimer

This project was produced using open-source tools and publicly available data for educational and portfolio purposes. Results are preliminary estimates only and should not be used for underwriting, pricing, regulatory, or investment decisions without independent validation against production-grade catastrophe models.

---

*Built with [CLIMADA](https://climada.ethz.ch/) | ETH Zurich Weather and Climate Risks Group*
