# Model Card

## Model name
D2C Customer Churn Prediction Model

## Intended use
This model is designed to estimate the probability that a customer will churn in the next 60 days so that CRM, marketing, and retention teams can prioritize outreach more effectively [file:1]. It is intended as a decision-support tool, not as a fully automated replacement for human judgment in high-value or ambiguous cases [file:1].

## Data used
The model should be trained either on the provided modeling snapshot or on a custom feature table built only from data available on or before the customer snapshot date, because the capstone explicitly warns against future-data leakage [file:1]. The underlying business signals may include recency, frequency, monetary behavior, returns, support activity, digital engagement, category diversity, ratings, discount usage, and customer profile fields where available [file:1].

## Prediction target
The target is `churn_next_60d`, a binary label indicating whether the customer churns in the 60 days following the snapshot date [file:1].

## Model approach
This repository should compare at least one baseline model with one stronger model such as Random Forest, Gradient Boosting, XGBoost, or another justified classifier, then save the final selected model based on evaluation and business trade-offs [file:1]. The exact selected model, threshold, and preprocessing steps should match the notebook and saved artifact.

## Performance
The final model performance must be recorded in `metrics.json`, including precision, recall, F1-score, ROC-AUC or PR-AUC, confusion-matrix values, and the selected decision threshold [file:1]. Accuracy alone is not sufficient for this task because churn is a business-prioritization problem where false positives and false negatives carry different costs [file:1].

## Threshold choice
The operating threshold should be chosen based on business priorities, not only statistical convenience. If the business wants to reduce missed churners, a lower threshold may be acceptable even at the cost of more false positives; if incentive budget is tight, a stricter threshold may be more appropriate [file:1].

## Limitations
- The model reflects only the signals present in the available snapshot or engineered features.
- Missing profile fields such as loyalty tier may reduce interpretability or consistency for some customers.
- Churn drivers not captured in the data, such as competitor offers or offline experiences, cannot be learned directly [file:1].

## Ethical and operational risks
This model should not be used to unfairly deprioritize customer groups based on incomplete proxies or to automate punitive decisions. The model estimates risk; it does not explain full customer intent, and it may be less reliable for sparse-data or newly acquired customers [file:1].

## Monitoring needs
After deployment or operational use, teams should monitor prediction distribution, feature drift, segment-level performance, intervention outcomes, and error concentration in important customer groups. Retraining should be triggered when performance degrades materially or when customer behavior patterns shift [file:1].

## When not to use
The model should not be used as the sole basis for high-cost retention actions, executive reporting without calibration review, or decisions involving missing or invalid inputs. It should also not be used if leakage controls were violated during feature preparation [file:1].
