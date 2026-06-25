# 💍 Marriage Factors on Cheating — An Econometric Analysis

> **Course:** Econometrics
> **Team:** Kai Yang, Bilwa Khaparde, Ishaan Singh
> **Language:** R

---

## 📌 Project Overview

What actually predicts whether someone will have an extramarital affair? Is it how long they've been married? Whether they have kids? How happy they are in their relationship? This project uses **econometric regression techniques** to quantify the relationship between marriage-related factors and infidelity using a real survey dataset.

By applying **Ordinary Least Squares (OLS) regression** and a **Probit model**, this analysis moves beyond guesswork to provide statistically grounded insights into the predictors of extramarital affairs — with clear implications for psychology, counseling, and social science research.

**Research Question:** What is the effect of marital happiness, years married, and having children on the likelihood of engaging in an extramarital affair?

**Null Hypothesis (H₀):** Years married has no effect on whether a person will have an affair (β(yrsmarr) = 0)

**Alternative Hypothesis (Hₐ):** Years married has a statistically significant effect (β(yrsmarr) ≠ 0)

---

## 🗂️ Repository Structure

```
marriage-cheating-econometrics/
│
├── code/
│   ├── EconFinalProject.Rmd    # Full R Markdown analysis (source)
│   └── EconFinalProject.html   # Rendered HTML report with all outputs
│
├── slides/
│   └── Econometrics__Final_Presentation.pdf
│
└── README.md
```

---

## 📊 Dataset

### Source
R.C. Fair (1978) — *"A Theory of Extramarital Affairs"*, Journal of Political Economy. Accessed via the `wooldridge` package in R (`data("affairs")`).

### Scale
- **601 observations** (survey respondents)
- **19 variables**
- No missing values

### Key Variables

| Variable | Type | Description |
|---|---|---|
| `naffairs` | Continuous | Number of extramarital affairs in the past year (dependent variable for OLS) |
| `affair` | Binary (0/1) | Whether respondent had at least one affair (dependent variable for Probit) |
| `ratemarr` | Ordinal (1–5) | Self-reported marital happiness (1 = very unhappy, 5 = very happy) |
| `yrsmarr` | Continuous | Number of years married |
| `kids` | Binary (0/1) | Whether the respondent has children |
| `age` | Continuous | Age of the respondent in years |
| `educ` | Continuous | Years of formal education |
| `relig` | Ordinal (1–5) | Level of religiosity (1 = anti-religious, 5 = very religious) |
| `male` | Binary (0/1) | Gender (1 = male, 0 = female) |
| `occup` | Ordinal | Occupational status (reverse Hollingshead scale) |

### Descriptive Statistics (Key Variables)

| Variable | Mean | Median | Std. Dev. |
|---|---|---|---|
| Rate of Marriage | 3.93 | 4.00 | 1.10 |
| Years Married | 8.18 | 7.00 | 5.57 |
| Kids (binary) | 0.72 | 1.00 | 0.45 |
| Number of Affairs | 1.46 | 0.00 | 3.30 |

> **Note:** 25% of respondents reported at least one affair. The median for `naffairs` is 0, indicating most individuals did not cheat, but a small group had significantly more, skewing the mean upward.

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| **R** | Core programming language |
| **R Markdown** | Reproducible analysis and report generation |
| **ggplot2** | Data visualizations (histograms, bar charts, pie charts) |
| **stargazer** | Formatted regression output tables |
| **AER / wooldridge** | Dataset access and econometric tools |
| **sandwich / lmtest** | Heteroskedasticity-robust standard errors (HC1) |
| **erer** | Marginal effects calculation for Probit models |
| **tidyverse / dplyr** | Data manipulation and wrangling |

---

## 📐 Methodology

### Step 1: Exploratory Data Analysis
Three visualizations were produced to understand the data before modeling:
- **Histogram of Marriage Ratings** — Most respondents rate their marriage 4 or 5, showing an overall positive bias in self-reported happiness
- **Pie Chart: Cheated vs. Not Cheated by Gender** — 25% of respondents reported an affair; men cheated at a marginally higher rate (12.98%) than women (11.98%)
- **Bar Chart: Marital Happiness by Gender and Kids** — Respondents with children consistently report higher happiness across both genders

### Step 2: OLS Multiple Regression (Dependent Variable: `naffairs`)

A series of 10 regression models were built progressively, starting with just `yrsmarr` and adding variables one by one to assess the incremental explanatory power of each predictor.

**Variables of Interest:** `yrsmarr`, `ratemarr`, `kids`

**Control Variables:** `male`, `age`, `educ`, `relig`

**Non-linear terms tested:** `yrsmarr²`, `educ²`, `yrsmarr³`, `educ³`

All models used **heteroskedasticity-robust standard errors (HC1)** to account for non-constant variance in the error terms.

**Model Selection via F-Tests:**
- A linear hypothesis test comparing the quadratic model to the linear model returned a p-value of 0.31 — **not significant**, meaning the quadratic terms did not improve fit
- A test for cubic terms returned p-value of 0.33 — also **not significant**
- **Final model selected: Linear specification** (Model 7), with all key predictors and no polynomial terms

### Step 3: Probit Model (Dependent Variable: `affair`, binary)

Because the outcome of interest is binary (cheated or not), a **Probit model** was also estimated, which predicts the *probability* of an affair rather than the count. **Marginal effects** were calculated to convert Probit coefficients into interpretable percentage-point changes in probability.

Three Probit models were run:
- Model 1: `yrsmarr + kids + ratemarr`
- Model 2: Adds `male + age + educ + relig`
- Model 3: Adds interaction term `male × kids`

---

## 📈 Key Results & Insights

### OLS Regression (Predicting Number of Affairs)

| Variable | Direction | Significance |
|---|---|---|
| Years Married (`yrsmarr`) | ➕ Positive | Statistically significant |
| Marital Happiness (`ratemarr`) | ➖ Negative (strong) | Statistically significant |
| Kids | ➖ Negative (slight) | Not statistically significant |
| Religiosity (`relig`) | ➖ Negative (strong) | Statistically significant |
| Age | ➖ Negative (slight) | Statistically significant |
| Gender (Male) | ➕ Positive (slight) | Not statistically significant |
| Education | Near zero | Not statistically significant |

### Probit Model — Marginal Effects (Change in Probability of an Affair)

| Factor | Effect on Probability of Affair |
|---|---|
| +1 year of marriage | **+0.4% to +1.7%** increase |
| Having children | **+6.1% to +6.7%** increase |
| +1 unit of marital happiness | **−8.2% to −8.4%** decrease *(strongest predictor)* |
| +1 unit of religiosity | **−5.6%** decrease |
| Being male (vs. female) | **+5.7% to +10.2%** increase |
| +1 year of age | **−0.7%** decrease |
| Education | No meaningful effect |

### Headline Insights

**Marital happiness is the single strongest predictor of fidelity.** Each one-point increase in marital satisfaction (on a 1–5 scale) reduces the probability of an affair by roughly 8%. This effect is consistent and statistically significant across all model specifications.

**Having children slightly increases affair likelihood**, contrary to the intuition that kids keep couples together. This may reflect increased relationship stress, reduced intimacy, or shifting priorities — though the effect is not statistically robust given large standard errors.

**Religiosity significantly deters infidelity**, reducing the probability of an affair by about 5.6% per unit increase. This supports the idea that moral and social norms tied to religion act as a meaningful behavioral constraint.

**Longer marriages correlate with slightly higher affair rates**, but the effect is small (~1.6% per additional year) and the relationship is linear — polynomial (quadratic/cubic) terms did not improve model fit.

**Gender and education have weak, statistically insignificant effects** on infidelity after controlling for other factors.

---

## ⚠️ Limitations

### Internal Validity
- **Omitted variable bias** — Personality traits, relationship history, and external stressors are absent from the model, preventing causal claims. The low adjusted R² (~13%) confirms these variables explain only a small share of variation in cheating behavior
- **Self-reporting bias** — Since infidelity carries social stigma, respondents may have underreported affairs, creating measurement error in the dependent variable
- **Sample selection bias** — People who cheat may be less likely to participate in relationship studies, meaning the true proportion of cheaters may be understated

### External Validity
- **Data is from 1978** — Social norms, gender roles, and attitudes toward infidelity have shifted significantly in the past 45+ years, limiting how well findings generalize to modern relationships
- **U.S.-only sample** — Cultural norms around marriage and infidelity vary widely across countries; results should not be generalized internationally

---

## ▶️ How to Run

1. Install R and RStudio
2. Install required packages:
   ```r
   install.packages(c("tidyverse", "ggplot2", "dplyr", "stargazer",
                      "AER", "wooldridge", "lmtest", "sandwich", "erer"))
   ```
3. Open `code/EconFinalProject.Rmd` in RStudio
4. Click **Knit → Knit to HTML** to reproduce the full analysis report

The dataset loads automatically from the `wooldridge` package — no external data files needed:
```r
library(wooldridge)
data("affairs")
```

Alternatively, open `code/EconFinalProject.html` directly in any browser to view the pre-rendered results without running any code.
