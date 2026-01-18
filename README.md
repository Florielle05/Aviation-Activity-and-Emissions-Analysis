# Aviation Activity and Greenhouse Gas Emissions Analysis

## 1. Problem Statement
The aviation sector is a significant and growing contributor to global greenhouse gas emissions.  
However, comparing countries solely based on **total aviation emissions** is misleading, as emission levels are strongly driven by the **scale of aviation activity**.

The objective of this project is to analyze the relationship between **aviation activity** and **aviation-related greenhouse gas emissions** at the **country–year level**, and to derive **interpretable indicators** that enable fair and meaningful cross-country comparisons.

---

## 2. Dataset
This project relies on the integration of multiple real-world datasets related to aviation, energy, and economic activity.

**Granularity**
- Country × Year

**Main data sources**
- Air passengers data
- Air freight data
- Transportation indicators
- Fuel consumption data
- GDP data
- Aviation-related CO₂ / GHG emissions data

**Key variables**
- `Country Name`
- `Year`
- `Nombre de passagers`
- `Fret`
- `Fuel consumption`
- `GDP`
- `Aviation emissions`

```bash
data1 = pd.read_csv('/kaggle/input/aerien1/annual-co-emissions-from-aviation.csv')
data_passagers = pd.read_csv(
    '/kaggle/input/donnee-ligne/API_IS.AIR.PSGR_DS2_fr_csv_v2_22687.csv',
    sep=",",
    quotechar='"',
    encoding='utf-8-sig',
    skiprows=4  # car souvent les 4 premières lignes sont des descriptions inutiles
)
data_fret = pd.read_csv(
    '/kaggle/input/fretmilliontonneskm/API_IS.AIR.GOOD.MT.K1_DS2_en_csv_v2_19498.csv',
    sep=",",
    quotechar='"',
    encoding='utf-8-sig',
    skiprows=4  # car souvent les 4 premières lignes sont des descriptions inutiles
)
data_transp = pd.read_csv(
    '/kaggle/input/transporteur/API_IS.AIR.DPRT_DS2_en_csv_v2_22688.csv',
    sep=",",
    quotechar='"',
    encoding='utf-8-sig',
    skiprows=4  # car souvent les 4 premières lignes sont des descriptions inutiles
)
data_pib = pd.read_csv('/kaggle/input/pibpays/API_NY.GDP.MKTP.CD_DS2_en_csv_v2_19294.csv',sep=",",
    quotechar='"',
    encoding='utf-8-sig',
    skiprows=4  )
data_carburant = pd.read_csv('/kaggle/input/carburant-new-sim/aviation_fuel_consumption_all_countries.csv', encoding='utf-8-sig')
```
---

## 3. Data Preparation
All datasets are cleaned, reshaped, and harmonized to ensure consistency across countries and years.

Main preprocessing steps include:
- Handling missing values
- Renaming and aligning country identifiers
- Converting year columns to numeric format
- Reshaping wide datasets using `melt`
- Merging datasets into a single country–year table
- Normalization of key variables for comparative analysis


******
TABLE: Preview of cleaned merged dataset  
→ `df.head()` or equivalent display of the final country–year dataset
******

---

## 4. Exploratory Data Analysis (EDA)
Exploratory analysis is conducted to understand the distribution of aviation activity and emissions across countries and over time.

The analysis focuses on:
- Distribution of aviation emissions
- Cross-country comparisons
- Identification of scale effects and outliers

******
GRAPH: Distribution of aviation emissions  
→ Histogram showing skewness and concentration of emissions
******

******
GRAPH: Cross-country comparison of aviation activity or emissions  
→ Bar chart or line plot highlighting differences between countries
******

---

## 5. Relationship Between Aviation Activity and Emissions
To assess whether emissions are primarily driven by activity scale, direct relationships between aviation activity indicators and emissions are analyzed.

******
GRAPH: Aviation activity vs emissions  
→ Scatter plot (e.g. passengers vs emissions)
******

The results show a **strong positive relationship** between aviation activity and emissions, suggesting that scale is a dominant driver.

---

## 6. Principal Component Analysis (PCA)
To move beyond individual variables, PCA is applied to aviation activity indicators in order to:
- Reduce dimensionality
- Separate **overall scale effects** from **structural differences**

******
CODE: PCA computation  
→ Cells performing normalization, PCA fitting, and projection
******

******
GRAPH: PCA explained variance  
→ Cumulative explained variance plot
******

******
GRAPH: PCA projection (PC1 vs PC2)  
→ Scatter plot of countries in PCA space
******

**Interpretation**
- **PC1** captures the overall scale of aviation activity  
- **PC2** reflects structural differences (e.g. passenger-oriented vs freight-oriented systems)

******
GRAPH: Emissions vs PC1  
→ Scatter plot showing alignment between emissions and activity scale
******

---

## 7. Interpretable Indicators
To allow fairer comparisons between countries, intensity-based indicators are constructed, such as:
- Emissions per passenger
- Relative contribution to global aviation emissions

******
CODE: Indicator construction  
→ Cells computing emissions intensity metrics
******

******
TABLE or GRAPH: Emissions per passenger by country  
→ Bar chart or ranked table
******

These indicators reveal substantial heterogeneity across countries with similar total emission levels.

---

## 8. Limitations
- The analysis is **descriptive**, not causal  
- Country-level aggregation hides airline-level efficiency differences  
- Regulatory, technological, and fuel-mix effects are not explicitly modeled  

---

## 9. What I Learned
Through this project, I developed strong practical skills in:
- Cleaning and merging heterogeneous real-world datasets
- Structuring exploratory analyses around clear questions
- Using PCA as an **interpretation tool**, not just a dimensionality reduction method
- Designing **decision-oriented indicators** beyond raw totals
- Communicating quantitative insights clearly while acknowledging limitations

---

## 10. How to Run
Install dependencies:
```bash
pip install -r requirements.txt
