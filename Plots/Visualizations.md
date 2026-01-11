# 📊 Visual Analysis & Business Interpretation

This section presents key visual analyses used to understand customer risk behavior, validate domain assumptions, and evaluate model performance.
Each visualization is designed to answer a **specific business question** relevant to credit underwriting.

---

## 1️⃣ Distribution of Credit Risk Classes

### 🔹 What this visualization shows

This bar chart displays the **number of customers in each risk category (P1–P4)**.

### 🔹 Why this visualization is important

* Credit risk datasets are **naturally imbalanced**
* Most customers are low or medium risk, while **high-risk customers are fewer but more costly**
* This imbalance makes **accuracy a misleading metric**

### 🔹 How to interpret it

* A dominant P2 class indicates class imbalance
* Minority classes (P3, P4) represent risky customers
* Justifies the need for:

  * Stratified sampling
  * Recall-focused evaluation
  * Class-weighted modeling

📌 **Business takeaway:**

> “The model must prioritize catching risky customers, not just predicting the majority class.”

---

## 2️⃣ Credit Score Distribution by Risk Class

### 🔹 What this visualization shows

A boxplot comparing **credit score distributions across risk classes (P1–P4)**.

### 🔹 Why this visualization is important

* Credit score is a **primary decision variable** in lending
* This plot validates whether the dataset aligns with **real-world credit logic**

### 🔹 How to interpret it

* Median credit score decreases from P1 → P4
* Higher-risk classes show lower scores and higher variability
* Confirms that:

  * Credit score is informative
  * The target labels are logically consistent

📌 **Business takeaway:**

> “Credit score alone is useful but insufficient — overlap between classes motivates multi-feature modeling.”

---

## 3️⃣ Missed Payments vs Risk Class (Violin Plot)

### 🔹 What this visualization shows

Distribution of **total missed payments** for each risk class, including density and quartiles.

### 🔹 Why this visualization is important

* Missed payments are **direct indicators of default risk**
* Averages hide extreme behavior; violins reveal **long-tail risk**

### 🔹 How to interpret it

* P1 customers cluster near zero missed payments
* P3 and P4 show:

  * Long right tails
  * High variance
* Confirms heavy-tailed delinquency behavior

📌 **Business takeaway:**

> “A small number of customers account for disproportionately high risk.”

---

## 4️⃣ Recent Enquiries vs Credit Score (Scatter Plot)

### 🔹 What this visualization shows

Scatter plot of **recent enquiries vs credit score**, colored by risk class.

### 🔹 Why this visualization is important

* Frequent credit enquiries often signal **credit hunger**
* Combined with low credit score, it is a strong **early warning signal**

### 🔹 How to interpret it

* Clusters of:

  * High enquiries + low score → P3/P4
  * Low enquiries + high score → P1/P2
* Visual evidence of interaction effects

📌 **Business takeaway:**

> “Recent enquiry behavior amplifies risk beyond credit score alone.”

---

## 5️⃣ Correlation Heatmap (Key Risk Features)

### 🔹 What this visualization shows

Correlation matrix among **important numerical credit risk variables**.

### 🔹 Why this visualization is important

* Identifies:

  * Redundant features
  * Strongly related risk indicators
* Helps in feature selection and model stability

### 🔹 How to interpret it

* Delinquency variables are strongly correlated with each other
* Demographic variables show weak correlations
* Enquiry behavior correlates with delinquency risk

📌 **Business takeaway:**

> “Behavioral features dominate risk prediction more than demographics.”

---

## 6️⃣ Confusion Matrix – Final Model (XGBoost Weighted)

### 🔹 What this visualization shows

Confusion matrix for the **final deployed model**, showing prediction vs actual class.

### 🔹 Why this visualization is important

* Reveals **where the model makes costly mistakes**
* Especially important for identifying **false negatives** in high-risk classes

### 🔹 How to interpret it

* Improved capture of P3 customers
* Slight increase in false positives is acceptable
* Trade-off favors **risk containment over approval rate**

📌 **Business takeaway:**

> “We intentionally sacrificed some accuracy to reduce financial loss.”

---

## 7️⃣ Feature Importance – XGBoost

### 🔹 What this visualization shows

Top contributing features used by the XGBoost model.

### 🔹 Why this visualization is important

* Provides **model transparency**
* Aligns ML outputs with **domain knowledge**
* Essential for regulatory and stakeholder trust

### 🔹 How to interpret it

* Top drivers include:

  * Recent enquiries
  * Delinquency recency
  * Credit history age
* Demographics play a smaller role

📌 **Business takeaway:**

> “The model learns behavior-driven risk patterns, not arbitrary signals.”

---

## 8️⃣ High-Risk Recall Improvement (Before vs After Weighting)

### 🔹 What this visualization shows

Comparison of **P3 recall** before and after class weighting.

### 🔹 Why this visualization is important

* Recall for risky customers is the **single most important metric** in credit risk
* Demonstrates **business-aware model tuning**

### 🔹 How to interpret it

* Recall improves dramatically (~30% → ~75%)
* Slight drop in overall accuracy is acceptable
* Aligns with conservative lending policies

📌 **Business takeaway:**

> “Catching risky customers matters more than maximizing approval accuracy.”
