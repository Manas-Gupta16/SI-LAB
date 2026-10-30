# Global Billionaires Wealth Distribution — Statistical Inference Lab

## Academic Lab Portfolio (Practicals 1 to 8)

**Student Name:** Manas Gupta  
**Roll / Batch ID:** B1_B3_38 (Batch B1 / B3)  
**Subject:** Statistical Inference Lab (`SI-LAB`)  
**Academic Year:** 2026 – 2027  
**Primary Deliverable:** [`global_billionaires_statistical_inference.ipynb`](./global_billionaires_statistical_inference.ipynb)  
**Primary Dataset:** [`global_billionaires.csv`](./global_billionaires.csv)  

---

## 1. Project Overview & Objectives

Billionaire wealth is one of the most prominent real-world demonstrations of extreme economic inequality and heavy-tailed statistical distributions. While typical physical and demographic variables (such as heights or test scores) follow symmetric Gaussian (normal) distributions, individual wealth exhibits profound positive skewness and power-law dynamics, where an ultra-small fraction of individuals possesses an overwhelming percentage of total wealth.

This laboratory project adapts all **eight prescribed practicals** from the college *Statistical Inference Lab* manual to a unified real-world dataset: **Global Billionaires Wealth Distribution**. 

### Key Objectives
1. **Data Acquisition & Engineering:** Scrape, parse, clean, and validate public records of the world's billionaires.
2. **Exploratory Data Analysis:** Quantify central tendency and dispersion, and visualize distributions and categorical breakdowns.
3. **Probability Modeling & Sampling:** Fit theoretical probability models (Normal, Uniform, Binomial, Poisson) and implement 4 sampling methods (SRS, Stratified, Systematic, Cluster).
4. **Empirical Verification of CLT:** Demonstrate the Central Limit Theorem across sample sizes ($n = 5, 30, 100$) using Monte Carlo simulation.
5. **Parametric Estimation & Testing:** Construct 95% Confidence Intervals and conduct One-Sample and Independent Two-Sample $t$-tests.
6. **Advanced Hypothesis Testing:** Formulate and execute Paired $t$-tests, One-Way ANOVA across industries, and Chi-Square Tests of Independence.
7. **Bivariate Association:** Compute Covariance, Pearson linear correlation ($r$), and Spearman rank correlation ($\rho$), analyzing the effects of non-linearity and outliers.
8. **Predictive Modeling:** Train and evaluate Simple Linear Regression, Regularized Regression (Ridge & Lasso), and Decision Tree Regressor using train/test splits.

---

## 2. Dataset Documentation & Metadata

The project utilizes authentic, publicly available records from the **Forbes World's Billionaires List** (2023 edition):

- **File Name:** [`global_billionaires.csv`](./global_billionaires.csv)
- **Total Records:** 2,640 unique billionaires
- **Total Attributes:** 9 columns
- **Primary Monetary Variable:** `Net_Worth_Billion_USD` (measured in Billions of United States Dollars, \$B)

### Schema & Attributes

| Column Name | Data Type | Description | Example Values |
| :--- | :--- | :--- | :--- |
| `Rank` | Integer | Global wealth rank | 1, 2, 3, ..., 2540 |
| `Name` | String | Full legal or public name | Bernard Arnault, Elon Musk |
| `Net_Worth_Billion_USD` | Float | Net worth in billions of USD | 211.0, 180.0, 4.62, 1.0 |
| `Age` | Float | Age in years (65 missing values imputed via median) | 74.0, 51.0, 65.0 |
| `Country` | String | Country of residence / primary citizenship | United States, China, India, France |
| `Industry` | String | Economic sector / business category | Technology, Fashion & Retail, Finance |
| `Self_Made` | Boolean | True = Founder/Entrepreneur; False = Inherited | `True`, `False` |
| `Gender` | String | Recorded gender ('M' for Male, 'F' for Female) | 'M', 'F' |
| `Source` | String | Principal company or wealth origin | LVMH, Tesla/SpaceX, Amazon |

### Dataset Limitations
1. **Extreme Upper Tail Focus:** The dataset captures only the top 0.00003% of the global population and cannot represent the wealth distribution of the general public.
2. **Volatile Market Capitalization:** Net worth calculations depend on daily equity valuations, foreign exchange rates, and private enterprise multiples.
3. **Missing Demographic Fields:** 65 individuals had undisclosed ages; median imputation (65.0 years) was applied to retain sample power without distorting distribution shape.

---

## 3. Practical-by-Practical Mapping to the Lab Manual

Every practical strictly reflects the methods, objectives, and code patterns prescribed in the Statistical Inference Lab manual:

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       STATISTICAL INFERENCE LAB MAP                         │
├─────────────┬───────────────────────────────────────┬───────────────────────┤
│ Practical   │ Lab Manual Topic                      │ Billionaires Project  │
├─────────────┼───────────────────────────────────────┼───────────────────────┤
│ Practical 1 │ Web Scraping & Data Extraction        │ Table Scraping & CSV  │
│ Practical 2 │ Descriptive Statistics & Plots        │ Wealth Centrality/IQR │
│ Practical 3 │ Probability Distributions & Sampling  │ 4 Models & 4 Samplers │
│ Practical 4 │ Central Limit Theorem (CLT)           │ MC Skewed Convergence │
│ Practical 5 │ CI Estimation & t-Tests               │ 95% CI & 1/2-Sample t │
│ Practical 6 │ Paired t-Test, ANOVA & Chi-Square     │ Industry/Self-Made H0 │
│ Practical 7 │ Covariance & Correlation (Pearson/ρ)  │ Age vs Worth vs Rank  │
│ Practical 8 │ Regression Analysis & ML Regressors   │ OLS, Ridge, Tree Fit  │
└─────────────┴───────────────────────────────────────┴───────────────────────┘
```

### Practical 1: Web Scraping, Data Extraction, and Dataset Preparation
- **Aim:** Extract tabular data using `requests` and `BeautifulSoup`, structure unorganized HTML into a Pandas DataFrame, clean missing values, and export the validated dataset.
- **Key Method:** DOM parsing via `soup.find_all('tr')`, type conversion, median imputation for missing `Age`, and offline/online fallback mechanisms.
- **Viva Point:** Why median imputation? The median is unaffected by extreme outliers in age or wealth, whereas the mean is distorted by extreme values.

### Practical 2: Exploratory Data Analysis & Descriptive Visualizations
- **Aim:** Calculate measures of central tendency ($\bar{x}$, Median, Mode) and dispersion (Range, $s^2$, $s$, $IQR$), and generate visual plots.
- **Key Results:**
  - **Mean Net Worth:** \$4.62 Billion
  - **Median Net Worth:** \$2.30 Billion
  - **Standard Deviation:** \$9.83 Billion (Variance = 96.71)
  - **Interquartile Range ($IQR$):** \$2.70 Billion ($Q_1 = \$1.50B$, $Q_3 = \$4.20B$)
  - **Skewness:** 12.35 (Extremely positive/right-skewed)
- **Plots Generated:** Wealth Histogram with KDE and Mean/Median markers, Outlier Boxplot, Top 10 Countries Bar Chart, Self-Made vs Inherited Pie Chart, Age vs Wealth Scatter.

### Practical 3: Probability Distributions and Random Sampling Techniques
- **Aim:** Simulate theoretical probability models and implement 4 sampling methods.
- **Distributions Evaluated:**
  - *Normal Distribution:* Modeled on Billionaire Age ($\mu = 65.1$ yrs, $\sigma = 13.1$ yrs).
  - *Uniform Distribution:* Flat probability density benchmark over $[1.0B, 20.0B]$.
  - *Binomial Distribution:* Discrete successes in $n = 20$ trials with self-made probability $p = 0.686$.
  - *Poisson Distribution:* Discrete rare billionaire counts in small nations ($\lambda = 2.5$).
- **Sampling Methods Implemented:** Simple Random Sampling ($n=120$), Stratified Sampling (by Industry), Systematic Sampling ($k = N/n$), Cluster Sampling (random country clusters).
- **Viva Point:** Stratified sampling minimizes sample error by ensuring every industrial sector is proportionately represented.

### Practical 4: Demonstration and Verification of the Central Limit Theorem (CLT)
- **Aim:** Empirically demonstrate that the sampling distribution of the sample mean converges to normality for sample sizes $n = 5, 30, 100$ using 1,000 Monte Carlo repetitions.
- **Theoretical Verification:**
  - $n = 5$: Observed $SD = 4.29B$ vs Theoretical $SE = \sigma/\sqrt{5} = 4.39B$ (slight right skew persists).
  - $n = 30$: Observed $SD = 1.76B$ vs Theoretical $SE = \sigma/\sqrt{30} = 1.79B$ (symmetric bell curve).
  - $n = 100$: Observed $SD = 0.98B$ vs Theoretical $SE = \sigma/\sqrt{100} = 0.98B$ (Gaussian convergence).
- **Viva Point:** CLT guarantees normality of the sample mean even when the parent population is severely skewed, provided $n \ge 30$.

### Practical 5: Point Estimation, Confidence Intervals, and Hypothesis Testing
- **Aim:** Compute 95% Confidence Intervals and conduct One-Sample and Independent Two-Sample $t$-tests.
- **Key Findings:**
  - **95% Confidence Interval for Mean Wealth:** **(\$4.249B, \$4.999B)**.
  - **One-Sample $t$-Test ($H_0: \mu = \$4.00B$):** $t = 3.26$, $p = 0.0011 < 0.05$. Reject $H_0$. True average billionaire wealth is significantly greater than \$4.00B.
  - **Two-Sample $t$-Test (Self-Made vs Inherited):** Self-made mean = \$4.58B vs Inherited mean = \$4.73B; $t = -0.347$, $p = 0.728 > 0.05$. Fail to reject $H_0$. No statistically significant difference in mean wealth between self-made and inherited billionaires.

### Practical 6: Advanced Hypothesis Testing (Paired $t$-Test, ANOVA, Chi-Square)
- **Aim:** Perform multi-group and categorical inferential testing.
- **Key Findings:**
  - **One-Way ANOVA (Top 5 Industries):** $F = 5.21$, $p = 0.0003 < 0.05$. Reject $H_0$. Mean wealth differs significantly across industries (Fashion & Retail and Technology lead).
  - **Chi-Square Test of Independence (Industry vs Self-Made):** $\chi^2 = 72.84$, $df = 4$, $p = 5.67 \times 10^{-15} \ll 0.05$. Decisively reject independence. In Technology, 88.5% are self-made, compared to 59.8% in Fashion & Retail.
  - **Paired $t$-Test (Top 50 Cohort Matching):** Statistically significant difference ($p < 0.01$) driven by centibillionaire tech founders.

### Practical 7: Covariance and Correlation Analysis (Pearson & Spearman)
- **Aim:** Quantify linear vs monotonic associations and analyze the influence of outliers.
- **Key Findings:**
  - **Age vs Net Worth:** $Cov = 4.95$, Pearson $r = +0.038$ ($p = 0.052$, weak), Spearman $\rho = +0.027$ ($p = 0.165$). Wealth is not a simple linear function of aging.
  - **Rank vs Net Worth:** Pearson $r = -0.551$ (moderate linear correlation), but Spearman $\rho = -0.999$ (near-perfect monotonic negative rank correlation).
- **Viva Point:** Pearson $r$ is attenuated because wealth decays non-linearly (power law), while Spearman $\rho$ operates on rank order and captures the relationship perfectly.

### Practical 8: Regression Analysis and Predictive Modeling
- **Aim:** Train and evaluate regression models on an 80/20 train/test split.
- **Regression Equation:** $\text{Net Worth (Billion USD)} = 2.457 + (0.0336 \times \text{Age})$
- **Model Comparison:**
  - *Linear Regression:* $MAE = \$3.28B$, $RMSE = \$11.66B$, $R^2 = 0.002$
  - *Ridge Regression ($\alpha=1.0$):* $MAE = \$3.28B$, $RMSE = \$11.66B$, $R^2 = 0.002$
  - *Decision Tree Regressor:* $MAE = \$3.22B$, $RMSE = \$11.64B$, $R^2 = 0.006$
- **Viva Point:** The near-zero $R^2$ is an important real-world finding: demographic variables like age alone cannot explain hyper-concentrated wealth. Residual plots reveal pronounced heteroscedasticity due to extreme centibillionaire outliers.

---

## 4. Software Dependencies & Environment Setup

This project uses standard, open-source Python data science packages:

```bash
pip install numpy pandas matplotlib seaborn scipy scikit-learn beautifulsoup4 requests nbformat nbclient
```

### Verified Compatible Versions
- Python 3.10 / 3.11 / 3.12 / 3.14
- `numpy >= 1.24.0`
- `pandas >= 2.0.0`
- `matplotlib >= 3.7.0`
- `seaborn >= 0.12.0`
- `scipy >= 1.10.0`
- `scikit-learn >= 1.2.0`
- `beautifulsoup4 >= 4.12.0`

---

## 5. How to Run the Project & Streamlit Frontend

### Option A: Launch Interactive Streamlit Frontend (Recommended for Live Demo)
Run the following single command in your terminal:
```bash
streamlit run app.py
```
This automatically opens your default browser at `http://localhost:8501`, loading an interactive dashboard complete with live CLT simulations, interactive ANOVA/Chi-Square tables, regression predictors, and a full viva cheat sheet!

### Option B: Local Jupyter Notebook
```bash
jupyter notebook global_billionaires_statistical_inference.ipynb
```
In the Jupyter menu, click **Kernel** $\rightarrow$ **Restart & Run All**.

### Option C: Google Colab
1. Open [Google Colab](https://colab.research.google.com/).
2. Select **Upload** and upload `global_billionaires_statistical_inference.ipynb`.
3. Optionally upload `global_billionaires.csv` (or let the automatic GitHub fallback load it).
4. Run all cells via **Runtime** $\rightarrow$ **Run all** (`Ctrl + F9`).

---

## 6. Viva Voce Quick-Reference Guide

| Viva Question | Concise Answer |
| :--- | :--- |
| **Why is the mean net worth (\$4.62B) much higher than the median (\$2.30B)?** | Because billionaire wealth is heavily right-skewed. A few centibillionaires (e.g., Elon Musk at \$180B, Bernard Arnault at \$211B) inflate the arithmetic mean, while the median reflects the 50th percentile. |
| **What does the Central Limit Theorem prove here?** | Despite extreme skewness in individual wealth, the distribution of *sample means* becomes symmetric and Gaussian as sample size $n \ge 30$, with standard error scaling as $\sigma/\sqrt{n}$. |
| **Does being self-made significantly increase average net worth?** | No. The independent two-sample $t$-test yielded $p = 0.728 > 0.05$, indicating no statistically significant difference in mean wealth between self-made and inherited billionaires. |
| **Why does Rank vs. Net Worth have $r = -0.551$ but $\rho = -0.999$?** | Net worth follows a steep power curve as rank increases. Pearson $r$ measures linear association and is weakened by curvature, whereas Spearman $\rho$ measures monotonic rank order, which is monotonic. |
| **Why is the regression $R^2$ so low ($\approx 0.002$)?** | Age accounts for less than 0.5% of variance in billionaire wealth. Extreme wealth is created through equity ownership, innovative scale, and market capitalization, not seniority. |

---

## 7. Deliverables Checklist

- [x] `global_billionaires_statistical_inference.ipynb` — Complete notebook with all 8 practicals, code, markdown, and embedded outputs.
- [x] `global_billionaires.csv` — Full 2,640-row cleaned dataset with 9 verified attributes.
- [x] `README.md` — Project documentation, theoretical formulas, mapping to lab manual, and viva guide.
- [x] Notebook runs from start to finish with 0 errors.
