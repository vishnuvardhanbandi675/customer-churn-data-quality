# Customer Churn: Data Quality and Exploratory Analysis

| File | What it is |
|---|---|
| `churn_quality_analysis.ipynb` | Full notebook: profiling, cleaning, validation checks, statistics, charts (already executed) |
| `data_quality_report.md` | One-page data-quality summary and findings |
| `data/customer_churn_clean.csv` | Cleaned, analysis-ready dataset (adds `ChurnFlag`, `TenureBand`) |
| `data/data_dictionary.csv` | Column definitions (portfolio upgrade) |
| `data/customer_churn_sample.csv` | Original raw file, unchanged |
| `charts/` | Distribution, segment, and churn-vs-numeric charts |

**Headline:** the data was already clean (no missing values, duplicates, or outliers; all 11 validation checks pass). 46.7% of customers churned, concentrated in Month-to-Month contracts, short tenure, and high support-ticket counts. With 15 rows these are hypotheses, not conclusions.

**Reproduce:** `pip install pandas numpy matplotlib jupyter`, then run the notebook top to bottom.
