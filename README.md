
# Jira Issue Resolution Time Prediction

---


## 📌 Project Overview

This project predicts the **resolution time (in hours)** of Jira issues using historical issue data.
It demonstrates a **complete applied machine learning workflow**, focusing on **correct problem framing, data leakage prevention, model comparison, and explainability** rather than metric chasing.

The objective is to build a **defensible and production-realistic regression model** that helps teams anticipate long-running issues and improve planning.

---

## 🎯 Problem Statement

Given historical Jira issue records, predict how long an issue will take to be resolved.

### Why this matters

* Better **resource & sprint planning**
* Early identification of **complex / long-running issues**
* Improved **SLA estimation**

---

## 🧠 Machine Learning Formulation

* **Task Type:** Regression
* **Target Variable:** `resolution_time_hours`
* **Why not classification or accuracy?**
  Resolution time is a continuous variable; using classification or “accuracy” would be misleading.

---

## 📊 Dataset

* Source: Jira issue export (CSV)
* Includes:

  * Issue metadata (type, priority, project details)
  * Textual fields (summary, description, comments)
  * Lifecycle timestamps (created, resolved)

### Key challenges

* High dimensionality (text + metadata)
* Sparse features (TF-IDF, one-hot encoding)
* Strongly skewed target distribution (hours → years)

---

## 🔧 Data Cleaning & Feature Engineering

Key steps:

* Removed columns with >95% missing values
* Parsed and validated datetime fields
* Computed resolution time in hours
* Removed invalid / negative durations
* Merged multiple comment fields into a single text feature
* Engineered temporal features:

  * Creation hour
  * Day of week
  * Weekend indicator

### Feature representation

* **Text:** TF-IDF (summary, description, comments)
* **Categorical:** One-hot encoded metadata
* **Numeric:** Time-based features

---

## 🔀 Train–Test Split

A **time-based split** was used instead of random sampling:

* **Training set:** older issues
* **Test set:** newer issues

This simulates real-world deployment and avoids **data leakage**.

---

## 🧪 Models Evaluated

### 1️⃣ Ridge Regression (Linear Baseline)

* Used as a baseline
* Performed poorly due to:

  * Non-linear relationships
  * Heavy target skew
* Established problem difficulty

---

### 2️⃣ LightGBM Regressor (Raw Target)

* First tree-based model tested
* Large improvement over linear baseline
* Still affected by extreme skew and variance

---

### 3️⃣ LightGBM Regressor (Log-Transformed Target) — **Final Model**

* Trained on `log(resolution_time_hours)`
* Best overall performance
* Reduced impact of extreme long-running issues
* Stable and reliable predictions

---

### 4️⃣ Random Forest Regressor (Log-Transformed Target)

* Used as a comparative tree-based model
* Performed better than linear baseline
* Inferior to LightGBM due to lack of boosting

---

## 📈 Evaluation Metrics

Since this is a regression problem, **accuracy is not applicable**.

Metrics used:

* **MAE (Mean Absolute Error)**
* **RMSE (Root Mean Squared Error)**

---

## 📊 Model Performance Comparison

| Model            | Target  | MAE (hours) | RMSE (hours) |
| ---------------- | ------- | ----------- | ------------ |
| Ridge Regression | Raw     | ~10,723     | ~12,367      |
| LightGBM         | Raw     | ~4,251      | ~4,742       |
| **LightGBM**     | **Log** | **~3,534**  | **~3,970**   |
| Random Forest    | Log     | ~4,817      | ~5,365       |

### Key observation

Tree-based models dramatically outperform linear regression, and **applying a log transformation further stabilizes variance**, allowing LightGBM to achieve the lowest error.

---

## 🧠 Model Selection Rationale

LightGBM with a log-transformed target was selected because:

* Linear models violated core assumptions
* Resolution time exhibited extreme skew
* Tree-based models captured non-linear feature interactions
* Gradient boosting outperformed bagging (Random Forest)
* Empirical evaluation showed the lowest MAE and RMSE

Model choice was **data-driven and evidence-based**, not heuristic.

---

## 🔍 Model Interpretability

Interpretability was addressed using **LightGBM feature importance**.

### Key insights

* Textual complexity (summary, description, comments) is the strongest driver
* Project-specific context significantly influences resolution time
* Temporal features provide secondary signals

SHAP was explored but intentionally excluded from the final version due to:

* Extremely high-dimensional sparse feature space
* Limited interpretability for TF-IDF feature indices

This reflects **pragmatic, real-world ML decision-making**.

---

## ✅ Final Conclusion

Predicting Jira issue resolution time is a challenging non-linear regression problem.
Linear models fail to capture the complexity of the data, while tree-based models perform significantly better.

By applying a log transformation and using LightGBM, the final model achieves the **lowest error with stable behavior**, making it the most suitable choice for this task.

---

## 🛠️ Tech Stack

* Python
* Pandas, NumPy
* Scikit-learn
* LightGBM
* Matplotlib

---

## 📂 Repository Structure

```
├── GFG_FINAL.csv
├── Resolution_time_pred_model.ipynb
├── README.md
```

---

## 🚀 Future Improvements (Optional)

* SLA-based evaluation metrics
* Grouped text feature interpretation
* Integration with Jira APIs for real-time inference

---

## 📌 Project Status

✅ **Completed**
This repository represents a **finished, portfolio-ready applied machine learning project**.


---
