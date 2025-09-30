# Fraud Detection in Financial Transactions  

This project is a solution for the **Data Science & Machine Learning internship task from Accredian**.  
The goal is to develop a machine learning model to detect fraudulent transactions and provide actionable insights for prevention.  

---

## 📑 Table of Contents
- [Business Context](#business-context)  
- [Dataset](#dataset)  
- [Setup and Usage](#setup-and-usage)  
- [Methodology](#methodology)  
- [Answers to Project Questions](#answers-to-project-questions)  

---

## 🏦 Business Context
The objective is to **proactively detect fraudulent financial transactions**.  
This involves building a predictive model and using its insights to answer key business questions regarding fraud patterns, prevention strategies, and effectiveness measurement.  

---

## 📊 Dataset
- **Size:** 6,362,620 records and 11 columns.  
- **Content:** Transaction type, amount, customer balances before/after transaction, fraud labels.  
- **Imbalance:** Only **0.129%** of transactions are fraudulent.  

📌 Dataset: [Kaggle Link](https://www.kaggle.com/datasets/ealaxi/paysim1)  
(File: `PS_20774392719_1491204439457_log.csv`)  

---

## ⚙️ Setup and Usage
1. **Download Dataset:** Place the CSV file in the same directory as the notebook.  
2. **Install Requirements:**  
   ```bash
   pip install -r requirements.txt
