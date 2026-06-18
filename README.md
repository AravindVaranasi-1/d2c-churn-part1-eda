# D2C Churn Part 4: FastAPI Churn Scoring Service

## Overview
This repository contains the Part 4 submission for the D2C churn capstone. The goal is to expose a churn-scoring model through a simple FastAPI service with working endpoints, validation, tests, reproducible setup, and a basic monitoring and responsible-use plan [file:1].

The API is intended for internal CRM or retention-tool usage and must be runnable on its own, which is a stated requirement for this part of the capstone [file:1].

## Repository contents
- `app/main.py` — FastAPI application entry point [file:1].
- `model.pkl` or `train_model.py` — either a saved model artifact or a script to reproduce and save the model [file:1].
- `requirements.txt` — dependencies for running the API [file:1].
- `tests/` or `test_api.py` — at least 3 API tests [file:1].
- `monitoring_plan.md` — deployment monitoring and responsible-use notes [file:1].
- `README.md` — setup, run, request/response, and testing instructions [file:1].

## Setup
Install dependencies:
```bash
pip install -r requirements.txt
```

If the repository includes `train_model.py` instead of a saved artifact, run the training script first:
```bash
python train_model.py
```

## Run the API
Start the application with:
```bash
uvicorn app.main:app --reload
```

The service should then be available locally, typically at `http://127.0.0.1:8000`.

## Required endpoints
### `GET /health`
Returns a simple health check response confirming the API is running [file:1].

Example response:
```json
{
  "status": "ok"
}
```

### `POST /predict`
Accepts one customer feature payload and returns a churn prediction response including probability, predicted class, risk level, and short explanation [file:1].

Example request:
```json
{
  "recency_days": 120,
  "frequency_180d": 1,
  "monetary_180d": 850.0,
  "return_rate_180d": 0.5,
  "ticket_count_90d": 2,
  "sessions_30d": 1,
  "last_visit_days_ago": 25
}
```

Example response:
```json
{
  "churn_probability": 0.72,
  "predicted_class": 1,
  "risk_level": "high",
  "risk_explanation": "Low recent activity and higher customer-friction signals indicate elevated churn risk."
}
```

### `POST /batch_predict`
Accepts multiple customer payloads and returns one prediction per record [file:1].

## Testing
Run tests with:
```bash
pytest -q
```

The repository should include at least three test cases covering health check, single prediction, and batch prediction behavior [file:1].

## Model and data notes
The API should load the same final model logic documented in Part 3 or a reproducible equivalent for this standalone repository. Any features used by the API must follow the same leakage-safe principle: inputs must represent information available on or before the customer snapshot date [file:1].

## Responsible use
The prediction output should support prioritization, not replace human judgment. High-risk predictions should inform outreach strategy, while ambiguous or high-value customers should still allow manual review [file:1].
