# Financial Transaction & Risk Analytics Dashboard

An end-to-end Power BI dashboard analyzing customer financial transactions to surface transaction trends, customer behaviour, and fraud/risk patterns.

**Executive Overview**

**Customer & Transaction Behaviour**

**Risk, Merchant & Geographic Analysis**


 🧰 Tools & Techniques

 Tool | What it was used for 

 **Power Query** | Cleaning the raw data before it reached the report — removed duplicate transaction records and fixed inconsistent text (typos, stray whitespace) in category fields 
 **Data Modeling** | Built a relationship between the customer table and the transaction table (via `customer_id`) so transactions could be sliced by customer segment, state, and occupation 
 **DAX** | Custom measures for every KPI — transaction totals, success rate, fraud rate, average risk score, YoY growth, and the threshold used to flag "high risk" transactions 

  🗂️ Dataset

- **Transactions:** ~50,000 records (Jan 2023 – Apr 2026) — transaction type, channel, merchant category, amount, fees, tax, status, fraud flag, risk score
- **Customers:** 5,000 records — demographics, occupation, customer segment, annual income, join date
- Two tables joined on `customer_id`

  🔑 Key Metrics

Metric Value 

 Total transactions 50,000 
 Total transaction amount ₹45.55 Cr 
 Average transaction value  ₹9.11K 
 Success rate  85.74% 
 Fraud rate  1.26% 
 Fraudulent transactions  630 (₹59.69L) 
 High-risk transactions (risk score ≥ 70) | 1,212 
 Average risk score  36.08 

📄 Dashboard Pages

1. **Executive Overview** — headline KPIs, monthly transaction trend, transactions by customer segment, transaction amount by merchant category and by channel
2. **Customer & Transaction Behaviour** — transaction amount by customer segment and occupation, channel-level performance table (success/failure/fraud), transaction type breakdown
3. **Risk, Merchant & Geographic Analysis** — fraud rate by channel, state-level breakdown by customer segment, fraud amount by transaction type, high-risk amount by merchant category, failure rate by state

💡 Key Insights

- **ATM transactions carry the highest fraud rate (1.41%)** among all channels, while Auto Debit has the lowest (1.09%) — a case for tighter ATM-side fraud controls.
- **Loan EMI and Transfer transactions account for the largest share of fraud amount**, ahead of card payments and withdrawals.
- **Delhi has the highest transaction failure rate (11.36%)** among the states in the dataset, notably higher than the rest.
- **Retail customers drive the majority of transaction volume**, but Premium and Wealth segments contribute disproportionately to high-value transactions.

🧹 Data Cleaning Notes

- Identified and removed **69 duplicate `transaction_id` records** (138 rows total were involved) before any aggregation — without this, transaction and fraud counts would have been overstated.
- Standardized the `channel` field, which contained a typo (`M@bile App`) and several entries with stray leading/trailing whitespace — left uncleaned, these were silently fragmenting totals across near-duplicate categories.
- Defined "high risk" as `risk_score ≥ 70`, applied consistently as a DAX measure rather than a hardcoded filter, so it stays correct if the underlying data changes.

 📌 What I Learned

While building the Executive Overview page, the "Transaction Amount by Month" chart initially showed what looked like a steady decline from January to December. On closer inspection, this turned out to be a data artifact, not a real trend: the dataset covers January 2023 through April 2026, so early months (Jan–Apr) were being summed across 4 years while later months (May–Dec) only had 3 years of data behind them. Restricting the chart to complete years (2023–2025) removed the artificial decline and revealed a flat, seasonally stable pattern.

Good reminder that a chart can be numerically correct and still tell a misleading story — worth checking what's actually being aggregated before trusting a trend line.
