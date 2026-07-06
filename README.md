# 🇦🇷 Argentina Demographics Analysis (1869 - 2022)

This repository contains demographic data and analytics for Argentina's population census from the first census in **1869** to the most recent one in **2022**. It features notebooks that explore the composition of the population by sex, age range, and place of birth/origin, and generates population pyramids for individual provinces and census years.

The dataset is sourced from the **INDEC** (Instituto Nacional de Estadística y Censos - Argentina's Statistics Agency) and was compiled/uploaded by Jorge Nicolau on [Kaggle](https://www.kaggle.com/datasets/jorquenlau/argentina-demographics-1869-2022).

---

## 📂 Repository Contents

The workspace includes the following key files:
- [pad_argentina.csv](file:///Users/jenic/Documents/argentina_demographics/pad_argentina.csv): The primary dataset containing demographic counts across census years, provinces, age groups, sex, and native/foreign-born origin.
- [Argentina_Demographics.ipynb](file:///Users/jenic/Documents/argentina_demographics/Argentina_Demographics.ipynb): A Jupyter notebook showing how to load the dataset efficiently using the `polars` library.
- [argentina_demographics](file:///Users/jenic/Documents/argentina_demographics/argentina_demographics): A Jupyter notebook containing exploratory data analysis, metadata inspection, and visualization of population pyramids using `pandas`, `seaborn`, and `matplotlib`.

---

## 📊 Dataset Schema (`pad_argentina.csv`)

The dataset comprises **19,008 rows** and **8 columns**. The schema is structured as follows:

| Column Name | Data Type | Description | Key Values & Meanings |
| :--- | :--- | :--- | :--- |
| `censo` | Integer | The year the census was conducted. | 11 census years: `1869`, `1895`, `1914`, `1947`, `1960`, `1970`, `1980`, `1991`, `2001`, `2010`, `2022`. |
| `iso_3166-2_AR` | String | ISO 3166-2 code corresponding to the province. | `AR` (Total Country), `AR-B` (Buenos Aires), `AR-C` (CABA), etc. (25 unique values). |
| `provincia` | String | The name of the Argentine province. | E.g., `ARGENTINA`, `BUENOS AIRES`, `TUCUMAN`, `SANTA FE`, etc. |
| `sexo` | String | Gender designation in the census records. | `M` = Mujer (Female)<br>`V` = Varón (Male) |
| `rango_etario` | String | Age group/range for the population bucket. | E.g., `00-04`, `05-09`, `10-14`, ..., `80-84`, `85+`. |
| `nativo_extranjero`| String | Place of birth classification. | `N` = Nativo (Native-born)<br>`E` = Extranjero (Foreign-born) |
| `poblacion` | Integer | Total count of individuals in this segment. | Population count. |

---

## 🔍 Analytics & Methodology

The notebook [argentina_demographics](file:///Users/jenic/Documents/argentina_demographics/argentina_demographics) performs the following analytical steps:

1. **Unique values check**: Outlines all available census years, provinces, and ISO codes.
2. **Province Name Mapping**: Includes a utility function `find_provincia_by_iso_code(iso_code, df)` to resolve ISO codes to full Spanish province names.
3. **Data Aggregation**: Groups the population counts by age group and gender for a selected province and year.
4. **Pyramid Construction**: Negates the population values of one gender to plot them on the left side of the vertical axis, creating the signature demographic pyramid shape using Seaborn bar plots.

### ⚠️ Important: Gender Swap Bug in Notebook Visualizations
> [!WARNING]
> In the original notebook [argentina_demographics](file:///Users/jenic/Documents/argentina_demographics/argentina_demographics) (Cell 4), there is a bug in how pivoted columns are renamed, causing the **genders to be swapped** in the population pyramid plots.
> 
> **How the bug occurs:**
> When the table is pivoted on the `sexo` column containing `M` (Mujer) and `V` (Varón), Pandas orders the pivoted columns alphabetically: `['M', 'V']`.
> The notebook renames columns using:
> ```python
> df_pivot.columns = ['grupo_etario', 'males', 'females']
> ```
> This maps `M` (Mujer/Female) to `males` and `V` (Varón/Male) to `females`, swapping the true demographics.
> 
> **How to fix it:**
> Update the renaming line in your code to correctly align Spanish prefixes to English labels:
> ```python
> # Correct renaming mapping M -> females, V -> males
> df_pivot = df_pivot.rename(columns={'M': 'females', 'V': 'males'})
> ```

---

## 📌 Historical Census Compatibility Notes

Due to over 150 years of territorial expansion, geopolitics, and changing methodologies, direct comparison between censuses should account for the following notes (as compiled by INDEC):

*   **1869 (First Census)**: Excludes areas not yet under national control (current provinces of *Chaco, Misiones, Formosa, La Pampa, Neuquén, Río Negro, Chubut, Santa Cruz, and Tierra del Fuego*). Also excludes Welsh colony of Rawson and estimates of the Indigenous population.
*   **1895**: Excludes estimates of the Indigenous population. Age/gender demographics are not available for *Santa Cruz* and *Tierra del Fuego*.
*   **1914**: Excludes estimates of the Indigenous population. Includes the territory of *Los Andes*, which was subsequently redistributed to *Salta, Jujuy, and Catamarca* to maintain temporal comparison.
*   **1947**: Includes the military zone of *Comodoro Rivadavia*, which was later split/redistributed between *Chubut* and *Santa Cruz*.
*   **1960**: Excludes the population of *Antarctica* and the *Falkland Islands* to maintain comparability with other censuses.
*   **1970**: Extrapolated from a sample rather than processing the entire population. Excludes the *Falkland Islands*.
*   **2022**: Population by origin, age, and sex was surveyed only for private households, excluding collective households.
*   **Age "Unknowns"**: The unknown age entries in the censuses of `1895`, `1914`, `1947`, `1960`, and `1991` have been distributed proportionally to clean the visualization data.

---

## 🚀 Setup and Usage

To run the analysis and reproduce or customize the population pyramids, set up a Python environment with the following packages:

```bash
pip install numpy pandas polars seaborn matplotlib
```

### Running the Pyramids
Open [argentina_demographics](file:///Users/jenic/Documents/argentina_demographics/argentina_demographics) (or your preferred editor) and adjust the parameters to draw pyramids for any year or province:

```python
censo = 2022             # E.g., 1869, 1914, 1980, 2022
provincia = 'AR-C'       # E.g., 'AR' (Countrywide), 'AR-B' (Buenos Aires), 'AR-C' (CABA)
```
