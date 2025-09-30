Fraud Detection in Financial Transactions

This project is a solution for the Data Science & Machine Learning internship task from Accredian.
The goal is to develop a machine learning model to detect fraudulent transactions and provide actionable insights for prevention.

📑 Table of Contents

Business Context

Dataset

Setup and Usage

Methodology

Answers to Project Questions

🏦 Business Context

The objective is to proactively detect fraudulent financial transactions.
This involves building a predictive model and using its insights to answer key business questions regarding fraud patterns, prevention strategies, and effectiveness measurement.

📊 Dataset

Size: 6,362,620 records and 11 columns.

Content: Transaction type, amount, customer balances before/after transaction, fraud labels.

Imbalance: Only 0.129% of transactions are fraudulent.

📌 Dataset: Kaggle Link

(File: PS_20774392719_1491204439457_log.csv)

⚙️ Setup and Usage

Download Dataset: Place the CSV file in the same directory as the notebook.

Install Requirements:

pip install -r requirements.txt


Run Notebook:

jupyter notebook Fraud_Detection.ipynb


Execute all cells sequentially for the full pipeline (data cleaning → training → evaluation).

🧠 Methodology

Data Cleaning: No missing values found.

EDA: Confirmed severe imbalance, correlation heatmap generated.

Feature Engineering:

One-hot encoding for type.

Dropped nameOrig, nameDest, isFlaggedFraud.

Model Choice:

XGBoost Classifier chosen for scalability, robustness to imbalance, and high performance.

Used scale_pos_weight to handle imbalance instead of resampling.

Evaluation Metrics:

Confusion Matrix

Classification Report (precision, recall, F1)

ROC Curve & AUC

❓ Answers to Project Questions
🔹 Data Cleaning (Missing values, outliers, multicollinearity)

Missing Values: None.

Outliers: Retained, as they often represent fraud cases.

Multicollinearity: Found correlations in balance columns, not an issue for XGBoost.

🔹 Model Description

Algorithm: XGBoost (Extreme Gradient Boosting).

Why XGBoost? Handles large, imbalanced tabular datasets well.

Imbalance Handling: Used scale_pos_weight instead of SMOTE.

🔹 Variable Selection

Included: Transaction features (step, amount, balances, encoded type).

Excluded:

nameOrig, nameDest → high-cardinality identifiers.

isFlaggedFraud → rule-based, not predictive.

🔹 Model Performance

Classification Report: High recall for fraud class.

Confusion Matrix: Showed true positives vs false positives/negatives.

ROC AUC Score: >0.99 (excellent fraud vs non-fraud separation).

🔹 Key Fraud Predictors

oldbalanceOrg – Sender’s balance before transaction.

step – Time unit in simulation.

amount – Transaction value.

type_TRANSFER – Transfer transactions.

type_CASH_OUT – Cash-out transactions.

📌 These align with known fraud schemes: account takeovers → transfers → cash-out.

🔹 Prevention Strategies

Real-time Anomaly Detection: Block suspicious transactions temporarily.

Rule-based Alerts: Trigger MFA for transfer + cash-out sequences.

Velocity Checks: Flag sudden spikes in activity.

🔹 Measuring Effectiveness

Fraud Rate: Should decrease.

Financial Loss Due to Fraud: Should reduce.

False Positive Rate: Must remain low to preserve customer trust.

A/B Testing: Compare new prevention system vs old system.

📌 Conclusion

This project demonstrates how machine learning can effectively detect fraudulent financial transactions using XGBoost and appropriate evaluation metrics.
The results highlight key fraud patterns and provide actionable prevention strategies for real-world deployment.
