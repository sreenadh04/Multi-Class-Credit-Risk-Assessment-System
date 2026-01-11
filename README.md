# Multi-Class-Credit-Risk-Assessment-System

## Project Overview

This project presents **IntelliRisk**, a **machine learning–based multi-class credit risk assessment system** designed to classify loan applicants into **four risk categories (P1, P2, P3, P4)** using a rich combination of:

- Credit bureau history  
- Recent financial behavior  
- Customer demographic and employment attributes  

Unlike binary default prediction systems, this project focuses on **risk segmentation**, which is closer to how **real-world banks and NBFCs operate**.  
The model supports **business-driven credit decisions** such as:

- **Automatic approval** for low-risk applicants  
- **Manual underwriting review** for medium-risk applicants  
- **Rejection or stricter controls** for high-risk applicants  

The system is developed on a **large, India-scale dataset (~51,336 customers with 87 features)**, making it suitable for **high-volume lending environments**.

---

## Objectives

- Build a **robust multi-class credit risk classifier** (P1–P4)
- Prioritize **recall of high-risk customers** to minimize financial loss
- Handle **real-world data challenges**, including:
  - Non-random and informative missing values
  - Highly skewed and heavy-tailed financial distributions
  - Imbalanced delinquency behavior
- Compare multiple machine learning models and select the most business-aligned one
- Align modeling decisions with **banking risk appetite and underwriting logic**

---

## Tech Stack

### Programming Language
- Python 3

### Libraries & Frameworks
- **Data Processing:** `pandas`, `numpy`
- **Visualization:** `matplotlib`, `seaborn`, `missingno`
- **Statistical Analysis:** `scipy`, `statsmodels`
- **Machine Learning:**
  - `scikit-learn`
  - `xgboost`

### Platform
- Google Colab

---

## Dataset Description

### Source
- Two internal case study datasets:
  - `case_study1.xlsx`
  - `case_study2.xlsx`

> ⚠️ Raw datasets are not shared publicly due to confidentiality.

### Size
- **Records:** 51,336 customers  
- **Features:** 87 (before feature engineering)

### Target Variable
- `Approved_Flag`

| Class | Meaning |
|------|--------|
| P1 | Lowest risk |
| P2 | Low–medium risk |
| P3 | Medium–high risk |
| P4 | Highest risk |

### Feature Categories
- **Credit Account Summary:** Total loans, active vs closed accounts
- **Delinquency History:** Missed payments, DPD counts, delinquency severity
- **Enquiry Behavior:** Credit/loan enquiries across multiple time windows
- **Utilization & Exposure:** Secured/unsecured exposure, balance ratios
- **Demographics:** Age, gender, education, marital status
- **Employment & Income:** Net monthly income, tenure with employer

---

## Methodology

### 1. Data Integration
- Merged two datasets using `PROSPECTID`
- Final dataset shape: **(51,336 × 87)**

---

### 2. Data Cleaning
- Identified **masked missing values (`-99999`)**
- Converted them to proper `NaN`
- Verified missingness patterns using statistical summaries and visual inspection

---

### 3. Missing Value Handling (Business-Aware)

Instead of naïve imputation, missingness was treated as **informative**:

- Dropped features with **>85% missing**
- Created indicator flags:
  - `has_delinquency_history`
  - `has_enquiry_history`
  - `has_payment_history`
  - `has_credit_history`

#### Informative Imputation Strategy
- **Delinquency-related features:**  
  Filled with `0` (indicating no delinquency)
- **Time-based features:**  
  Filled with a large value representing “never occurred”
- **Percentage features:**  
  Filled with median values

This approach preserves **real-world meaning** and avoids misleading assumptions.

---

### 4. Categorical Encoding

- **Ordinal encoding:** `EDUCATION` (based on academic progression)
- **Binary mapping:** `GENDER`, `MARITALSTATUS`
- **One-hot encoding:**
  - `last_prod_enq2`
  - `first_prod_enq2`

All categorical variables were fully converted to numerical form with no leakage.

---

### 5. Outlier Detection and Handling

Financial data is naturally skewed. A **skewness-aware capping strategy** was used:

- Near-normal features → IQR-based capping  
- Right-skewed features → 1st–99th percentile capping  

This:
- Controls extreme values
- Preserves genuine risky behavior
- Prevents model instability

---

### 6. Feature Scaling

- Applied **RobustScaler** to selected continuous variables
- Rationale:
  - Financial variables are skewed and heavy-tailed
  - Median-based scaling is more stable and interpretable
  - Reduces sensitivity to extreme income or exposure values

---

## Exploratory Data Analysis (EDA)

### Analyses Performed
- Univariate distributions (histograms, boxplots)
- Numerical–numerical relationships (scatter plots)
- Numerical–categorical comparisons (boxplots, violin plots)
- Categorical–categorical analysis (normalized heatmaps)
- Correlation analysis across numerical features

### Key Insights
- Strong negative relationship between **credit score and delinquency**
- High enquiry frequency strongly associated with **higher risk classes**
- Majority of customers show **zero delinquency**, with a long tail of risky behavior
- Clear separation between risk classes based on:
  - Missed payments
  - Enquiry recency
  - Credit history age

---

## Model Implementation

### Models Trained
- Decision Tree
- Random Forest
- Logistic Regression (Multinomial)
- XGBoost
- XGBoost (Class-Weighted)

### Training Strategy
- 80–20 **stratified train-test split**
- Multi-class classification (4 classes)
- Business-driven class weighting:
  - Higher weight assigned to **P3 and P4** (riskier customers)

---

## Evaluation Metrics (Recall-First Approach)

Given the financial cost of misclassifying risky customers, **recall was treated as the primary metric**, especially for **P3 and P4** classes.

### Metrics Used
- **Recall (High-Risk Focus – P3 & P4):** Primary metric
- Precision
- F1-score
- Accuracy (reported for completeness, not optimization)
- Confusion Matrix (class-wise error analysis)

---

## Results and Observations

### Model Performance (Test Accuracy)

| Model | Accuracy |
|------|----------|
| Decision Tree | 0.7719 |
| Random Forest | 0.7636 |
| Logistic Regression | 0.7573 |
| **XGBoost** | **0.8033** |
| XGBoost (Weighted) | 0.7452 |

---

### Business-Critical Recall Analysis

In credit risk assessment, **false negatives (risky customers classified as safe)** result in **direct financial loss**, making recall more important than raw accuracy.

Key findings:

- Baseline models showed **poor recall for P3 (medium–high risk)** customers
- Class-weighted XGBoost improved **P3 recall from ~30% to ~75%**
- This improvement came with a small drop in overall accuracy but:
  - Captured significantly more risky applicants
  - Better aligned with conservative banking risk policies
  - Demonstrated cost-aware modeling decisions

---

### Best Model Selection

- **Unweighted XGBoost** achieved the highest overall accuracy
- **Weighted XGBoost** is preferred for **risk-sensitive deployment** due to:
  - Strong improvement in high-risk recall
  - Better delinquency capture
  - Alignment with real-world underwriting objectives

---

### Important Features (Top Drivers)

- Recent enquiry counts (`enq_L3m`, `enq_L6m`)
- Time since first and recent delinquency
- Credit history age
- Personal loan enquiry ratios
- Standard account counts

These features closely match **domain knowledge used by credit risk teams**.

---

## How to Run the Project

1. Clone or download the repository  
2. Open `credit_risk_model.ipynb` in **Google Colab**
3. Upload datasets:
   - `case_study1.xlsx`
   - `case_study2.xlsx`
4. Run cells sequentially:
   - Data Loading → EDA → Feature Engineering → Modeling → Evaluation

---

## Folder Structure

```text
Multi-Class-Credit-Risk-Assessment-System/
├── data/
│   ├── case_study1.xlsx
│   └── case_study2.xlsx
├── notebooks/
│   └── credit_risk_model.ipynb
├── outputs/
│   ├── plots/
│   └── reports/
├── README.md
```

## Future Improvements

* Probability threshold optimization for cost-sensitive decisions
* Cost-based loss function aligned with bank policies
* Cross-validation for production stability
* Model explainability (SHAP, feature attribution)
* Deployment-ready API using FastAPI or Flask

---

## Conclusion

This project demonstrates a **production-oriented credit risk modeling pipeline** that combines:

* Strong data understanding
* Business-aware feature engineering
* Robust machine learning techniques

The solution is scalable, interpretable, and aligned with **real-world banking risk frameworks**, making it suitable for **academic projects, hackathons, and industry evaluation**.

---

## Author

**Name:** *Annaluru Sreenadh*
