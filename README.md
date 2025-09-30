# Fraud-Detection
To Detect Fraud
Fraud Detection in Financial Transactions
This project is a solution for the Data Science & Machine Learning internship task from Accredian. The goal is to develop a machine learning model to detect fraudulent transactions and provide actionable insights for prevention.

Table of Contents

Business Context

Dataset

Setup and Usage

Methodology

Answers to Project Questions

Business Context
The objective is to proactively detect fraudulent financial transactions. This involves building a predictive model and using its insights to answer key business questions regarding fraud patterns, prevention strategies, and the measurement of their effectiveness.

Dataset
The dataset contains a simulation of mobile money transactions with a total of 6,362,620 records and 11 columns. The data includes information such as transaction type, amount, customer balances before and after the transaction, and a label indicating whether the transaction was fraudulent.

A key characteristic of this dataset is its severe class imbalance: only 0.129% of transactions are fraudulent.

Setup and Usage
To run this project, you need Python and the libraries listed below.

Download the Dataset:

The dataset can be found at this Kaggle link.

Download the file PS_20774392719_1491204439457_log.csv and place it in the same directory as your Jupyter Notebook.

Install Required Libraries:

It is highly recommended to use a virtual environment.

Create a requirements.txt file with the necessary libraries and run:

pip install -r requirements.txt

Run the Notebook:

Launch Jupyter Notebook and open the Fraud_Detection.ipynb file.

Run the cells sequentially to execute the complete pipeline: data loading, cleaning, model training, and evaluation.

Methodology
The approach follows a standard machine learning workflow, tailored to the specific challenges of fraud detection.

Data Cleaning: The script first checks for missing values. In this dataset, none were found.

Exploratory Data Analysis (EDA): The initial analysis confirmed the severe class imbalance and examined the relationships between numerical features using a correlation heatmap.

Feature Engineering & Selection:

The categorical type column was converted into numerical format using one-hot encoding, as it is a crucial predictor.

Identifier columns (nameOrig, nameDest) and the business rule column (isFlaggedFraud) were dropped as they are not suitable for direct use in the model.

Model Choice: An XGBoost Classifier was chosen. This model is highly effective for tabular data, performs well on large datasets, and includes a built-in parameter (scale_pos_weight) to effectively handle class imbalance without requiring external libraries for resampling.

Evaluation: Due to the class imbalance, accuracy is a misleading metric. The model's performance was evaluated using:

Confusion Matrix: To visualize the counts of true/false positives and negatives.

Classification Report: To assess precision and, most importantly, recall. High recall for the fraud class is critical to ensure we catch as many fraudulent transactions as possible.

ROC AUC Score: To measure the model's ability to distinguish between the two classes.

Answers to Project Questions
Here are the detailed answers to the questions posed in the task description.

1. Data cleaning including missing values, outliers and multi-collinearity.

Missing Values: The dataset was checked for missing values using df.isnull().sum(), and none were found.

Outliers: In fraud detection, outliers are often the fraudulent transactions we want to detect. Therefore, outliers in columns like amount were not removed but were treated as valid data points for the model to learn from.

Multi-collinearity: A correlation heatmap was generated. It revealed high correlation between oldbalanceOrg/newbalanceOrig and oldbalanceDest/newbalanceDest. This is expected and is not a significant issue for tree-based models like XGBoost, which are robust to multicollinearity.

2. Describe your fraud detection model in elaboration.
The model used is an XGBoost (Extreme Gradient Boosting) Classifier. It is a powerful ensemble learning algorithm that builds decision trees sequentially. Each new tree is trained to correct the errors of the previous ones, making the model highly accurate.

Why XGBoost?: It was chosen for its high performance, scalability for large datasets, and its ability to handle imbalanced data effectively.

Handling Imbalance: Instead of resampling techniques like SMOTE, I used the built-in scale_pos_weight parameter. This parameter adjusts the weight given to the minority class (fraud) during training, forcing the model to pay more attention to it. Its value was set to the ratio of non-fraudulent to fraudulent samples.

3. How did you select variables to be included in the model?
Variable selection was performed as follows:

Inclusion: All numerical columns (step, amount, balances) and the one-hot encoded type columns were included as they contain relevant transactional information.

Exclusion:

nameOrig and nameDest were dropped because they are high-cardinality categorical features (unique identifiers) that provide no generalizable patterns.

isFlaggedFraud was dropped as it's a simple business rule and not an output of a predictive process. isFraud is our true target variable.

4. Demonstrate the performance of the model by using best set of tools.
The model's performance was demonstrated using tools appropriate for imbalanced classification:

Classification Report: This provided key metrics like precision, recall, and F1-score. The model achieved high recall for the 'Fraud' class, which is the primary goal (catching as much fraud as possible).

Confusion Matrix: This visualization clearly showed the number of true positives (fraud caught), false positives (legitimate transactions flagged), true negatives, and false negatives (fraud missed).

ROC Curve and AUC Score: The model achieved a high ROC AUC score (typically > 0.99), indicating an excellent ability to distinguish between fraudulent and non-fraudulent transactions.

5. What are the key factors that predict fraudulent customer?
The model's feature importance scores revealed the top predictors:

oldbalanceOrg: The sender's balance before the transaction.

step: The time unit in the simulation.

amount: The value of the transaction.

type_TRANSFER: Whether the transaction was a transfer.

type_CASH_OUT: Whether the transaction was a cash-out.

6. Do these factors make sense? If yes, How? If not, How not?
Yes, these factors make perfect sense and align with the problem description. The data dictionary states that the fraudulent scheme is to take over customer accounts, transfer the funds out, and then cash out.

The model correctly identified type_TRANSFER and type_CASH_OUT as the most important transaction types.

oldbalanceOrg and amount are critical because fraud often involves transferring an amount that nearly or completely empties the original account.

step (time) is also important as fraudulent activities might occur in specific patterns over time.

7. What kind of prevention should be adopted while company update its infrastructure?
Based on the model's findings, the following prevention strategies should be adopted:

Real-time Anomaly Detection: Implement the trained model in the transaction pipeline. Any transaction flagged with a high probability of being fraud should be temporarily blocked pending further verification.

Rule-based Alerts: For transactions that fit the most common fraud pattern (a TRANSFER of a large percentage of the account balance followed shortly by a CASH_OUT from the destination account), trigger an immediate multi-factor authentication (MFA) challenge for the user.

Velocity Checks: Monitor the frequency of transactions for an account. A sudden spike in activity, especially transfers and cash-outs, should be flagged as suspicious.

8. Assuming these actions have been implemented, how would you determine if they work?
To determine the effectiveness of the new prevention measures, we would track the following Key Performance Indicators (KPIs):

Fraud Rate: The percentage of transactions that are fraudulent. This should decrease.

Total Financial Loss Due to Fraud: The total monetary value saved. This is the most important business metric.

False Positive Rate: The number of legitimate transactions that are incorrectly flagged or blocked. This is crucial for maintaining a good customer experience and must be kept low.

A/B Testing: Roll out the new measures to a subset of users and compare their fraud rates against a control group still on the old system. This provides a direct, data-driven measurement of the new system's impact.
