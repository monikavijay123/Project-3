# RFM Customer Segmentation Analysis

AT-SQL (SQL Server / SSMS) project that segments 10,000+ customers into actionable marketing groups using **Recency, Frequency, and Monetary (RFM)** analysis — surfacing top revenue drivers and flagging churn risk for targeted retention campaigns.

## 📌 Overview

This project builds a multi-layer RFM scoring model to classify customers based on their purchasing behavior:

- **Recency (R)** — How recently did the customer purchase?
- **Frequency (F)** — How often do they purchase?
- **Monetary (M)** — How much do they spend?

Each dimension is scored 1–5, combined into an RFM profile, and mapped to one of seven business-friendly segments — each with a tailored recommended action.

## 🧩 Segments

| Segment | Description | Recommended Action |
|---|---|---|
| **Champions** | Bought recently, buy often, spend the most | Reward, upsell, request referrals |
| **Loyal** | Consistent, frequent buyers | Upsell higher-value products, loyalty perks |
| **Potential Loyalists** | Recent customers with average frequency | Offer incentives to increase purchase frequency |
| **At Risk** | High past value, haven't purchased recently | Personalized win-back campaigns |
| **Need Attention** | Above-average recency/frequency but slipping | Limited-time offers, re-engagement emails |
| **Hibernating** | Low recency, frequency, and monetary value | Reactivation campaigns, surveys |
| **Lost** | Long inactive, minimal historical value | Low-cost reactivation or deprioritize |

## 🛠️ Technical Approach

- **3 chained CTEs** to progressively build the RFM model:
  1. Aggregate raw transaction data per customer
  2. Score each customer 1–5 on R, F, and M
  3. Concatenate scores and map to final segment via `CASE` logic
- **`DATEDIFF`** to calculate recency in days since last purchase
- **`COUNT(DISTINCT ...)`** to calculate frequency without inflating counts from duplicate order lines
- **`COALESCE`** to gracefully handle customers with no purchase history (NULLs from the join)
- **`LEFT JOIN`** to retain the full customer base, not just those with transactions
- **`CASE` statements** throughout for scoring buckets and segment assignment
- **`#temp` table pattern** — final RFM output is materialized into a temp table so downstream queries (segment counts, revenue concentration, churn flags) can run repeatedly in the same SSMS session without recomputing the full CTE chain

## ▶️ How to Run

1. Open the scripts in **SQL Server Management Studio (SSMS)**.
2. Run `01`–`03` in order within a single session to build and populate `#RFM_Segments`.
3. Run any query in `04_insights_queries.sql` against `#RFM_Segments` as needed — no need to re-run the CTE chain.
4. Temp table is session-scoped; re-run steps 1–3 if you start a new SSMS session.

## 🔧 Requirements

- SQL Server 2016+ / SSMS
- A transactions table with, at minimum: `CustomerID`, `OrderID`, `OrderDate`, `OrderAmount`
- A customers table 





