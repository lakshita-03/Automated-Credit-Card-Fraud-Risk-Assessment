# Automated Credit Card Fraud Risk Assessment

## Project Overview
- This project focuses on **automated credit card fraud risk assessment** using machine learning and workflow automation. The **Sparkov Credit Card Transactions** dataset is used to train and evaluate fraud detection models.
- The project uses **Logistic Regression as a baseline and XGBoost as the final model** to generate transaction-level fraud predictions, risk scores, and risk levels.
- The XGBoost model is deployed through **FastAPI** and connected to **n8n** to automate fraud alert processing, with **Google Gemini** generating structured case notes for flagged transactions.

## Data Sources
The project uses the **Sparkov Credit Card Transactions dataset**, which consists of two CSV files: **`fraudTrain.csv`** and **`fraudTest.csv`**.
### Dataset Summary

| Attribute | Details |
|---|---|
| Dataset | Sparkov Credit Card Transactions |
| Data Files | `fraudTrain.csv`, `fraudTest.csv` |
| Total Transactions | 1,296,675 |
| Total Columns | 23 |
| Target Variable | `is_fraud` |
| Fraud Rate | ~0.58% |

The dataset contains simulated credit card transaction records with information about customers, merchants, transaction amounts, locations, and fraud status. The data is highly imbalanced, with fraudulent transactions representing a small proportion of total transactions.

### Data Files

- **`fraudTrain.csv`** — Used for model training and development.
- **`fraudTest.csv`** — Used for evaluating the trained models on unseen transactions.

### Key Columns

| Column | Description |
|---|---|
| `trans_date_trans_time` | Transaction date and time |
| `cc_num` | Credit card/account identifier |
| `merchant` | Merchant associated with the transaction |
| `category` | Transaction category |
| `amt` | Transaction amount |
| `city`, `state`, `zip` | Customer location information |
| `lat`, `long` | Customer coordinates |
| `merch_lat`, `merch_long` | Merchant coordinates |
| `city_pop` | Population of customer's city |
| `job` | Customer occupation |
| `dob` | Customer date of birth |
| `trans_num` | Unique transaction identifier |
| `is_fraud` | Fraud indicator / target variable |

### Target Variable

- `0` → Legitimate transaction
- `1` → Fraudulent transaction

The exploratory analysis identified significant class imbalance, along with differences in transaction amounts and fraud rates across transaction categories.

## Architecture   

<img src="picture/Fraud Detection Workflow Architecture.png" alt="banner" width="47%"> 

### Workflow Steps
1. **Transaction Input**- A transaction is provided to the fraud detection pipeline with the required transaction attributes.
2. **Preprocessing**-The transaction data is cleaned and transformed using the preprocessing steps developed during model training.
3. **Fraud Prediction**-The trained XGBoost model generates a probability-based fraud risk score.
4. **Risk Classification**-The risk score is converted into a risk level such as Low, Medium, or High, along with the final fraud prediction.
5. **FastAPI Integration**-The trained model is exposed through a FastAPI endpoint, allowing external applications and automation workflows to request fraud predictions.
6. **n8n Automation**- n8n acts as the workflow orchestration layer. It receives the transaction/prediction information and controls the subsequent fraud-triage process.
7. **AI Case-Note Generation**- Google Gemini receives the relevant transaction and fraud-risk information and generates a structured case note that can assist an analyst in reviewing the flagged transaction.
8. **Final Output**- The workflow produces a structured fraud-alert/case-note output containing the model's risk assessment and AI-generated investigation context.
