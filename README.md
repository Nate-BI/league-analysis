
# League Role Analysis
This is a DSC 80 project website.
# 🎮 Who Carries More in League of Legends?

**Name:** Haoshuo Bi  
**UCSD DSC 80 Final Project – Spring 2025**

---

## Introduction

We analyze whether **Mid** or **Bot** players contribute more to their teams in competitive League of Legends matches.  
Using data from the 2022 season, we explore statistical performance metrics, conduct hypothesis testing, and train predictive models to understand carry potential across roles.
This dataset includes over **120,000 rows** from more than **10,000 matches**, with each match containing **10 player-level** and **2 team-level rows**.  
For our analysis, we focus on the following relevant columns:

- `position`: the player’s role (e.g., top, jng, mid, bot, sup)  
- `kills`, `deaths`, `assists`: key stats used to evaluate performance  
- `dpm`: damage per minute, a strong indicator of contribution  
- `teamname`, `side`: contextual match metadata

---

## Data Cleaning and Exploratory Data Analysis

We removed team-level rows, filtered for player roles (top, jng, mid, bot, sup), and cleaned key columns (`kills`, `deaths`, `assists`, `dpm`).  
We also removed rows with missing values in those key performance columns.
Each match in the dataset has **12 rows** — **10 for individual players** and **2 for team-level summaries**.  
Since our question focuses on individual player performance, we excluded team-level rows by selecting only standard roles.

### Cleaned DataFrame Preview

The following image shows the head of our cleaned DataFrame. It includes only player-level rows and keeps only the relevant columns: `gameid`, `playername`, `position`, `teamname`, `side`, `kills`, `deaths`, `assists`, and `dpm`.

<img src="assets/df-players-head.png.jpg" alt="Head of Cleaned DataFrame" style="width:800px;">

### Position Distribution  
<iframe src="assets/position-distribution.html" width="800" height="600" frameborder="0"></iframe>
Most players are evenly distributed across the five roles.

### DPM Distribution  
<iframe src="assets/dpm-distribution.html" width="800" height="600" frameborder="0"></iframe>
Most players have a DPM between 300 and 600, with a few extreme outliers over 1000.

### DPM by Position  
<iframe src="assets/dpm-by-position.html" width="800" height="600" frameborder="0"></iframe>
The boxplot shows that Mid and Bot players generally have higher DPM than other roles, with Mid laners having the highest median and more upper-range outliers. 
This supports the hypothesis that Mid or Bot may carry more often.

### KDA by Position  
<iframe src="assets/kda-by-position.html" width="800" height="600" frameborder="0"></iframe>
According to the KDA boxplot, Bot and Mid roles again show higher median and upper quartile values, suggesting that they often contribute more significantly to team kills and assists with fewer deaths.

---

## Assessment of Missingness

We simulated 30% missing values (MCAR) in the `dpm` column to evaluate whether missingness depends on player role or team.  
Permutation tests were performed to determine if observed differences are statistically significant.

### NMAR Reasoning

We suspect that the `dpm` (damage per minute) column may be **Not Missing At Random (NMAR)**.  
If a player performs extremely poorly—such as going AFK or disconnecting—their damage may not be recorded at all.  
In this case, the value is missing because of what it would have been (i.e., low or zero), which fits the definition of NMAR.  
To verify this, we would need more information about how damage statistics are recorded, such as developer logs or documentation.

### DPM Missingness by Position  
<iframe src="assets/dpm-missing-by-position.html" width="800" height="600" frameborder="0"></iframe>

```text
Observed stat (position): 0.0022  
p-value: 0.9890
```

> Conclusion: There is no significant evidence that missingness depends on position.

Although we initially hypothesized that `dpm` missingness might depend on player `position`, the high p-value (0.9890) suggests the missingness is likely independent.  
We did not identify a column that significantly predicts missingness under the MCAR simulation.

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

> Conclusion: The result is statistically significant (p < 0.05), suggesting **Bot players tend to have higher KDA than Mid players on average**.  
> However, since this is an observational study and not a randomized controlled experiment, this result does not imply causation.

---

## Framing a Prediction Problem

We aim to predict whether a player is **Mid** or **Bot** based on their performance stats.  
This reframes the role comparison as a binary classification task using numerical features: `kills`, `deaths`, `assists`, and `dpm`.
We use **accuracy** as our evaluation metric, since the two classes (Mid and Bot) are roughly balanced and the cost of misclassification is symmetric.

---

## Baseline Model

We trained a logistic regression model using scaled numerical features.  
This model serves as a baseline and helps evaluate how distinguishable Mid and Bot players are.

### Baseline Model Performance  
<iframe src="assets/baseline-model.html" width="800" height="600" frameborder="0"></iframe>

> Baseline accuracy: ~56.9% 
All four features used are **quantitative**; no ordinal or nominal features were included, so no encoding was necessary.  
As a simple baseline model without tuning or feature selection, this performance (~56.9%) is a reasonable starting point for comparison.

---

## Final Model

We improved upon the baseline model by applying feature transformations and using a Random Forest classifier with hyperparameter tuning.  
This model captures potential non-linear interactions between performance metrics and player role.

We used a `ColumnTransformer` to apply:
- `StandardScaler` to `kills` and `deaths`, as these are approximately normally distributed;
- `QuantileTransformer` to `assists`, due to its right-skewed distribution;
- `dpm` was used without transformation.

We performed grid search over two hyperparameters:
- `n_estimators`: number of trees (50, 100)
- `max_depth`: maximum tree depth (5, 10, None)

The best parameters were:  
`max_depth = 10`, `n_estimators = 100`

### Final Model Performance  
<iframe src="assets/final-model.html" width="800" height="600" frameborder="0"></iframe>

> Final accuracy: **56.76%**, slightly lower than the baseline model (56.96%).  
> Although the final model did not outperform the baseline, it demonstrates careful feature engineering and the ability to capture more complex relationships between features and role classification.

---

## Fairness Analysis

We tested whether our final model performs equally well for players of different roles.

**Group X:** Mid laners  
**Group Y:** Bot laners  
**Evaluation Metric:** Precision  

We conducted a permutation test to assess potential bias in model precision across roles.

**Null Hypothesis (H₀):** The model is fair; the precision for Mid and Bot players is the same.  
**Alternative Hypothesis (H₁):** The model is unfair; the precision differs significantly between the two roles.

### Fairness Test  
<iframe src="assets/fairness-test.html" width="800" height="600" frameborder="0"></iframe>

**Observed Precision Difference:** 0.0000  
**p-value:** 1.0000

> **Conclusion:** Since the p-value is greater than 0.05, we fail to reject the null hypothesis.  
> This suggests that the model performs **equally fairly** for both Mid and Bot players.

---
