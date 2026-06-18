# Monitoring Plan

## Purpose
This document outlines what should be monitored after deployment of the FastAPI churn-scoring service. The capstone requires monitoring coverage for data drift, prediction distribution, business outcomes, API errors, retraining triggers, and responsible use [file:1].

## 1. Data drift
Track whether incoming feature distributions differ materially from the data used to train the model. Important examples include recency, order frequency, monetary value, return rate, ticket volume, sessions, and last-visit patterns [file:1].

Suggested checks:
- daily or weekly summary statistics,
- PSI or similar drift metrics,
- share of missing or invalid inputs,
- sudden changes in key categorical distributions.

## 2. Prediction distribution
Monitor the volume and distribution of predicted churn probabilities and predicted classes over time. A sudden jump in high-risk predictions may indicate data drift, upstream pipeline issues, or real business deterioration [file:1].

Suggested checks:
- average predicted probability,
- percentage classified as high risk,
- segment-level or channel-level score distributions,
- comparison with prior weeks.

## 3. Business outcomes
Prediction quality should be connected to downstream business outcomes rather than monitored only as a technical artifact. The retention team should evaluate whether flagged customers are actually more likely to churn and whether interventions improve retention or margin-adjusted value [file:1].

Suggested checks:
- churn rate by predicted risk band,
- retention uplift after intervention,
- coupon cost versus saved revenue,
- response rate by treatment type.

## 4. API reliability and errors
Since this is a production-style service endpoint, technical health matters alongside model quality. The team should monitor uptime, latency, validation errors, unexpected exceptions, and failed batch requests [file:1].

Suggested checks:
- endpoint success rate,
- average latency and p95 latency,
- count of 4xx and 5xx responses,
- malformed payload frequency,
- log volume by endpoint.

## 5. Retraining triggers
Retraining should not be purely calendar-based. It should be triggered when there is evidence that customer behavior, data quality, or model performance has shifted enough to reduce usefulness [file:1].

Possible triggers:
- sustained drop in precision/recall on monitored outcomes,
- persistent feature drift,
- changes in campaign strategy or acquisition mix,
- new product or channel launches,
- large shifts in customer support or return behavior.

## 6. Responsible-use note
The API output should be used to prioritize outreach, not to automate blanket discounting or deny service. High-risk predictions should be combined with customer value, support context, and campaign budget limits before action is taken [file:1].

The team should avoid treating the score as a complete explanation of customer intent. Particularly for high-value, new, or mixed-signal customers, human review remains appropriate [file:1].
