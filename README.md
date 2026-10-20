# Money and Happiness in America: Income, Subjective Well-Being, and Economic Growth (1972–2024)

**Authors / Maintainers:**
- Kelden Jamtsho

**Course:** Data Science & Data Visualization  

---

## 1. Problem & Research Question

Does money buy happiness, and does national economic growth make a society happier over time? 

This project investigates the relationship between economic resources and subjective well-being in the United States over a 52-year period (1972–2024). Specifically, it examines the **Easterlin Paradox**—the divergence between cross-sectional income-happiness gradients and long-run national trends.

### Research Question
> *In the United States of America, how is family income associated with self-reported happiness, and how has this relationship, and the national level of reported happiness, changed between 1972 and 2024 as the country became substantially richer?*

---

## 2. Dataset Source & Usage Permissions

This study combines individual-level survey data with macro-level national economic indicators:

### 1. General Social Survey (GSS)
* **Provider:** NORC at the University of Chicago
* **Coverage:** 35 survey waves (1972–2024), $N = 64,848$ usable respondents after validity filtering.
* **Key Variables:** `HAPPY` (self-reported happiness), `REALINC` (inflation-adjusted family income), `AGE`, `SEX`, `EDUC`.
* **Access & Permissions:** Publicly available for academic and research use. 
  * [GSS Data Download Page](https://gss.norc.org/Get-The-Data)
  * [GSS Cumulative Stata File](https://gss.norc.org/content/dam/gss/get-the-data/documents/stata/GSS_stata.zip)

### 2. World Bank GDP Per Capita
* **Provider:** World Bank Open Data
* **Indicator:** `NY.GDP.PCAP.KD` (GDP per capita in constant 2015 USD).
* **Coverage:** Annual series for the United States (1972–2024).
* **Access & Permissions:** Open data under the [Creative Commons Attribution 4.0 International License (CC-BY 4.0)](https://datacatalog.worldbank.org/public-licenses).
  * [World Bank Indicator Page](https://data.worldbank.org/indicator/NY.GDP.PCAP.KD?locations=US)
  * Direct API Endpoint: `https://api.worldbank.org/v2/country/USA/indicator/NY.GDP.PCAP.KD?format=json&per_page=1000&date=1972:2024`

---

## 3. Methods & Tools Used

### Tools & Libraries
* **Language:** Python 3.x
* **Data Processing:** `pandas`, `numpy`
* **Statistical Analysis:** `scipy.stats`, `statsmodels` (OLS regression, Chi-Square tests, correlation analysis)
* **Visualization:** `matplotlib`, `seaborn`, HTML/CSS/JS (for interactive dashboard generation)
* **Environment:** Jupyter Notebook

### Analytical Workflow
1. **Data Cleaning & Filtering:** Filtered raw GSS data for valid responses in `HAPPY` and `REALINC`. Recoded happiness into a 3-point ordinal scale ($1 = \text{not too happy}$, $2 = \text{pretty happy}$, $3 = \text{very happy}$).
2. **Income Normalization:** Because income deflators and survey bands vary across survey waves, family income (`REALINC`) was converted into **within-year income quintiles** (poorest, 2nd, middle, 4th, richest).
3. **Cross-Sectional Analysis:** Computed contingency tables, Chi-Square tests ($\chi^2$), Cramér's $V$, and Odds Ratios (OR) with $95\%$ Confidence Intervals ($95\%$ CI) to evaluate relative income differences. OLS regression was used to control for demographic factors (age, sex, education).
4. **Time-Series Analysis:** Tracked longitudinal changes in the share of "very happy" Americans against constant-dollar GDP per capita growth using Spearman rank correlation ($\rho$) and Kendall rank correlation ($\tau$).

---

## 4. Key Findings & Primary Limitation

### Key Findings
1. **Within-Year Income Gradient:** Within any given year, relative family income is strongly and positively associated with subjective happiness ($\chi^2(8) = 2861.3, p < 1 \times 10^{-300}$, Cramér's $V = 0.149$).
   * $39.7\%$ of respondents in the richest quintile report being "very happy," compared to only $20.7\%$ in the poorest quintile (Odds Ratio $= 2.52, 95\%$ CI $[2.39, 2.66]$).
2. **The Long-Run Paradox:** While real US GDP per capita grew **2.49×** (from $\approx \$26,799$ in 1972 to $\$66,857$ in 2024), the proportion of "very happy" Americans declined overall from $30.3\%$ to $19.8\%$.
   * A strong negative association exists between national GDP per capita growth and the percentage of "very happy" citizens (Spearman $\rho = -0.663, p < 0.0001$).
3. **Growing Income Divide:** The happiness gap between the top and bottom income quintiles has widened noticeably since the 2000s.

### Primary Limitation
* **Survey Mode Changes & Period Effects:** A major limitation is the GSS methodological shift in recent waves (2020–2024), transitioning from traditional face-to-face interviews to web and hybrid administration, alongside COVID-19 pandemic dislocations. These mode effects introduce potential measurement bias that may artificially accentuate the post-2020 decline in reported happiness independent of economic trends.

---

## 5. Instructions for Running the Analysis

### Prerequisites
Ensure you have Python 3.8+ installed along with the required packages:

```bash
pip install pandas numpy matplotlib seaborn scipy statsmodels requests jupyter
```

### Step-by-Step Execution

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/KELDENJAMTSHOMC664183/CISC7204.git
   cd CISC7204
   ```

2. **Download & Prepare Datasets:**
   * Download the cumulative GSS Stata dataset (`GSS_stata.zip`) from [NORC](https://gss.norc.org/Get-The-Data) and extract `gss_cum.dta` into the project root or `data/` directory.
   * The World Bank GDP data will be fetched automatically via API during notebook execution.

3. **Run the Jupyter Notebook:**
   ```bash
   jupyter notebook
   ```
   * Open the primary notebook (`.ipynb`) in your browser.
   * Run all cells sequentially (`Cell` -> `Run All`) to reproduce data cleaning, statistical tests, static figures, and the interactive HTML summary page.

---

## Repository Structure

```text
.
├── data/                  # Local directory for raw/processed datasets
├── figures/               # Generated static figures (PNG/SVG)
├── index.html             # Interactive HTML results dashboard
├── analysis_notebook.ipynb# Primary Python 3 Jupyter Notebook
└── README.md              # Project documentation