# Python-Loan_Approval_Data_Simulation
The goal of this project is to generate a synthetic dataset that mimics loan applications similar to what banks or fintech companies handle. Each record represents a loan applicant with demographic, financial, and credit-related details.

### importing python modules

import pandas as pd

import numpy as np

from datetime import datetime, timedelta

import random

### Number of rows
n = 1500

### Gender
gender = np.random.choice(['Male', 'Female'], size=n, p = [0.65, 0.35])

### Marital_Status
marital_status = np.random.choice(['Yes', 'No'], size=n, p = [0.70, 0.30])

### Dependent
dependent = np.random.choice(['0', '1', '2', '3+'], size=n, p = [0.20, 0.15, 0.30, 0.35])

### Education
education = np.random.choice(['Graduate', 'Not Graduate'], size=n, p = [0.60, 0.40])

### Self_Employed
self_employed = np.random.choice(['Yes', 'No'], size=n, p =[0.70, 0.30])

### Property_area
property_area = np.random.choice(['Urban', 'Semiurban', 'Rural'], size=n, p = [0.30, 0.40, 0.30])

### Loan_ID
loan_id = [f"L{100000 + i}" for i in range(n)]

### App_income
app_income = np.round(np.random.lognormal(mean = 8.0, sigma = 0.8, size=n )).astype(int)

### CoApp_income
coApp_income =np.round(np.random.lognormal(mean = 6.0, sigma = 0.8, size=n )).astype(int)
coApp_income[np.random.rand(n)<0.4] = 0

### Loam_Amount
Loan_amount = np.random.normal(loc=150000, scale=60000, size=n).astype(int)
Loan_amount = np.clip(Loan_amount, 20000,10000000)

### Loan_Amount_Term
loan_amount_term = np.random.choice([120, 180, 240, 300, 360, 480], size=n)

### Credit_History
credit_history = np.random.choice([0, 1], size=n, p = [0.30, 0.70])

### Probability_Base
prob_base = 0.2 + (credit_history * 0.5) + ( education == 'Graduate') * 0.05

### Total_Income
total_income = app_income + coApp_income

### Income_Ratio
income_ratio = Loan_amount / np.where(total_income == 0, 1, total_income)

### Probability_income_factor
prob_income_factor = np.where(income_ratio < 1.5, 0.15, np.where (income_ratio < 3, 0.05, -0.05))

### Probability
prob = prob_base + prob_income_factor

prob = np.clip(prob, 0.01, 0.98)

### Loan_Status
loan_status = np.where(np.random.rand(n) < prob, 'Y', 'N')

### importing Dataframe

df = pd.DataFrame({
    'Loan_ID': loan_id,
    'Gender': gender,
    'Marital_Status' : marital_status,
    'Dependent': dependent,
    'Education': education,
    'Self_Employed': self_employed,
    'Property_Area' : property_area,
    'App_Income': app_income,
    'CoApp_Income': coApp_income,
    'Loan_Amount': Loan_amount,
    'Loan_Amount_Term': loan_amount_term,
    'Credit_History': credit_history,
    'Loan_Status' : loan_status,
    'Total_Income': total_income
})
