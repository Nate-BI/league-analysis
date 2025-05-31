
# League Role Analysis
This is a DSC 80 project website.
# 🎮 Who Carries More in League of Legends?

**Name:** Haoshuo Bi  
**UCSD DSC 80 Final Project – Spring 2025**

---

## Introduction

We analyze whether **Mid** or **Bot** players contribute more to their teams in competitive League of Legends matches.  
Using data from the 2022 season, we explore statistical performance metrics, conduct hypothesis testing, and train predictive models to understand carry potential across roles.

---

## Data Cleaning and Exploratory Data Analysis

We removed team-level rows, filtered for player roles (top, jng, mid, bot, sup), and cleaned key columns (`kills`, `deaths`, `assists`, `dpm`).  
We also removed rows with missing values in those key performance columns.

### Position Distribution  
<iframe src="assets/position-distribution.html" width="800" height="600" frameborder="0"></iframe>

### DPM Distribution  
<iframe src="assets/dpm-distribution.html" width="800" height="600" frameborder="0"></iframe>

### DPM by Position  
<iframe src="assets/dpm-by-position.html" width="800" height="600" frameborder="0"></iframe>

### KDA by Position  
<iframe src="assets/kda-by-position.html" width="800" height="600" frameborder="0"></iframe>

---

## Assessment of Missingness

We simulated 30% missing values (MCAR) in the `dpm` column to evaluate whether missingness depends on player role or team.  
Permutation tests were performed to determine if observed differences are statistically significant.

### DPM Missingness by Position  
<iframe src="assets/dpm-missing-by-position.html" width="800" height="600" frameborder="0"></iframe>

```text
Observed stat (position): 0.0022  
p-value: 0.9890
```

> Conclusion: There is no significant evidence that missingness depends on position.

---

## Hypothesis Testing

We tested whether Mid and Bot players differ significantly in KDA using a permutation test.

**Null Hypothesis:** Mid and Bot players have the same KDA distribution.  
**Alternative Hypothesis:** Their KDA distributions are different.

### KDA Hypothesis Test  
<iframe src="assets/kda-hypothesis-test.html" width="800" height="600" frameborder="0"></iframe>

```text
Observed difference in KDA (mid - bot): -0.1980 
p-value: 0.0000
```

> Conclusion: The result is statistically significant (p < 0.05), suggesting Mid players tend to have higher KDA.

---

## Framing a Prediction Problem

We aim to predict whether a player is **Mid** or **Bot** based on their performance stats.  
This reframes the role comparison as a binary classification task using numerical features: `kills`, `deaths`, `assists`, and `dpm`.

---

## Baseline Model

We trained a logistic regression model using scaled numerical features.  
This model serves as a baseline and helps evaluate how distinguishable Mid and Bot players are.

### Baseline Model Performance  
<iframe src="assets/baseline-model.html" width="800" height="600" frameborder="0"></iframe>

> Baseline accuracy: ~56.9% 

---

## Final Model

We improved upon the baseline using feature transformations and a random forest classifier.  
This model captures non-linear interactions and improves prediction performance.

### Final Model Performance  
<iframe src="assets/final-model.html" width="800" height="600" frameborder="0"></iframe>

> Final accuracy: Higher than baseline, indicating clear statistical differences between Mid and Bot roles.

---

## Fairness Analysis

We tested whether the model's **prediction error rates** are unfairly distributed between roles.  
A permutation test was conducted to assess potential bias in precision across Mid and Bot predictions.

### Fairness Test  
<iframe src="assets/fairness-test.html" width="800" height="600" frameborder="0"></iframe>

```text
Observed stat (teamname): 0.0000 
p-value: 1.0000
```

> Conclusion: No significant difference in prediction fairness between Mid and Bot roles.

---
