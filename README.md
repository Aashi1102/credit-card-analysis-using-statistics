<p align="center">
  <img src="assets/banner.png" width="100%" alt="Atliiqo Banner"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.x-blue?logo=python" />
  <img src="https://img.shields.io/badge/Pandas-Data%20Analysis-yellow?logo=pandas" />
  <img src="https://img.shields.io/badge/NumPy-Numerical%20Computing-blue?logo=numpy" />
  <img src="https://img.shields.io/badge/Matplotlib-Visualization-orange" />
  <img src="https://img.shields.io/badge/Seaborn-Statistical%20Plots-lightblue" />
  <img src="https://img.shields.io/badge/Status-Completed-brightgreen" />
</p>

<p align="center">
  <img src="https://img.shields.io/github/stars/Aashi1102/credit-card-analysis-using-statistics?style=social" />
  <img src="https://img.shields.io/github/forks/Aashi1102/credit-card-analysis-using-statistics?style=social" />
</p>

# 🏦 AtliiQo Bank Credit Card Analysis

> **Data-driven customer segmentation and A/B testing to identify a potential target segment and evaluate campaign impact on transaction value.**

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-yellow?logo=pandas)
![NumPy](https://img.shields.io/badge/NumPy-Numerical%20Computing-blue?logo=numpy)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-orange)
![Seaborn](https://img.shields.io/badge/Seaborn-Statistical%20Plots-lightblue)
![SciPy](https://img.shields.io/badge/SciPy-Statistics-blue)
![Statsmodels](https://img.shields.io/badge/Statsmodels-Hypothesis%20Testing-green)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange)

---

## 📌 Project Overview

AtliiQo Bank is planning to introduce a new credit card in a competitive Indian market.

Before promoting the card to a large customer base, the bank needs to understand:

* Which customer segment could be a good target?
* What are the spending and credit characteristics of different customer groups?
* Can a targeted campaign increase customer transaction value?

To answer these questions, the project was divided into **two phases**:

### Phase 1 — Customer & Target Market Analysis

Customer, credit-profile, and transaction data were cleaned and analyzed to understand customer behavior and identify a potential target segment.

### Phase 2 — A/B Testing & Statistical Analysis

A campaign was evaluated by comparing the average transaction value of a **test group** with a **control group** using hypothesis testing.

### Overall Workflow

```text
Customer Data
Credit Profile Data
Transaction Data
        │
        ▼
Data Cleaning & Preprocessing
        │
        ▼
Exploratory Data Analysis
        │
        ▼
Customer Segmentation
        │
        ▼
Identify Potential Target Segment
        │
        ▼
Campaign Experiment
        │
        ▼
Control Group vs Test Group
        │
        ▼
Sample Size & Power Analysis
        │
        ▼
Hypothesis Testing
        │
        ▼
Business Interpretation
```

---

# 🎯 Business Problem

Launching a credit card without understanding customer behavior can result in poor targeting and ineffective marketing.

The goal of this project was to use customer and transaction data to:

1. Understand the characteristics of the existing customer base.
2. Analyze income, credit profile, payment behavior, and purchasing patterns.
3. Identify a potentially under-served customer segment.
4. Design an experiment for the selected segment.
5. Determine whether the campaign resulted in a statistically significant increase in average transaction value.

> **Note:** The project evaluates transaction-value improvement rather than directly measuring credit-card activation or adoption.

---

# 📊 Dataset

The analysis uses three main datasets.

| Dataset             | Initial Records | Main Information                                           |
| ------------------- | --------------: | ---------------------------------------------------------- |
| Customer Data       |           1,000 | Age, gender, location, occupation, income, marital status  |
| Credit Profile Data |           1,004 | Credit score, utilization, debt, inquiries, credit limit   |
| Transaction Data    |         500,000 | Transaction amount, platform, category, payment type, date |

### Customer Data

Important fields include:

```text
cust_id
name
gender
age
location
occupation
annual_income
marital_status
```

### Credit Profile Data

```text
cust_id
credit_score
credit_utilisation
outstanding_debt
credit_inquiries_last_6_months
credit_limit
```

### Transaction Data

```text
tran_id
cust_id
tran_date
tran_amount
platform
product_category
payment_type
```

---

# 🧹 Phase 1 — Data Cleaning & Preprocessing

The raw data contained missing values, duplicate records, unrealistic values, and unusual transaction amounts.

Different treatment methods were used depending on the variable and its business context.

---

## 1. Handling Missing Annual Income

There were **50 missing annual-income values**.

Instead of removing those customers, missing income was filled using the **median income of the customer's occupation**.

For example:

```text
Occupation
     ↓
Find occupation-wise median income
     ↓
Use that median for missing income
```

### Why median?

Income is highly skewed, so the median is less affected by very high-income values than the mean.

---

## 2. Handling Unrealistic Ages

The raw customer data contained ages ranging from:

```text
Minimum = 1
Maximum = 135
```

Ages below 15 and above 80 were treated as unrealistic values for this customer dataset.

These values were replaced using the **occupation-wise median age**.

After treatment:

```text
Minimum age = 18
Maximum age = 64
```

Customers were then divided into three age groups:

```text
18–25
26–48
49–65
```

The purpose of these groups was to make customer-segment comparison easier.

---

## 3. Cleaning Credit Profile Data

The credit-profile dataset initially contained:

```text
1,004 records
```

while there were approximately:

```text
1,000 unique customers
```

Duplicate customer IDs were identified.

The duplicate records were inspected, and the dataset was reduced to one record per customer.

### Missing Credit Limit

Missing credit limits were filled using the **mode of credit limit within the customer's credit-score range**.

Credit-score ranges were created such as:

```text
300–449
450–499
500–549
550–599
600–649
650–699
700–749
750–799
```

This provided a more relevant replacement than using one overall value.

---

## 4. Outstanding Debt Validation

Some customers had:

```text
Outstanding Debt > Credit Limit
```

This was treated as invalid according to the business rule that outstanding debt should not exceed the available credit limit.

Those values were replaced with the customer's credit limit.

This is an example of using **domain knowledge rather than only statistical rules** when cleaning data.

---

# 💳 Transaction Data Cleaning

The transaction dataset contained approximately **500,000 transactions**.

---

## 5. Missing Platform Values

The `platform` column contained missing values.

EDA showed that Amazon was the most frequently used platform across product categories.

Therefore, missing platform values were filled using the overall mode:

```text
Amazon
```

---

## 6. Zero Transaction Amounts

There were:

```text
4,734 transactions
```

with a transaction amount of zero.

Instead of immediately deleting them, the transactions were investigated.

All of these zero-value records were associated with:

```text
Platform       → Amazon
Category       → Electronics
Payment Type   → Credit Card
```

There were **15,288 transactions** in this specific combination.

For the valid positive transactions in this group, the median transaction amount was:

```text
₹554
```

The zero values were therefore replaced using this group-level median.

---

## 7. Extreme Transaction Values

The transaction amount contained unusually high values, with the maximum reaching:

```text
₹69,999
```

An IQR-based approach was used to identify extreme transaction values.

The upper threshold was approximately:

```text
₹1,107
```

Values at or above this threshold were treated as extreme observations for the analysis.

Instead of simply deleting them, the values were replaced using the **mean transaction amount for their respective product category**, calculated after excluding the extreme observations.

After treatment:

```text
Maximum transaction amount ≈ ₹999
```

The resulting transaction distribution remained right-skewed but was much less dominated by extreme values.

---

# 📈 Exploratory Data Analysis

After cleaning, the data was explored across several dimensions:

### Customer characteristics

* Age
* Gender
* Location
* Occupation
* Annual income
* Marital status

### Credit characteristics

* Credit score
* Credit utilization
* Outstanding debt
* Credit limit
* Credit inquiries

### Transaction behavior

* Transaction amount
* Payment type
* Product category
* Shopping platform
* Age group

The objective was not simply to create charts, but to answer:

> **Who are the customers, how do they behave, and which group could represent an opportunity for the new credit card?**

---

# 🔎 Credit Profile Insights

A correlation analysis was performed on numerical customer and credit variables.

Some important correlations were:

| Variable Pair                   | Correlation |
| ------------------------------- | ----------: |
| Credit Score ↔ Credit Limit     |   **0.848** |
| Credit Limit ↔ Annual Income    |   **0.685** |
| Outstanding Debt ↔ Credit Limit |   **0.811** |
| Annual Income ↔ Age             |   **0.619** |
| Credit Score ↔ Annual Income    |   **0.576** |

The strongest relationship observed was between:

```text
Credit Score ↔ Credit Limit
```

with a correlation of approximately:

```text
0.85
```

This indicates a strong positive association in this dataset.

> Correlation shows association, not causation.

---

# 🎯 Target Segment Identification

After analyzing age groups, income, credit characteristics, payment behavior, and purchasing categories, the:

# **18–25 age group**

was selected as a **potential untapped target segment** for further experimentation.

This group represented approximately:

```text
24.6% of the customer base
```

### Segment Comparison

| Metric                |   18–25 |    26–48 |    49–65 |
| --------------------- | ------: | -------: | -------: |
| Average Annual Income | ₹37,091 | ₹145,870 | ₹260,166 |
| Average Credit Limit  |  ₹1,130 |  ₹20,561 |  ₹41,699 |
| Average Credit Score  |  484.45 |   597.57 |   701.52 |

### Why 18–25?

The analysis showed that this group:

* Represents a meaningful portion of the customer base.
* Has a relatively low average income.
* Has lower average credit limits.
* Has lower average credit scores.
* Has relatively lower exposure to credit-card payments.
* Shows purchasing activity in categories such as:

  * Electronics
  * Fashion & Apparel
  * Beauty & Personal Care

This combination suggested that the segment could be worth testing rather than assuming it would automatically be the best market.

---

# 🧪 Phase 2 — A/B Testing

After identifying the potential target segment, the next step was to evaluate campaign performance.

The experiment compared:

```text
Control Group
      vs
Test Group
```

The metric analyzed was:

> **Average transaction value**

The question was:

> **Did the test group have a significantly higher average transaction value than the control group?**

---

# 📐 Sample Size & Statistical Power

Before analyzing the campaign results, sample-size planning was explored.

The parameters used were:

```text
Significance level (α) = 0.05
Statistical power      = 0.80
Effect size            = 0.20
```

For an expected effect size of `0.20`, the calculated sample requirement was approximately:

```text
393 observations per group
```

Different effect sizes were also tested:

| Effect Size | Approx. Sample per Group |
| ----------: | -----------------------: |
|         0.1 |                    1,570 |
|         0.2 |                      393 |
|         0.3 |                      175 |
|         0.4 |                       99 |
|         0.5 |                       63 |
|         1.0 |                       16 |

### Main takeaway

A smaller expected effect generally requires a larger sample size to detect reliably.

---

# 📊 Campaign Results

The post-campaign dataset contained:

```text
62 campaign-date observations
```

Each observation contained the average transaction value for the control and test groups.

### Control Group

```text
Mean = ₹221.18
SD   = ₹21.36
```

### Test Group

```text
Mean = ₹235.98
SD   = ₹36.66
```

Difference:

```text
₹235.98 − ₹221.18
= ₹14.80
```

Relative increase:

```text
≈ 6.69%
```

So, the test group had a higher average transaction value than the control group.

---

# 📐 Hypothesis Testing

A right-tailed two-sample Z-test was used.

### Null Hypothesis — H₀

There is no increase in average transaction value for the test group.

### Alternative Hypothesis — H₁

The test group's average transaction value is higher than the control group's.

In simple terms:

```text
H₀: Test ≤ Control

H₁: Test > Control
```

---

# 📊 Statistical Results

The calculated Z-statistic was:

```text
Z = 2.7466
```

At a 5% significance level, the right-tailed critical value was:

```text
Zcritical = 1.6449
```

Since:

```text
2.7466 > 1.6449
```

the null hypothesis was rejected.

### P-value

```text
p-value = 0.00301
```

Since:

```text
0.00301 < 0.05
```

the result was statistically significant at the 5% significance level.

The result was also verified using Statsmodels:

```text
Z = 2.7483
p-value = 0.002995
```

The small difference between the manually calculated and Statsmodels values comes from using rounded summary statistics in the manual calculation.

---

# 💡 Business Interpretation

The analysis provides statistical evidence that:

> **The test group had a higher average transaction value than the control group.**

In practical terms, the campaign showed evidence of improving transaction value within the tested segment.

However, this result should **not** be interpreted as:

* guaranteed credit-card adoption,
* guaranteed profitability,
* guaranteed future campaign success, or
* proof that the campaign will work for every customer segment.

The project measured **transaction value**, not direct card activation or long-term customer profitability.

---

# 🧠 Statistical Concepts Applied

This project provided practical exposure to:

### Data Analysis

* Data cleaning
* Missing-value treatment
* Outlier detection
* Exploratory Data Analysis
* Customer segmentation
* Correlation analysis

### Statistics

* A/B Testing
* Hypothesis Testing
* Z-Test
* T-Test
* Chi-Square Test
* Central Limit Theorem
* Normal Distribution
* Confidence Intervals
* P-values
* Type I Error
* Type II Error
* Effect Size
* Statistical Power
* Sample Size Calculation

---

# 🛠️ Technology Stack

| Category      | Technologies        |
| ------------- | ------------------- |
| Programming   | Python              |
| Data Analysis | Pandas, NumPy       |
| Visualization | Matplotlib, Seaborn |
| Statistics    | SciPy, Statsmodels  |
| Environment   | Jupyter Notebook    |

---

# 📂 Project Structure

```text
Atliqo-Bank-Project/
│
├── dataset/
│
├── phase_1_atliqo_bank.ipynb
├── phase_2_atliqo_bank.ipynb
│
├── Other/
│   ├── Central_Limit_therom.ipynb
│   ├── z_test_hypothesis_testing_assignment.ipynb
│   ├── t test.ipynb
│   └── abtesting.ipynb
│
└── README.md
```

---

# 🔄 End-to-End Project Workflow

```text
                 BUSINESS QUESTION
                        │
                        ▼
          ┌──────────────────────────┐
          │ Customer + Credit +      │
          │ Transaction Data         │
          └────────────┬─────────────┘
                       │
                       ▼
              DATA CLEANING
                       │
             ┌─────────┼─────────┐
             ▼         ▼         ▼
         Missing    Duplicates  Invalid
         Values      Records     Values
             │         │         │
             └─────────┼─────────┘
                       ▼
                  EDA & ANALYSIS
                       │
                       ▼
              CUSTOMER SEGMENTATION
                       │
                       ▼
                  18–25 SEGMENT
                       │
                       ▼
               EXPERIMENT DESIGN
                       │
                       ▼
             SAMPLE SIZE / POWER
                       │
                       ▼
              CONTROL vs TEST
                       │
                       ▼
          AVERAGE TRANSACTION VALUE
                       │
                       ▼
               Z-TEST / P-VALUE
                       │
                       ▼
              STATISTICAL DECISION
                       │
                       ▼
             BUSINESS INTERPRETATION
```

---

# 🎓 Key Learnings

The biggest learning from this project was that **data analysis is not just about making charts**.

The project connected:

```text
Business Problem
       ↓
Data
       ↓
Cleaning
       ↓
EDA
       ↓
Customer Segmentation
       ↓
Experiment Design
       ↓
Statistical Testing
       ↓
Business Decision
```

It helped demonstrate how statistical concepts such as **hypothesis testing, p-values, effect size, statistical power, and A/B testing** can be applied to a practical business problem.

---

# ⚠️ Limitations & Areas for Improvement

There are several ways this analysis could be improved in a real-world setting.

### 1. Direct Card Adoption

The current experiment measures transaction value rather than actual:

```text
Card Application
Card Activation
Card Usage
```

A future experiment should directly track these outcomes.

### 2. Larger Experiment

The sample-size analysis suggested approximately **393 observations per group** for an effect size of 0.20, while the available post-campaign data contained 62 date-level observations.

A larger experiment would provide stronger evidence.

### 3. Daily Aggregated Data

The Phase 2 dataset contains daily average values rather than individual customer-level observations.

A future analysis could work with customer-level experimental data and account for the time-based nature of the observations.

### 4. Campaign ROI

Statistical significance does not tell us whether the campaign was profitable.

Future analysis should include:

```text
Campaign Cost
       +
Revenue / Transaction Value
       +
Customer Acquisition Cost
       ↓
ROI
```

### 5. Predictive Modeling

A future version could build an ML model to predict:

```text
Probability of Credit Card Adoption
```

using customer demographics, income, credit profile, and transaction behavior.

### 6. Interactive Dashboard

The analysis could be converted into:

* Power BI dashboard
* Streamlit application

to allow business users to explore customer segments interactively.

---

# 🚀 Future Scope

Possible future improvements include:

* Track actual credit-card applications and activations.
* Run a larger and longer A/B test.
* Analyze customer-level campaign data.
* Predict credit-card adoption probability.
* Estimate customer lifetime value.
* Analyze campaign ROI.
* Test multiple customer segments.
* Build an interactive Power BI/Streamlit dashboard.
* Automate statistical reporting.

---

# 📌 Final Outcome

### Target Segment

**18–25 age group**

identified as a potential untapped segment based on customer characteristics, credit profile, payment behavior, and purchasing patterns.

### Campaign Result

The test group showed:

```text
Average Transaction Value
₹235.98 vs ₹221.18
```

with an approximate:

```text
6.69% increase
```

### Statistical Result

```text
Z-statistic = 2.7466
p-value     = 0.00301
α           = 0.05
```

Therefore:

```text
Reject H₀
```

and conclude that the test group showed **statistically significant evidence of a higher average transaction value**.

---

# 👩‍💻 Author

### Aashi Tomar

B.Tech Computer Science Engineering
Artificial Intelligence & Machine Learning

**Interests:**

* Machine Learning
* Data Science
* Statistics
* Artificial Intelligence
* Python

---

## ⭐ Key Takeaway

> **The project demonstrates how customer data, statistical analysis, segmentation, and experimentation can be combined to support a data-driven business decision.**
