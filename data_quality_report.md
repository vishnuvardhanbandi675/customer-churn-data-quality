# Data Quality Summary: Customer Churn Sample

**Source:** `data/customer_churn_sample.csv` (synthetic). **Rows:** 15. **Columns:** 11 raw, 13 after cleaning.
**Full working:** `churn_quality_analysis.ipynb` (executed, with outputs and charts).

## 1. Audit results

| Check | Result | Action |
|---|---|---|
| Missing values | 0 in all 11 columns | None needed |
| Full-row duplicates | 0 | None needed |
| Duplicate CustomerID | 0 | None needed |
| Category spelling / case / whitespace | Consistent in all 5 categorical columns | Trimmed whitespace defensively (0 values changed) |
| Outliers (IQR rule and z > 3) | 0 flagged in Age, Tenure, MonthlyCharges, TotalCharges, SupportTickets | Kept all rows |
| Impossible values | None (ages 23-60, tickets 0-6, no negatives) | None needed |
| TotalCharges = Tenure x MonthlyCharges | Holds in all 15 rows (max difference 0.00) | Kept; noted as redundant |
| One price per plan | Basic 49.99, Pro 79.99, Enterprise 149.99 | None needed |

**Conclusion:** the file is already clean. Cleaning consisted of type standardisation and two derived fields, with **no rows removed, imputed, or altered**.

## 2. Cleaning decisions and assumptions

1. Categorical columns cast to `category` dtype after trimming text.
2. `ChurnFlag` added (Yes = 1, No = 0) as a numeric target; original `Churn` kept.
3. `TenureBand` added (0-6, 7-12, 13-24, 25+ months). Band edges are my own choice.
4. The highest values (Enterprise plan, 48-month tenure, 7,199.52 total) are legitimate, not errors: they sit inside the IQR fences and match the pricing rule.
5. No imputation or deletion, because nothing was missing, duplicated, or invalid.

## 3. Validation (11 assertions, all passed)

Row count unchanged (15 to 15); CustomerID unique; no missing values; age 18-100; tenure >= 0; charges > 0; tickets >= 0; TotalCharges reconciles; Churn only Yes/No; ChurnFlag matches Churn; every row has a TenureBand.

## 4. Summary statistics

Overall churn: **46.7%** (7 of 15).

| Field | Mean | Median | Min | Max |
|---|---|---|---|---|
| Age | 39.1 | 38 | 23 | 60 |
| TenureMonths | 18.8 | 15 | 2 | 48 |
| MonthlyCharges | 84.66 | 79.99 | 49.99 | 149.99 |
| TotalCharges | 2,088.48 | 1,099.78 | 99.98 | 7,199.52 |
| SupportTickets | 2.5 | 2 | 0 | 6 |

## 5. Initial patterns

| Segment | Churn rate | Customers |
|---|---|---|
| Month-to-Month contract | 100% | 7 |
| One Year / Two Year contract | 0% | 8 |
| Basic / Pro / Enterprise plan | 71% / 50% / 0% | 7 / 4 / 4 |
| Tenure 0-12 months / 13-24 / 25+ | 100% / 20% / 0% | 6 / 5 / 4 |
| Debit Card / UPI / Credit Card / Bank Transfer | 100% / 75% / 33% / 0% | 2 / 4 / 6 / 3 |

Churners vs stayers: tenure 7.0 vs 29.1 months, support tickets 4.3 vs 1.0, age 30.4 vs 46.6, monthly charge 58.6 vs 107.5.

## 6. Limitations

- **n = 15**: results are descriptive only; extreme 0% / 100% splits partly reflect tiny groups.
- **Overlapping drivers**: contract, plan, tenure, and price move together (all Enterprise customers are on Two-Year contracts), so effects cannot be separated.
- **Synthetic data**: patterns may be designed rather than real.
- **Currency** is not stated in the source.
