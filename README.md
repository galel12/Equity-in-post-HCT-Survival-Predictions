# Equity in Post-HCT Survival Predictions

<p align="center">
  <img src="assets/header.png" width="25%" style="border-radius:20px;" />
</p>

## Introduction

We are Gal Elharar and Roy Wolfer, computer science students passionate about data science and its real-world impact, particularly in healthcare. For our semester project, we wanted to work on something meaningful and socially relevant. The [Equity in post-HCT Survival Predictions](https://www.kaggle.com/competitions/equity-post-HCT-survival-predictions) Kaggle competition offered exactly that.

This challenge focuses on predicting survival probabilities for patients undergoing hematopoietic cell transplantation (HCT), with a strong emphasis on *equitable* outcomes across racial groups. It uses a **Stratified Concordance Index (C-index)** to evaluate how fair and accurate predictions are across subpopulations—highlighting the ethical side of AI in medicine.

---

## Problem Statement

Survival prediction in HCT is complicated by social, racial, and economic disparities. Most existing models don't account for these factors, leading to biased care recommendations.

The competition asks participants to develop models that are **both accurate and fair**, using synthetic clinical data designed to mimic real-world patterns while preserving privacy. Our goal was to build such models—maximizing predictive performance while minimizing inequality across race groups.

---

## Our Approach

### 🧪 Data Exploration & Preprocessing
- **Missing Values**: Handled as separate categories for categorical features.
- **Feature Importance & Correlation**: Used to reduce redundancy.
- **Feature Reduction**: Found marginal improvement after removing the least informative features.

### 🧠 Modeling
We built and compared multiple survival models:

#### 🔹 Basic Models
| Model | Type | Tool |
|---|---|---|
| Cox Proportional Hazards | Linear | `lifelines` |
| Cox PH | Gradient Boosted | `XGBoost`, `CatBoost` |
| Accelerated Failure Time (AFT) | Gradient Boosted | `XGBoost`, `CatBoost` |

#### 🔸 Target Transformation Models

To apply regression models with MSE loss, we transformed the time-to-event targets into more learnable forms. These transformations emphasized ranking patients by event timing.

We tested five variants:
- **Survival Probability**: Maps time to Kaplan–Meier survival estimates, assigning lower probabilities to earlier events.
- **Partial Hazard**: Uses Cox model outputs to guide ranking.
- **Separate**: Assigns distinct values to events vs. non-events.
- **Rank Log**: Applies log-rank scaling for smoother separation.
- **Quantile (Best)**: Uniformly spreads events from 0–1, sets non-events to a fixed negative value.

> The **Quantile transformation** yielded our highest c-index, slightly outperforming the Cox baseline while being simple and efficient.

---

## Evaluation Strategy

We evaluated models using:
- **Stratified C-index** (main metric)
- **t-AUC** (time-dependent AUC)
- **Fairness across race groups**

> Key decision: We prioritized *c-index* over *t-AUC* in final model selection, in alignment with the competition’s goal.

---

## Results

### 🏆 Best Models
- **Cox PH with XGBoost**: Strongest baseline and final model (after transformation).
- **Transform Quantile + XGBoost (MSE)**: Slightly improved C-index and fairness after tuning.

### 📈 Hyperparameter Tuning
Using **Optuna**, we improved:
- Overall C-index.
- Group consistency (lower standard deviation).
- Group mean, showing better generalization.

### ⚖️ Fairness Findings
- Asian patients had consistently high prediction scores.
- White patients had the lowest scores.
- Our model aimed to **balance** performance across all six racial groups, accepting slight tradeoffs in global metrics for equity.

---

## Reflections

This project wasn’t just about leaderboard scores—it was about learning how to build **responsible, fair, and interpretable models**. We learned that behind every data point is a patient, and that equity in modeling is not a “bonus” feature, but a core requirement in healthcare AI.

---

## Working Environment

We tested several platforms before settling on **VSCode + Live Share**, with a Python virtual environment. This gave us real-time collaboration, full intellisense, and smooth workflow compared to Colab or Deepnote.

---

## Acknowledgements

Thanks to Kaggle for the platform and synthetic dataset, and to our university instructors for their support throughout the project.

---

## References

- [Kaplan–Meier Estimator](https://en.wikipedia.org/wiki/Kaplan%E2%80%93Meier_estimator)
- [Cox Proportional Hazards Model](https://en.wikipedia.org/wiki/Proportional_hazards_model)
- [Accelerated Failure Time Model](https://en.wikipedia.org/wiki/Accelerated_failure_time_model)
- [Optuna: Hyperparameter Optimization](https://optuna.org/)