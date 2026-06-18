# Data Quality Report

## Scope
This report summarizes the main data-quality findings from the Part 1 raw-data audit for the D2C churn capstone. The audit covers customer, order, support-ticket, web activity, churn-label, and intervention-history tables, with emphasis on missingness, duplicates, key consistency, date logic, and leakage risk [file:1][file:340].

## Files reviewed
The audit covers the following core raw files:
- `customers.csv`
- `orders.csv`
- `support_tickets.csv`
- `web_events_snapshot.csv`
- `churn_labels.csv`
- `intervention_history.csv` [file:1][file:340]

## Missing values
The most visible missing-data issue is in `customers.csv`, where `loyalty_tier` is missing for a large share of customers and `skin_type` is also partially missing in the draft analysis [file:340]. In `orders.csv`, `rating` is partially missing, which is reasonable because not every order may have a review, but this should still be treated carefully during analysis because average-rating metrics may otherwise overrepresent highly engaged customers [file:340].

Recommended handling:
- Treat `loyalty_tier` missingness as a meaningful category such as `Unknown` or `Not Enrolled`, rather than dropping rows.
- Treat `skin_type` as an optional profile field and preserve a missing category for descriptive analysis.
- Use caution when aggregating `rating`; report both review count and average rating when possible.

## Duplicate and key checks
The draft audit reports no full-row duplicates in the major datasets and shows 2,400 unique customers in the customer, churn-label, web snapshot, intervention-history, and modeling-snapshot tables, indicating clean one-row-per-customer snapshot tables [file:340]. This is important because it means customer-level left joins on `customer_id` should be stable for Part 1 descriptive analysis [file:340].

Recommended handling:
- Keep one-to-many relationships only in transactional tables like orders and support tickets.
- Validate row counts after each join to ensure no accidental multiplication of snapshot records.

## Date consistency
The churn-label and web/intervention tables are snapshot-based, while orders and support tickets are event-based over time [file:1][file:340]. A critical issue is that `orders.csv` extends beyond the snapshot date used for churn labeling, which means naive order aggregation can leak future information into descriptive conclusions or downstream model features [file:1][file:340].

Recommended handling:
- Restrict order-based features to data on or before the snapshot date.
- Document any time windows explicitly in the notebook.
- Apply the same rule to any future feature engineering in Parts 2–4 [file:1].

## Join and schema issues
The dataset structure is broadly suitable for customer-level analysis because most snapshot tables appear keyed at one row per customer, while order and support tables can be aggregated before merge [file:340]. The main practical join risk is not missing keys but accidental duplication when merging raw orders or raw tickets directly onto customer-level data without aggregation first [file:340].

Recommended handling:
- Aggregate orders and tickets by `customer_id` before joining to the master analysis table.
- Keep a documented customer-universe base table for EDA.

## Unusual values and outliers
The draft EDA script explores gross amount, discount percentage, returned flag, support ticket counts, sessions, and last-visit measures, which are all variables that can show skewed distributions and extreme values in commerce data [file:340]. Outliers in spend, discounts, delivery days, ticket volume, or inactivity should not be removed blindly because they may correspond to the most operationally important churn-risk cases [file:1][file:340].

Recommended handling:
- Inspect outliers visually with histograms or capped plots.
- Use robust summaries such as medians, percentiles, or bucketed analyses.
- Retain operationally meaningful extremes unless proven to be data errors.

## Leakage-sensitive columns
The capstone specifically warns against using future or post-snapshot information as inputs for model-related work [file:1]. In this dataset, the highest-risk leakage source is post-snapshot order activity if not filtered properly, while churn-label fields themselves must never be used as predictors outside evaluation or descriptive comparison [file:1][file:340].

Recommended handling:
- Never use `churn_next_60d` as an input feature.
- Filter orders and any event tables to the allowed time horizon before feature creation.
- Explicitly document leakage prevention in later repos.

## Overall assessment
The data appears analytically usable, with clean customer keys and low structural duplication, but it requires careful treatment of missing profile fields and strict time filtering to avoid leakage [file:340][file:1]. The strongest risk to submission quality is not raw corruption but misuse of future transactional information or overconfident interpretation of incomplete profile and rating fields [file:1][file:340].
