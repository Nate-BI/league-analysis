# League Role Analysis
This is a DSC 80 project website.
# 🎮 Who Carries More in League of Legends?

**Name:** Haoshuo Bi  
**UCSD DSC 80 Final Project – Spring 2025**

---

## Introduction

We analyze whether **Mid** or **Bot** players contribute more to their teams in competitive League of Legends matches.

---

## Data Cleaning and Exploratory Data Analysis

We removed team-level rows, filtered for player roles (top, jng, mid, bot, sup), and cleaned key columns (`kills`, `deaths`, `assists`, `dpm`).

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

We examined missingness in the `dpm` column and found that it is **likely not dependent** on team or position.

### DPM Missingness by Position  
<iframe src="assets/dpm-missing-by-position.html" width="800" height="600" frameborder="0"></iframe>

---

## Hypothesis Testing

We tested whether Mid and Bot players differ significantly in KDA using a permutation test.

**Null Hypothesis:** Mid and Bot players have the same KDA distribution.  
**Alternative Hypothesis:** Their KDA distributions are different.

### KDA Hypothesis Test  
<iframe src="assets/kda-hypothesis-test.html" width="800" height="600" frameborder="0"></iframe>

---

## Framing a Prediction Problem

We aim to predict whether a player is **Mid** or **Bot** based on their performance stats.

---

## Baseline Model

We trained a logistic regression model using scaled numerical features.

### Baseline Model Performance  
<iframe src="assets/baseline-model.html" width="800" height="600" frameborder="0"></iframe>

---

## Final Model

We improved upon the baseline with feature transformations and a random forest classifier.

### Final Model Performance  
<iframe src="assets/final-model.html" width="800" height="600" frameborder="0"></iframe>

---

## Fairness Analysis

We tested whether the model's **precision** differs between Mid and Bot roles.  
No significant difference was found.

### Fairness Test  
<iframe src="assets/fairness-test.html" width="800" height="600" frameborder="0"></iframe>
