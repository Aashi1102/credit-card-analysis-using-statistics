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

# 🏦 Atliiqo Bank Credit Card Analysis

A data-driven customer and transaction analysis project that uses **Exploratory Data Analysis, customer segmentation, A/B testing, and hypothesis testing** to identify a potential target segment for Atliiqo Bank's new credit card and evaluate whether a campaign increased customer transaction value.

---

## 📌 Project Overview

Atliiqo Bank wants to launch a new credit card in a competitive Indian market.

Before launching the card broadly, the bank needs to answer two important questions:

1. **Which customer segment should be targeted?**
2. **Does the campaign actually increase customer transaction value?**

To answer these questions, the project was divided into two phases:

```text
Customer + Credit + Transaction Data
                ↓
        Data Cleaning & EDA
                ↓
         Customer Segmentation
                ↓
      Identify Target Segment
                ↓
           A/B Testing
                ↓
      Control Group vs Test Group
                ↓
        Hypothesis Testing
                ↓
       Statistical Decision
                ↓
        Business Recommendation
```

---

# 🎯 Problem Statement

Launching a new credit card without understanding customer behavior can lead to poor targeting and ineffective marketing.

The objective of this project was to use customer, credit, and transaction data to:

* understand customer characteristics and spending behavior
* identify a potentially untapped customer segment
* design an A/B testing approach for the selected segment
* determine whether the campaign produced a statistically significant increase in average transaction value

---

# 🧠 Project Approach

The project consists of two major phases.

## Phase 1 — Target Market Analysis

The first phase focused on understanding the customer base and identifying a suitable target segment.

### Main steps

```text
Data Loading
     ↓
Data Inspection
     ↓
Data Cleaning
     ↓
Missing Value Treatment
     ↓
Outlier Detection & Treatment
     ↓
Exploratory Data Analysis
     ↓
Customer Segmentation
     ↓
Target Segment Identification
```

---

# 📊 Data Used

The analysis worked with three main datasets:

| Dataset           |         Records |
| ----------------- | --------------: |
| Customer Data     |           1,000 |
| Credit Score Data | 1,004 initially |
| Transaction Data  |         500,000 |

The credit-score data contained duplicate customer records, which were checked using `cust_id` and reduced to the required unique customer records.

---

# 🧹 Data Cleaning & Preprocessing

Real-world datasets contained missing values, duplicate records, unrealistic values, and unusual transaction values.

Instead of applying the same cleaning method everywhere, the treatment was based on the type of data and the business context.

## Missing Income Values

Missing annual income values were handled using the **occupation-wise median income**.

This was preferred over simply removing the records because income can vary considerably across occupations.

---

## Age Outliers

The dataset contained unrealistic ages, including values as low as **1** and as high as **135**.

Potential age outliers were identified and treated using occupation-wise median age.

After treatment:

* Minimum age: **18**
* Maximum age: **64**

Customers were then grouped into:

```text
18–25
26–48
49–65
```

---

## Credit Score Data

The credit-score dataset initially contained **1,004 records** for approximately **1,000 customers**.

Duplicate customer IDs were identified and removed to maintain one customer-level record.

Credit-score ranges were also created for analysis.

---

## Outstanding Debt

Some records contained outstanding debt greater than the customer's credit limit.

Instead of relying only on statistical outlier detection, a business rule was applied:

```text
Outstanding Debt > Credit Limit
                ↓
          Treat as invalid
                ↓
      Replace using Credit Limit
```

This demonstrates the use of **domain/business logic during data cleaning**.

---

## Transaction Data

The transaction dataset contained approximately **500,000 records**.

Zero-value transactions were investigated rather than immediately deleted.

There were **4,734 zero-value transactions**, with notable concentration around certain product/platform combinations.

These values were treated using relevant group-level transaction statistics.

Extreme transaction values were also identified and treated using product-category-level statistics.

---

# 📈 Exploratory Data Analysis

The cleaned data was explored across multiple customer and transaction dimensions, including:

* Age
* Annual income
* Credit score
* Credit limit
* Occupation
* Payment method
* Product category
* Platform
* Transaction amount

The goal was not simply to create visualizations, but to understand:

> **Who are the customers, how do they behave, and which group could represent an opportunity for the new credit card?**

---

# 🎯 Target Segment Identification

The analysis identified the:

# **18–25 age group**

as a potential untapped customer segment.

The segment represented approximately:

**24.6% of the customer base**

### Key characteristics

| Metric               |   18–25 |    26–48 |    49–65 |
| -------------------- | ------: | -------: | -------: |
| Average Income       | ₹37,091 | ₹145,870 | ₹260,166 |
| Average Credit Limit |  ₹1,130 |  ₹20,561 |  ₹41,699 |
| Average Credit Score |  484.45 |   597.57 |   701.52 |

The younger segment also showed relatively low credit-card usage compared with the older groups.

At the same time, their transaction activity showed interest in categories such as:

* Electronics
* Fashion & Apparel
* Beauty & Personal Care

This combination made the 18–25 segment a potential target for further experimentation.

---

# 🧪 Phase 2 — A/B Testing

After identifying the target segment, the second phase focused on evaluating the campaign.

The experiment compared:

```text
Control Group
      vs
Test Group
```

The objective was to determine whether the test group achieved a higher average transaction value.

---

# 📐 Sample Size Planning

Before conducting the experiment, sample-size requirements were examined using statistical power analysis.

The main parameters considered were:

```text
Significance level (α) = 0.05
Statistical power      = 0.80
Effect size            = 0.20
```

For an effect size of **0.20**, the calculated requirement was approximately:

**393 observations per group**

Different effect sizes were also evaluated to understand the trade-off between the expected effect and required sample size.

| Effect Size | Approx. Required Sample |
| ----------: | ----------------------: |
|         0.1 |                   1,570 |
|         0.2 |                     393 |
|         0.3 |                     175 |
|         0.4 |                      99 |
|         0.5 |                      63 |
|         1.0 |                      16 |

This helped demonstrate an important experimental-design concept:

> **Smaller expected effects generally require larger samples to detect reliably.**

---

# 📊 Post-Campaign Analysis

The post-campaign data contained **62 campaign-date observations** with average transaction values for the control and test groups.

### Control Group

* Mean transaction value: **221.18**
* Standard deviation: **21.36**

### Test Group

* Mean transaction value: **235.98**
* Standard deviation: **36.66**

The test group therefore had a higher average transaction value:

```text
235.98 − 221.18
= 14.80
```

This represents approximately a **6.69% increase relative to the control-group mean**.

---

# 📐 Hypothesis Testing

A **right-tailed two-sample Z-test** was used to determine whether the test group's average transaction value was significantly higher than the control group's.

### Null Hypothesis — H₀

There is no increase in average transaction value for the test group.

### Alternative Hypothesis — H₁

The test group's average transaction value is higher than the control group's.

---

# 📊 Statistical Result

The calculated test statistic was approximately:

**Z = 2.7466**

The critical value at a 5% significance level for the right-tailed test was approximately:

**1.6449**

Since:

```text
2.7466 > 1.6449
```

the null hypothesis was rejected.

The calculated p-value was approximately:

**0.00301**

Since:

```text
0.00301 < 0.05
```

the result was statistically significant at the 5% level.

The result was also independently verified using `Statsmodels`, producing approximately:

```text
Z = 2.7483
p = 0.002995
```

---

# 💡 Business Interpretation

The statistical analysis provides evidence that the test group had a significantly higher average transaction value than the control group.

In simple terms:

> **The campaign showed evidence of increasing average transaction value within the tested segment.**

However, statistical significance should not automatically be interpreted as proof that the campaign will produce the same result for every customer or future campaign.

---

# 🧠 Statistical Concepts Used

This project provided hands-on implementation of:

### Data Analysis

* Data cleaning
* Missing-value treatment
* Outlier detection
* Exploratory Data Analysis
* Customer segmentation

### Statistical Analysis

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

# 🛠️ Tech Stack

### Programming

* Python

### Data Analysis

* Pandas
* NumPy

### Visualization

* Matplotlib
* Seaborn

### Statistics

* SciPy
* Statsmodels

### Environment

* Jupyter Notebook

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

# 🔄 Complete Workflow

```text
Customer Data
Credit Score Data
Transaction Data
        ↓
Data Cleaning
        ↓
Missing Value Treatment
        ↓
Outlier Detection
        ↓
Exploratory Data Analysis
        ↓
Customer Segmentation
        ↓
18–25 Target Segment
        ↓
Experiment Design
        ↓
Sample Size & Power Analysis
        ↓
Control vs Test
        ↓
Average Transaction Comparison
        ↓
Two-Sample Z-Test
        ↓
p-value = 0.00301
        ↓
Reject H₀
        ↓
Evidence of Higher Transaction Value
```

---

# 🎓 Key Learnings

The main learning from this project was that **data analysis and statistics can directly support business decisions**.

Instead of stopping at visualization, the project followed the complete path:

```text
Business Question
       ↓
Data
       ↓
Cleaning
       ↓
Analysis
       ↓
Target Identification
       ↓
Experiment
       ↓
Statistical Testing
       ↓
Business Decision
```

The project also demonstrated why proper data cleaning, experiment design, and statistical interpretation are important before making conclusions from customer data.

---

# 🔮 Future Improvements

Possible extensions include:

* Use larger real-world campaign datasets
* Track actual card adoption/activation
* Perform longer-term A/B testing
* Analyze customer lifetime value
* Build predictive models for card adoption
* Create a Power BI or Streamlit dashboard
* Test additional customer segments
* Incorporate campaign cost and ROI into the final decision

---

# 👩‍💻 Author

## Aashi Tomar

B.Tech Computer Science (AI & ML)

Interested in:

* Machine Learning
* Data Science
* Statistics
* Artificial Intelligence
* Python

---

⭐ If you found this project useful, consider giving the repository a star!
