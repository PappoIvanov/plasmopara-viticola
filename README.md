# Grapevine Downy Mildew and Climate — A Data Science Analysis

**SoftUni Data Science  — Final Exam Project**
**Author:** Pavel Ivanov

> **Interactive notebook:** The main analysis notebook — including all interactive Plotly charts and the regional risk dashboard — can be viewed in full at [nbviewer.org](https://nbviewer.org/github/PappoIvanov/plasmopara-viticola/blob/main/notebooks/grapevine_downy_mildew_analysis.ipynb). GitHub does not render interactive visualizations; use this link for the full experience.

---

## Research Question

> Under which meteorological conditions does *Plasmopara viticola* (grapevine downy mildew) appear in European vineyards, and which periods of the year carry the highest infection risk for Bulgarian wine-growing regions?

---

## Background

*Plasmopara viticola* is the oomycete responsible for downy mildew — one of the most economically damaging grapevine diseases in Europe. Its life cycle is tightly coupled to weather: primary infections require a minimum temperature, leaf wetness, and rainfall. This makes it a suitable subject for data-driven climate analysis.

The classical **Goidanich Rule (1959)** defines the conditions for primary infection:

```
Infection risk is high when:
  - Mean temperature ≥ 11 °C
  - Cumulative rainfall ≥ 10 mm over 24–48 h
  - Grapevine shoot length ≥ 10 cm (phenological threshold)
```

This project tests whether observed occurrence data and historical weather records are consistent with this rule, and extends the analysis to four Bulgarian wine-growing regions.

---

## Data Sources

| # | Source | Description | Format |
|---|--------|-------------|--------|
| 1 | [PLOS ONE — Gourment et al. (2020)](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0230254) | 156 vineyard site-years from Bordeaux (2010–2018): monthly temperature, precipitation, and binary disease outcome | `.xlsx` |
| 2 | [GBIF — Global Biodiversity Information Facility](https://www.gbif.org/species/3204123) | 670 occurrence records of *P. viticola* across Europe, with coordinates and observation dates | `.csv` / `.json` |
| 3 | [Open-Meteo Historical Weather API](https://open-meteo.com/en/docs/historical-weather-api) | ERA5-based daily weather (2010–2023) for four Bulgarian wine regions: temperature, precipitation, relative humidity, wind speed, ET₀ | `.csv` |

All raw data files are stored in `data/raw/` and are never modified. Any transformations are saved separately in `data/processed/`.

---

## Project Structure

```
Plasmopara Viticola/
├── README.md
├── requirements.txt
├── data/
│   ├── raw/                  # Original, unmodified source files
│   ├── processed/            # Cleaned and transformed data
│   └── cleaned/
├── notebooks/
│   └── grapevine_downy_mildew_analysis.ipynb   # Main analysis notebook
├── references/               # Papers, methodology notes
└── notes/
```

---

## Methods Overview

1. **Exploratory Data Analysis (EDA)** — distributions, correlations, temporal and spatial patterns
2. **Data Cleaning & Hygiene** — handling missing values, type corrections, outlier inspection
3. **Weather Matching** — linking each GBIF occurrence to local weather conditions via the Open-Meteo API
4. **Hypothesis Testing** — Mann-Whitney U test and Welch's t-test to compare weather conditions at outbreak vs. non-outbreak periods
5. **Logistic Regression** — modeling probability of high disease incidence as a function of monthly weather variables (Bordeaux dataset)
6. **Risk Assessment** — applying learned thresholds to Bulgarian climate data to identify seasonal risk windows
7. **Interactive Dashboard** — Plotly visualisation of infection risk by region, year and month for four Bulgarian wine-growing regions

---

## Mathematical Models

### Goidanich Primary Infection Rule
$$\text{Risk} = 1 \iff T_{mean} \geq 11°C \;\land\; P_{24h} \geq 10\text{ mm}$$

### Logistic Regression
$$P(\text{high incidence}) = \frac{1}{1 + e^{-(\beta_0 + \beta_1 T_{May} + \beta_2 P_{Jun} + \ldots)}}$$

### Goidanich Risk Index (daily)
$$RI_t = \mathbf{1}[T_t \geq 11] \cdot \mathbf{1}[P_t \geq 10]$$

Seasonal risk is computed as the cumulative sum of daily risk flags over the growing season (April–September).

---

## How to Run

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Launch JupyterLab
jupyter lab

# 3. Open the notebook
# notebooks/grapevine_downy_mildew_analysis.ipynb
```

---

## Key Findings

- **89.1%** of GBIF occurrence records were observed under temperatures ≥ 11 °C, consistent with the Goidanich primary infection threshold.
- All four weather variables tested (May/June precipitation, April/May temperature) show statistically significant differences between high- and low-incidence site-years in the Bordeaux dataset (**p < 0.001**, both Welch's t-test and Mann-Whitney U).
- A logistic regression model trained on spring weather variables achieves **ROC-AUC = 0.736**, confirming meaningful predictive signal.
- Among four Bulgarian wine regions (2010–2023), the **Danube Plain (Pleven)** accumulates the highest infection risk (133 Goidanich risk days), with **June** as the peak risk month across all regions.
- A 1–2 month temporal lag is observed between peak infection-risk conditions (May–June) and peak symptom observations in GBIF data (July–August), consistent with the known incubation period of *P. viticola*.

---

## References

- Gourment, M. et al. (2020). *Forecasting severe grape downy mildew attacks using machine learning.* PLOS ONE. https://doi.org/10.1371/journal.pone.0230254
- Goidanich, G. (1959). *Manuale di Patologia Vegetale.* Edizioni Agricole, Bologna.
- GBIF.org (2026). *iNaturalist Research-grade Observations — Plasmopara viticola occurrence records.* https://www.gbif.org/dataset/50c9509d-22c7-4a22-a47d-8c48425ef4a7
- iNaturalist (2026). *iNaturalist Research-grade Observations.* California Academy of Sciences & National Geographic Society. https://www.inaturalist.org
- Hersbach, H. et al. (2020). *The ERA5 global reanalysis.* Quarterly Journal of the Royal Meteorological Society, 146(730), 1999–2049. https://doi.org/10.1002/qj.3803
- Open-Meteo (2026). *Historical Weather API — ERA5 reanalysis data.* https://open-meteo.com/en/docs/historical-weather-api
- Salinari, F. et al. (2006). *Downy mildew (Plasmopara viticola) epidemics on grapevine under climate change.* Global Change Biology.

---

## License

This project is submitted as academic work for SoftUni. Data sources are used under their respective open licenses (CC-BY, CC0).