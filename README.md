# Statistical Experimentation & Causal Inference Engine

An end-to-end Data Science experimentation and causal inference pipeline built in Python using strictly **NumPy**, **Pandas**, **Matplotlib**, **Seaborn**, and Python's built-in **`math` / `statistics`** modules—implementing every statistical test, confidence interval, guardrail, and variance-reduction estimator from mathematical first principles without black-box wrappers.

![Statistical Experimentation & Causal Inference Dashboard](Screenshot%202026-11-12%20184048.png)

---

## Business Context & Experimental Design
Evaluating product changes (such as a redesigned SaaS onboarding flow) requires more than running a basic T-test. Real-world user telemetry suffers from right-skewed monetization, noisy engagement metrics, and potential traffic-splitting bugs.

This engine simulates **$N = 12,000$ randomized users** split across **Control** and **Variant B**, stratified by **Device** (`Desktop`, `Mobile`, `Tablet`) and **Plan Tier** (`Starter`, `Pro`, `Enterprise`), alongside a 14-day pre-experiment engagement covariate (`pre_exp_engagement`).

---

## Statistical & Causal Methodology (Implemented From Scratch)

| Stage | Method / Estimator | Mathematical Formulation | Purpose |
| :--- | :--- | :--- | :--- |
| **1. Integrity Guardrail** | **Sample Ratio Mismatch (SRM)** | $\chi^2 = \sum \frac{(O_i - E_i)^2}{E_i}$ | Validates unbiased 50/50 randomization before evaluating lift |
| **2. Conversion Lift** | **Two-Proportion Z-Test & Wilson CI** | $Z = \frac{\hat{p}_B - \hat{p}_A}{\sqrt{\hat{p}(1-\hat{p})(\frac{1}{n_A}+\frac{1}{n_B})}}$ | Tests statistical significance & boundary-safe 95% confidence bounds |
| **3. Causal Precision** | **CUPED Variance Reduction** | $Y_{\text{cuped}} = Y - \frac{\text{Cov}(Y, X)}{\text{Var}(X)}(X - \bar{X})$ | Strips pre-experiment user noise to tighten treatment standard error |
| **4. Skewed Revenue** | **Tie-Corrected Mann-Whitney U** | $U = R_1 - \frac{n_1(n_1+1)}{2}$ | Non-parametric rank test for right-skewed Log-Normal payer revenue |
| **5. Empirical CI** | **Monte Carlo Bootstrap** | $10,000$ resamples with replacement | Derives empirical 2.5th & 97.5th percentile confidence bounds |
| **6. Segment Validity** | **Chi-Square Independence** | $\chi^2$ across Device strata ($\text{df}=2$) | Rules out Simpson's Paradox across Desktop, Mobile, and Tablet |

---

## Key Experimental Results

* **SRM Integrity Check:** Control ($6,069$) vs. Variant B ($5,931$) yielded $\chi^2 = 1.5870$ ($p = 0.2078$) — **PASS**[cite: 1].
* **Conversion Rate Lift:** Increased from **12.39%** to **14.84%** (**+2.45 percentage points**), validated via Two-Proportion Z-Test ($Z = 3.9088, p = 0.000093$, Cohen's $h = 0.0714$) and 10,000-draw Bootstrap 95% CI **[1.20 pp, 3.66 pp]**[cite: 1].
* **CUPED Variance Reduction:** Reduced session duration variance by **94.5%** (Standard Error dropped from **2.749s** to **0.644s**), isolating a **+17.64s** causal engagement lift ($Z = 27.41$)[cite: 1, 2].
* **Payer Revenue Lift:** Median revenue among converted users rose from **$61.59** to **$67.31** ($U = 301,536, Z = -3.0922, p = 0.001987$)[cite: 1].
* **Device Stratification:** Consistent lift confirmed across **Desktop** (`13.4%` vs `16.4%`), **Mobile** (`11.5%` vs `12.2%`), and **Tablet** (`10.2%` vs `15.7%`) ($\chi^2 = 21.13, p < 0.001$)[cite: 1, 2].

---

## Repository Structure
* `Untitled4.ipynb` — Complete Google Colab / Jupyter notebook
* `Screenshot 2026-11-12 184048.png` — 4-panel dark executive diagnostic dashboard[cite: 4]
