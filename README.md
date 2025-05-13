# BNPL Risk Analysis Project

![Default Rates by Grade and DTI - Heatmap](default_by_grade_dti_heatmap.png)

## Project Overview
This analysis investigates lending risk factors with a focus on metrics relevant to Buy Now, Pay Later (BNPL) services. Using loan data from 2007-2018, I analyzed default patterns across risk grades and debt-to-income ratios to identify key risk segments for financial services applications.

## Key Findings

### 1. Default Rate by Risk Grade
Analysis revealed a clear correlation between assigned risk grades and default outcomes:
- Grade A loans (highest quality) had default rates around 3-7%
- Grade F loans showed default rates of 33-44%
- Each grade deterioration showed approximately 5-10% increased default risk

### 2. DTI as a Strong Risk Predictor
![Default Rate by DTI Range](default_by_dti_range.png)

The debt-to-income ratio proved to be a significant predictor of default risk:
- DTI below 10%: ~13% default rate
- DTI between 20-30%: ~20% default rate
- DTI above 30%: ~25% default rate

This progressive increase demonstrates how DTI thresholds could be effectively used in BNPL approval systems.

### 3. Combined Risk Matrix Analysis
The heat map visualization demonstrates the compounding effect when combining grade and DTI factors:
- Grade A borrowers maintain relatively low default rates (3-7%) even with higher DTI
- Grade G borrowers with DTI > 30% showed a 100% default rate
- Mid-grade borrowers (C-E) show significant DTI sensitivity, with default rates increasing 5-9 percentage points as DTI increases

## BNPL Applications
This analysis provides several insights applicable to BNPL risk management:

1. **Risk Tiering Strategy**: The clear stratification by grade suggests a tiered approval approach would be effective for BNPL services
2. **DTI Thresholds**: Implementing DTI cutoffs (~30%) could significantly reduce defaults
3. **Combined Scoring Model**: The risk matrix demonstrates how combining factors can create more precise risk segmentation

## Methodology
1. **Data cleaning and preparation**: Focused on key risk indicators including loan amount, loan grade, debt-to-income ratio (DTI), and default status
2. **Feature engineering**: Created BNPL-relevant metrics including:
   - Loan-to-income ratio
   - Small purchase flag (< $1000)
   - DTI buckets
   - Risk segmentation based on grade and DTI

## Technical Implementation
```python
# Key code snippets
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Data preparation
df['is_default'] = df['loan_status'].isin(['Charged Off', 'Default']).astype(int)
df['loan_to_income'] = df['loan_amnt'] / df['annual_inc']
df['small_purchase'] = (df['loan_amnt'] < 1000).astype(int)
df['dti_bucket'] = pd.cut(df['dti'], bins=[0, 10, 20, 30, 100])

# Risk segmentation
conditions = [
    (df['grade'].isin(['A','B'])) & (df['dti'] < 15),
    (df['grade'].isin(['F','G'])) | (df['dti'] > 35)
]
choices = ['Low Risk', 'High Risk']
df['risk_segment'] = np.select(conditions, choices, default='Medium Risk')

# Risk matrix visualization
risk_matrix = df.pivot_table(index='grade', columns='dti_bucket', values='is_default', aggfunc='mean')
sns.heatmap(risk_matrix, annot=True, fmt=".0%", cmap="YlOrRd")
plt.title('Default Rates by Grade and DTI')
plt.savefig('risk_matrix.png')
```

## Next Steps
With more BNPL-specific data, this analysis could be enhanced by:
1. Incorporating repayment behavior on smaller, shorter-term loans
2. Analyzing the impact of multiple concurrent BNPL commitments
3. Developing an automated risk scoring system specifically calibrated for BNPL lending patterns

---

## About Me
Final-year Computer Science student specializing in data analytics and financial risk modeling. Experienced in Python, SQL, and Power BI, with a focus on creating actionable insights from complex financial datasets. Connect with me on [LinkedIn](https://www.linkedin.com/in/your-profile).

## Repository Contents
- `tabby_project.py`: Main analysis script
- `default_by_grade_dti_heatmap.png`: Heat map visualization of default rates
- `default_by_dti_range.png`: Bar chart of default rates by DTI
- `risk_matrix.csv`: Exported risk segmentation data
