# ab_test_landing_page-
This project presents an analysis of an A/B experiment with a landing page, with a particular focus on experimental validity, statistical accuracy, and business decision-making.  

The goal is to properly assess whether the new landing page provides a **statistically and practically significant improvement**
in conversion rate. Analysis included data auditing, temporal stability checks, robustness to dilution and heterogeneity, Bayesian posterior estimation  and alignment between statistical evidence and business decisions.

---

## Dataset

**Source:** Public landing page A/B test dataset  

| column | description |
| ------------ | --------------------- |
| user_id | unique user |
| timestamp | event time |
| group | control / treatment |
| landing_page | old_page / new_page |
| converted | 0 / 1 |

Where the 'control' group corresponds to users viewing the "old_page", while the 'treatment' group corresponds to those viewing the "new_page".

Synthetic data imputation is applied in a controlled manner to provide:
- dilution analysis (ITT vs. PP),
- testing for segment-level heterogeneity.

| column           | Type        |Value       | Purpose         |
| ----------------- | ----------- |------------------- | --------------- |
| device_type       | categorical|'mobile'/'desktop'| segment effects |
| geo_region        | categorical |'US'/ 'EU'/ 'Other'| heterogeneity   |
| is_returning_user | boolean     |True/False | behavior        |
| exposure_count    | integer     |0/1/2/3 | ITT vs PP       |

---

## Repository structure
ab-test-landing-page/  
  │  
  ├─ README.md  
  │  
  ├── notebooks/  
  │ ├── 00_data_audit_and_data_enrichment.ipynb  
  │ └── 01_hypothesis_testing.ipynb  
  │  
  ├── data/  
  │ ├── raw/ab_data.csv  
  │ └── processed/ab_enriched.csv  
  └── LICENSE


---

## Methodology overview

The analysis follows a structured, decision-oriented workflow:

### 1. Data audit & validation
- Logical consistency and duplicate checks
- Sample ratio mismatch (SRM)
- Daily traffic split stability

### 2. Data enrichment
- Controlled synthetic features to support:
  - exposure-based filtering (PP),
  - segment-level analysis

### 3. Primary inference (ITT)
- Temporal sanity checks
- Difference in conversion rates and Newcombe confidence interval
- Hypothesis testing (difference in proportions)
- Decision rule based on **Minimum Practical Effect (MPE)=0,3 pp**

### 4. ITT vs PP (dilution analysis)
- Comparison of ITT and PP estimates 
- Interpretation of dilution or exposure mismatch

### 5. Heterogeneous treatment effects (HTE)
- Interaction-first logistic regression
- Likelihood ratio tests
- Multiple-testing correction (Holm)
- Interpretation of segment-level heterogeneity

### 6. Bayesian decision add-on
- Beta-Binomial posterior inference
- Probability of positive and practically meaningful effects

### 7. Business evaluation
- Expected value (EV) calculation (Assuming 1 million users visits per month and conversion value = €5)
- Statistical evidence translated into actionable decision-making

---

## Key results

- No statistically or practically significant uplift was detected.
- Confidence intervals include zero and remain below the MPE threshold.
- ITT and PP estimates are nearly identical, indicating negligible dilution.
- There is no evidence of segment-level heterogeneity or Simpson’s paradox.
- Bayesian posterior algorithm assigns a low probability to a positive effect and
  near-zero probability to exceeding the MPE.
- The expected value of a global rollout is negative under reasonable assumptions.

---

## Recommendation

**Do not ship the new landing page globally.**

Suggested next steps:
- targeted rollout
- design iteration
- or longer experiment with higher exposure.

---

## Limitations

- The experiment may be underpowered to detect very small effects (below the defined MPE).
- Heterogeneity tests may miss small segment-level effects.
- Results apply to the observed traffic mix and time window.

---

## Tools & stack

- Python
	- pandas, numpy
	- scipy, statsmodels
	- matplotlib, seaborn
- Jupyter Notebook

---

## License

This project is released under the MIT License.
