# Error Analysis

## Purpose
This report documents model errors for the D2C churn classifier. The capstone requires both false-positive and false-negative analysis, plus at least 10 specific customer examples with business interpretation [file:1].

## Why error analysis matters
In churn prediction, a false negative means the model misses a customer who is actually likely to churn, which can lead to lost revenue or missed recovery opportunities. A false positive means the model flags a customer as high risk when they were not actually going to churn, which can waste incentives, distort CRM prioritization, or annoy customers with unnecessary interventions [file:1].

## Error types
### False positives
False positives are customers predicted as churn risk who did not churn in the next 60 days. These cases often arise when the model interprets low recency, low engagement, or support activity as a danger signal even though the customer may simply be between normal purchase cycles [file:1].

Business risk:
- unnecessary coupon or incentive cost,
- operational load on CRM or support teams,
- possible margin erosion from over-targeting [file:1].

### False negatives
False negatives are customers predicted as safe who actually churned. These are often the more serious commercial error when the company depends on the model to trigger intervention [file:1].

Business risk:
- missed win-back opportunity,
- unaddressed customer dissatisfaction,
- silent churn from weak engagement or hidden friction [file:1].

## Required customer examples
Replace the placeholders below with real examples from your test predictions. Each case should include the true label, predicted label, probability score, and the main feature pattern behind the error [file:1].

### False positives
1. `CUSTOMER_ID_FP_1` — predicted high risk, actual non-churn. Explain which features looked risky and why the customer may still have stayed.
2. `CUSTOMER_ID_FP_2` — predicted high risk, actual non-churn. Note whether low activity, support contact, or recency may have been misread.
3. `CUSTOMER_ID_FP_3` — predicted high risk, actual non-churn. Explain why intervention cost might have been unnecessary.
4. `CUSTOMER_ID_FP_4` — predicted high risk, actual non-churn. Identify whether this is a threshold issue or a feature issue.
5. `CUSTOMER_ID_FP_5` — predicted high risk, actual non-churn. Comment on business tolerance for this type of error.

### False negatives
6. `CUSTOMER_ID_FN_1` — predicted low risk, actual churn. Explain which warning signs the model likely missed.
7. `CUSTOMER_ID_FN_2` — predicted low risk, actual churn. Note whether the customer had weak visible signals despite later churn.
8. `CUSTOMER_ID_FN_3` — predicted low risk, actual churn. Comment on business cost of missing this case.
9. `CUSTOMER_ID_FN_4` — predicted low risk, actual churn. Explain whether the feature set lacks a useful signal.
10. `CUSTOMER_ID_FN_5` — predicted low risk, actual churn. Note whether additional support or engagement features could help.

## Interpretation guidance
When filling the final version, look for patterns such as:
- customers with high spend but sudden inactivity,
- customers with repeated support issues but still moderate frequency,
- customers with good historical value but poor recent engagement,
- customers whose churn may be driven by factors not fully captured in the current snapshot [file:1].

## Improvement ideas
Common improvement paths include threshold recalibration, better recency feature engineering, interaction features between support and spend, and clearer handling of missing profile variables or engagement sparsity. Any proposed change should remain leakage-safe and be justified from your actual results [file:1].
