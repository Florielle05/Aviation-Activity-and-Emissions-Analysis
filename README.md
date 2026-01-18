# Aviation Activity and Emissions Analysis

## Project Overview

This project analyzes the relationship between aviation activity and aviation-related greenhouse gas emissions
across countries and years.

The objective is not only to explore correlations, but to derive **interpretable indicators**
that help understand what drives emissions and how countries can be meaningfully compared.

The analysis is based on a cleaned and merged dataset combining:
- aviation activity indicators (passengers, freight, carriers)
- aviation-related CO₂ / greenhouse gas emissions
- country-level and temporal information

---

## Key Questions

- How strongly are aviation emissions driven by the scale of activity?
- Do structural differences in aviation models (passenger vs freight-oriented) matter?
- Why are total emissions alone insufficient to compare countries fairly?

---

## Methodology

### 1. Data Preparation
- Cleaning and harmonization of multi-source aviation and emissions data
- Aggregation at the country-year level
- Handling of missing values and consistency checks

### 2. Exploratory Analysis
- Descriptive statistics and trend analysis
- Cross-country comparisons of aviation activity and emissions

### 3. Principal Component Analysis (PCA)
- PCA is applied to aviation activity variables to reduce dimensionality
- **PC1** captures the overall scale of aviation activity
- **PC2** reflects structural differences between passenger- and freight-oriented systems

The PCA results show that aviation emissions closely follow **PC1**, indicating that emissions
are primarily driven by activity volume rather than traffic structure.

### 4. Interpretable Indicators
To move beyond raw emissions, the project introduces intensity-based metrics such as:
- **CO₂ emissions per passenger**
- Share of global aviation emissions by country

These indicators allow fairer comparisons across countries with different aviation scales.

---

## Key Findings

- Aviation emissions are strongly correlated with the overall scale of aviation activity.
- Structural differences (passenger vs freight orientation) play a secondary role.
- A small number of countries account for a large share of global aviation emissions.
- Emissions per passenger vary significantly across countries, suggesting differences in efficiency.

---

## Repository Structure
```bash
.
├── notebook/
│ └── notebook-analyse.ipynb
├── data/
│ └── (cleaned datasets or data preparation instructions)
├── outputs/
│ └── figures and tables generated in the analysis
├── requirements.txt
└── README.md
```


---

## How to Run

Install dependencies:
```bash
pip install -r requirements.txt
```

Open the notebook:
```bash
jupyter notebook notebook/notebook-analyse.ipynb
```
---

## Limitations and Next Steps

The analysis focuses on aggregated country-level data and does not capture airline-level efficiency.

Results are descriptive and explanatory rather than predictive.

Future work could include scenario analysis or anomaly detection to identify over- or under-performing systems.

---

## Why This Project Matters
This project demonstrates the ability to:

clean and merge real-world datasets

apply dimensionality reduction meaningfully

move from exploratory analysis to decision-oriented indicators

communicate results clearly to non-technical stakeholders
