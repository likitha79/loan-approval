# Loan Eligibility Prediction System

A machine learning-based web application that estimates a loan applicant's approval probability, eligible loan amount, and estimated monthly EMI from financial and personal inputs.

## Live Demo

**Web Application:** https://loan-eligibility-project-sstz.onrender.com/

> This project is intended for educational and preliminary screening purposes. Predictions are estimates and are not a guarantee of loan approval.

## Project Overview

Traditional loan assessment can be time-consuming and inconsistent. This project uses supervised machine learning to provide a quick, data-driven estimate of loan eligibility through a Flask web application with user registration and login.

The application accepts ten input features:

- Monthly income
- Employment type
- Credit score
- Number of existing loans
- Existing monthly EMI
- Number of dependents
- Savings balance
- Loan type
- Requested loan amount
- Tenure in months

It returns three results:

- **Approval probability** — estimated probability that the application is approved
- **Eligible loan amount** — predicted eligible amount
- **Estimated monthly EMI** — calculated using the predicted eligible amount, requested tenure, and a 10.5% annual interest rate

## Features

- User registration and login
- Password hashing with Werkzeug security utilities
- Session-protected loan eligibility form
- Machine learning-based approval probability prediction
- Eligible loan amount prediction
- Monthly EMI calculation
- JSON-based `/predict` API endpoint
- Input validation and error handling

## Technology Stack

| Area | Technologies |
|---|---|
| Programming language | Python |
| Web framework | Flask |
| Machine learning | scikit-learn, XGBoost |
| Data processing | pandas, NumPy |
| Model persistence | joblib |
| Frontend | HTML, Jinja2 templates |
| Authentication security | Werkzeug password hashing, Flask sessions |
| Deployment | Render |

## Dataset and Preprocessing

The project report describes a dataset of **1,000 loan applicant records** with 31 original columns. Ten applicant features are used as model inputs.

Preprocessing and model preparation include:

- Handling missing values where required
- Selecting relevant features and excluding identifier-type fields
- Encoding categorical features such as employment type and loan type
- Applying Z-score standardization to numerical features
- Splitting the data into 80% training and 20% testing sets
- Using 5-fold cross-validation and Grid Search for hyperparameter tuning

For the binary approval task, the report groups `Approved` and `Partially Approved` as positive outcomes and `Rejected` as the negative outcome.

## Machine Learning Models

Two prediction tasks are implemented:

1. **Classification:** estimates the probability of loan approval.
2. **Regression:** estimates the eligible loan amount.

The evaluated algorithms include Logistic Regression, K-Nearest Neighbours (KNN), Random Forest, XGBoost, and Linear Regression for the loan-amount baseline.

### Reported Evaluation Results

#### Loan approval classification

| Model | Accuracy | F1-score | ROC-AUC |
|---|---:|---:|---:|
| Logistic Regression | 83.00% | 0.9000 | 0.8895 |
| KNN (K = 7) | 93.00% | 0.9628 | 0.8321 |
| Random Forest Classifier | 94.00% | 0.9681 | 0.9045 |
| **XGBoost Classifier** | **95.00%** | **0.9733** | **0.9246** |

#### Eligible loan amount regression

| Model | R² score | RMSE |
|---|---:|---:|
| Linear Regression | 0.6546 | ₹662,738 |
| Random Forest Regressor | 0.7082 | ₹609,143 |
| **XGBoost Regressor** | **0.7609** | **₹551,410** |

Based on the reported test results, the XGBoost Classifier was selected for approval prediction and the XGBoost Regressor for eligible loan amount prediction.

## EMI Calculation

The estimated monthly EMI is calculated using the reducing-balance formula:

\[
EMI = \frac{P \times r \times (1+r)^n}{(1+r)^n - 1}
\]

Where:

- `P` is the predicted eligible loan amount
- `r` is the monthly interest rate (`10.5 / 12 / 100`)
- `n` is the tenure in months

If the interest rate is zero, the EMI is calculated as `P / n`.

## Application Routes

| Route | Method | Purpose |
|---|---|---|
| `/register` | GET, POST | Register a new user |
| `/login` | GET, POST | Authenticate a user |
| `/logout` | GET | End the user session |
| `/` | GET | Display the protected loan eligibility form |
| `/predict` | POST | Validate inputs and return prediction results as JSON |

The `/predict` endpoint returns `approval_probability`, `eligible_loan_amount`, and `estimated_emi`.

## Run Locally

The steps below assume that you have the project source code and the required model files.

### 1. Clone the repository

```bash
git clone https://github.com/likitha79/loan-approval.git
cd loan-approval
```

### 2. Create and activate a virtual environment

```bash
python -m venv venv
```

**Windows (PowerShell):**

```powershell
.\venv\Scripts\Activate.ps1
```

**macOS / Linux:**

```bash
source venv/bin/activate
```

### 3. Install dependencies

If the repository contains a `requirements.txt` file:

```bash
pip install -r requirements.txt
```

Otherwise, install the packages required by the application (adjust versions to match the project):

```bash
pip install Flask scikit-learn xgboost pandas numpy joblib
```

### 4. Check the required files

Confirm that the source code includes the Flask application, required HTML templates, trained model files, and any required data/configuration files. The project report refers to:

- `app.py`
- `models/loan_approval_model.pkl`
- `models/loan_amount_model.pkl`
- `templates/login.html`
- `templates/register.html`
- `templates/index.html`
- `data/users.json`

The actual repository layout may differ. Do not commit real credentials or secret keys.

### 5. Start the application

```bash
python app.py
```

Open the local address printed by Flask in your terminal (commonly `http://127.0.0.1:5000`).

=> Example Prediction Response

A successful prediction returns JSON in this general format:

```json
{
  "approval_probability": 0.8735,
  "eligible_loan_amount": 1250000.0,
  "estimated_emi": 26886.42
}
```

These values are an illustrative example from the project report, not a live prediction.

## Limitations

- Model estimates depend on the dataset and training process.
- The reported evaluation scores do not guarantee the same performance on new applicants.
- The estimated EMI uses a fixed annual interest rate of 10.5%; actual lender rates and charges may differ.
- The output is not a formal lending decision or financial advice.
- The project should be reviewed for fairness, bias, security, and data privacy before real-world use.

## Future Enhancements

- Integrate real-time credit bureau and bank statement data
- Add explainability using tools such as SHAP
- Replace JSON-based user storage with a database such as PostgreSQL
- Add an administrator dashboard for loan officers
- Support interest rates that vary by loan type, tenure, and credit score
- Conduct fairness and bias audits and retrain models periodically

## References

- [Scikit-learn Documentation](https://scikit-learn.org/stable/)
- [Flask Documentation](https://flask.palletsprojects.com/)
- [XGBoost Documentation](https://xgboost.readthedocs.io/)
- [Werkzeug Documentation](https://werkzeug.palletsprojects.com/)
- [Live Web Application](https://loan-eligibility-project-sstz.onrender.com/)

## Contributors

 Y. Chaitanya — [GitHub](https://github.com/yakkalichaitanya/loan-eligibility-project-amount)
 T. Likitha — [GitHub](https://github.com/likitha79/loan-approval)
 S. Rohit — [GitHub](https://github.com/sontirohit999/Loan_Eligibilty)
 S. DivyaSri — [GitHub](https://github.com/divyasrisimhadri/loan-eligibility-project)

---

Developed as an academic Data Mining project for the 2026–2027 academic year at Prasad V. Potluri Siddhartha Institute of Technology.
