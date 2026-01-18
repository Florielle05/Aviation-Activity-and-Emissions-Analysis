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

TABLE: Preview of cleaned merged dataset  
<img width="737" height="337" alt="image" src="https://github.com/user-attachments/assets/3217e438-e5aa-4b16-97dd-5f0711587e30" />


---

## 4. Exploratory Data Analysis (EDA)
Exploratory analysis is conducted to understand the distribution of aviation activity and emissions across countries and over time.

The analysis focuses on:
- Distribution of aviation emissions
- Cross-country comparisons
- Identification of scale effects and outliers

---

## 5. Relationship Between Aviation Activity and Emissions
To assess whether emissions are primarily driven by activity scale, direct relationships between aviation activity indicators and emissions are analyzed.

## scatter plot of Aviation activity variables as a function of CO2 emission
```
sns.scatterplot(data = X, x = 'Total annual CO₂ emissions from aviation', y = 'Nombre de passagers', color = 'blue')
sns.scatterplot(data = X, x = 'Total annual CO₂ emissions from aviation', y = 'fret(TonnesMbyKm)', color = 'red')
sns.scatterplot(data = X, x = 'Total annual CO₂ emissions from aviation', y = 'Departs de Transporteur', color = 'green')
sns.scatterplot(data = X, x = 'Total annual CO₂ emissions from aviation', y = 'Carburant utilise', color = 'orange')
```
Result 
<img width="566" height="430" alt="image" src="https://github.com/user-attachments/assets/be35b389-0cd1-417f-acda-158f9cc21a32" />


The results show a **strong positive relationship** between aviation activity and emissions, suggesting that scale is a dominant driver.

---

## 6. Principal Component Analysis (PCA)
To move beyond individual variables, PCA is applied to aviation activity indicators in order to:
- Reduce dimensionality
- Separate **overall scale effects** from **structural differences**

A- Correlation matrix R
B- Sorted eigenvector matrix
C- Principal component matrix F
D- Saturation Matrix S
E- obtaining the main axes
<img width="402" height="361" alt="image" src="https://github.com/user-attachments/assets/f7e7d1bc-4a08-451b-b6be-83aa361a3e36" />

---

## 7. Interpretation of Principal Components
The saturation matrix allows us to project the axes and observe their dependencies on the previous variables. 
<img width="513" height="238" alt="image" src="https://github.com/user-attachments/assets/7712e4d6-e57f-434b-86de-ae3f40d96e9b" />
With this last one, we notice that:
- PC1 mainly captures overall aviation activity (high loadings on passengers and freight).
- PC2 reflects structural differences between passenger-dominated and freight-dominated countries.
- Countries with high PC1 scores are those with both high traffic and high emissions.

After cleaning, standardizing, studying, analyzing, and reducing the components of the data, we will study the relationship between aviation and total greenhouse gas emissions. The first step is to import the greenhouse gas data, clean it again:
```
data_gaz = pd.read_csv('/kaggle/input/gaz-effet-serre/API_EN.GHG.ALL.PC.CE.AR5_DS2_en_csv_v2_32372.csv',sep=",",
    quotechar='"',
    encoding='utf-8-sig',
    skiprows=4  )
data_gaz = data_gaz.loc[:,['Country Name','2013', '2014', '2015',	'2016',	'2017',	'2018',	'2019',	'2020']]
data_gaz = data_gaz.dropna()
data_gaz = data_gaz.melt(id_vars='Country Name', var_name='Year', value_name='Total greenhouse gas emissions')
data_gaz['Year'] = data_gaz['Year'].astype(int)
```
and combine it with the axes to create a single dataset. Here is a preview of it:
<img width="624" height="282" alt="image" src="https://github.com/user-attachments/assets/e99ef3ca-ef7b-4efc-a5a3-b378336f0993" />

After that, we create a line plot of the variables, axis 1, axis 2, and greenhouse gas emissions, and we obtain this:
<img width="554" height="417" alt="image" src="https://github.com/user-attachments/assets/9dd4af62-1b00-4c82-9d49-18d78d9df98a" />
The figure suggests a strong co-movement between total aviation-related greenhouse gas emissions and the first principal component (PC1), interpreted as an indicator of the overall scale of air traffic. In contrast, the second principal component (PC2), associated with structural differences between passenger- and freight-oriented systems, shows a weaker correlation with emissions. These results indicate that variations in emissions are more closely linked to the volume of activity than to the structure of air traffic, which supports the use of intensity indicators for relevant international comparisons.

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
