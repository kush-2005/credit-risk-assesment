# Credit Risk Prediction

A web app that predicts the probability of a loan applicant defaulting. A trained XGBoost/scikit-learn pipeline is served through a FastAPI backend, with a simple HTML/CSS/JS frontend.

## Project Structure

```
credit_risk/
├── main.py                    # FastAPI app (/predict endpoint + static frontend)
├── Credit_Risk.ipynb          # Data exploration, training, threshold tuning
├── credit_risk_dataset.csv    # Training dataset
├── credit_risk_model.pkl      # Trained model pipeline
├── best_threshold.pkl         # Tuned decision threshold
├── static/                    # Frontend (index.html, style.css, script.js)
├── requirements.txt           # Python dependencies
├── runtime.txt                # Python runtime version
├── render.yaml                # Render deployment config
└── gitignore.txt              # Git ignore rules
```

## Getting Started

```bash
pip install -r requirements.txt
uvicorn main:app --reload
```

Open http://127.0.0.1:8000 for the UI, or http://127.0.0.1:8000/docs for the interactive API docs.

## API

### `POST /predict`

Request body:

| Field | Type | Description |
|---|---|---|
| `person_age` | int | Applicant age |
| `person_income` | float | Annual income |
| `person_home_ownership` | str | e.g. `RENT`, `OWN`, `MORTGAGE`, `OTHER` |
| `person_emp_length` | float | Employment length (years) |
| `loan_intent` | str | Purpose of the loan |
| `loan_grade` | str | Loan grade (A–G) |
| `loan_amnt` | float | Loan amount |
| `loan_int_rate` | float | Interest rate |
| `loan_percent_income` | float | Loan amount as a fraction of income |
| `cb_person_default_on_file` | str | `Y` / `N` – prior default on file |
| `cb_person_cred_hist_length` | int | Credit history length (years) |

Response:

```json
{
  "default_probability": 0.12,
  "default_prediction": 0,
  "threshold": 0.45,
  "Result": "Low Risk"
}
```

An applicant is flagged **High Risk** when `default_probability >= threshold`.

## Deployment

Configured for [Render](https://render.com) via `render.yaml` (free web service, auto-deploy on push). Start command: `uvicorn main:app --host 0.0.0.0 --port $PORT`.

## Tech Stack

FastAPI, Uvicorn, Pydantic, pandas, scikit-learn, XGBoost, joblib.
