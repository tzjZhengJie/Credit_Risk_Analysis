# Credit_Risk_Analysis

A data-driven credit risk analysis identifying the top 3 actionable insights from a portfolio of 28,495 borrower loans, using Python and pure SQL via SQLite.

---

## Files
- `credit_risk_SQL_analysis_coinhako_v2.ipynb` — main analysis notebook
- `credit_risk_dataset.csv` — source data ([Kaggle](https://www.kaggle.com/datasets/laotse/credit-risk-dataset))

---

## Approach

All core analysis is written in SQL (via Python `sqlite3`). Data is cleaned before any query runs. Insights are kept to three to respect the leadership briefing format — directional signals for stakeholders to investigate, not prescriptive policy.

---

## Data Cleaning

Raw dataset: 32,581 rows. After cleaning: **28,495 rows**.

| Step | Reason |
|---|---|
| 165 duplicate rows removed | Inflate counts and skew averages |
| `person_age >= 100` filtered | No valid borrower is 144 years old |
| `person_emp_length >= 60` filtered | Cannot have 123 years of employment |
| `loan_int_rate` nulls dropped | Missing rates distort all pricing analysis |

---

## Key Findings

### Insight 1 — Loan Grade
Default rates span **9.61% (Grade A) → 98.31% (Grade G)**, a ~10× spread. Grade D alone represents 11.4% of the portfolio at a 59.22% default rate — the largest high-risk segment by volume.

### Insight 2 — DTI Cliff at 30%
Default rate is stable at ~15% for DTI ≤30%, then jumps to **71% above 30%** — a 4.66× lift. Critically, this cliff holds across every grade: a Grade A borrower above 30% DTI defaults at 60.9%, worse than a Grade D borrower below it (55.2%).

### Insight 3 — Prior Default History
Y-flag borrowers default at **37.83% vs 18.21%** for clean-file — a 2.08× lift. The bank charges a 4.22% rate premium to Y-flag borrowers, but the default rate gap suggests this is insufficient to cover expected losses.

---

## Priority

**If only one insight can be acted on: Insight 2 — DTI Cliff.**

DTI is a single hard number at origination, overrides grade, covers 11.6% of the portfolio with one rule, and requires no new model or system change. Grade is most useful as the triage tool within DTI — it tells you how aggressively to price and monitor borrowers near the 30% boundary.

---

## Compound Risk

Combining grade and DTI reveals the danger zone: **Grade D+ borrowers above 30% DTI** — 715 loans (2.5% of portfolio) — carry an **84% default rate** and **$10.5M expected loss**. The most urgent intervention target.

---

## Next Question

Does loan intent interact with grade and DTI to create distinct risk micro-segments? Debt consolidation loans default at 28.46% — the highest of any intent. A three-way `grade × DTI × intent` cut is the natural next SQL analysis.

---

## Stack
- Python 3
- pandas, sqlite3
