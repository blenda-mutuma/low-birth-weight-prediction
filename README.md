# Predicting Low Infant Birth Weight

**Python · pandas · seaborn · scikit-learn** | BAN 668, Quinnipiac University · Fall 2024

## The question

Which maternal factors are linked to low birth weight (under 2.5 kg), and can a model reliably flag at-risk pregnancies? In a screening problem, a missed at-risk case costs far more than a false alarm, so the analysis focuses on **recall**, not just accuracy.

## Data

189 births recorded at Baystate Medical Center, Springfield, MA (1986). Variables include mother's age, weight at last menstrual period, race, smoking status, hypertension history, uterine irritability, premature labor history, and first-trimester physician visits.

The notebook expects `low_birth_weight_project.csv` in the same folder. This is the widely used public Baystate dataset (also available as `birthwt` in R's `MASS` package).

## What I did

1. **Cleaned the data.** Renamed cryptic columns (`lwt` → `last_menstrual_weight`), removed 6 duplicate records, dropped variables that leaked the target, and confirmed there were no missing values.
2. **Engineered a feature.** Grouped mothers into Teen, Young Adult, and Mature Adult age bands.
3. **Explored the data.** Ran univariate and bivariate analysis of every predictor against the outcome.
4. **Modeled.** One-hot encoded and scaled the features, then compared logistic regression, a decision tree, a tuned decision tree (GridSearchCV with class weights), and k-nearest neighbors.
5. **Tuned the decision threshold.** Used the precision–recall curve to move logistic regression's cutoff from 0.50 to 0.34.

## Results

| Model | Accuracy | F1 | Recall |
|---|---|---|---|
| **Logistic regression (threshold 0.34)** | **76%** | **0.64** | **0.67** |
| Logistic regression (default 0.50) | 76% | 0.57 | 0.50 |
| Tuned decision tree | 49% | 0.46 | 0.67 |
| Decision tree | 54% | 0.32 | 0.33 |
| K-nearest neighbors | 68% | 0.14 | 0.08 |

Moving the threshold raised recall from 0.50 to 0.67 with no loss in accuracy. KNN shows why accuracy alone misleads on imbalanced data: 68% accuracy, but it caught only 8% of low-birth-weight cases.

**Key findings**
- **Smoking:** 42.9% of mothers who smoked had a low-birth-weight baby, compared with 25.7% of non-smokers.
- **Strongest predictors:** mother's weight at last menstrual period, followed by hypertension history and smoking.

**Limitations:** this is a small, imbalanced sample (183 records after cleaning, about 32% positive), so the results are indicative, not clinical.

## Files

| File | Contents |
|---|---|
| `low_birth_weight_analysis.ipynb` | Full analysis, from cleaning to model comparison |
| `docs/final_report.pdf` | Individual written report |
| `docs/team_presentation.pdf` | Class presentation, delivered with Margaret Gachau, Cameron Morosky, and Marcelline Ouma |
