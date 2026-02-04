# 🚀 Jira Issue Resolution Time Prediction

## 📌 Project Overview
This project focuses on predicting **issue resolution time** in Jira using historical issue data and machine learning techniques. By leveraging issue metadata, temporal patterns, priority levels, activity indicators, and workload-related features, the model estimates how long a Jira issue will take to be resolved.

This solution can help engineering and support teams improve **SLA planning, resource allocation, and issue prioritization**.

---

## 🎯 Problem Statement
Given historical Jira issue data, predict the **Resolution Time (in days)** for an issue before it gets resolved.

**Problem Type:** Supervised Machine Learning – Regression

---

## 📊 Dataset Information
- **Source:** Public Jira Dataset (Kaggle)
- **Total Records:** ~49,000 issues
- **Initial Features:** 491 columns
- **Final Features:** ~50 columns (after cleaning)
- **Target Variable:** `Resolution_Time` (in days)

The dataset includes:
- Issue metadata (type, priority, project details)
- Timestamps (created, updated, resolved)
- Text data (summary, description)
- Activity signals (comments, votes)
- Reporter-related workload information

---

## 🧠 Machine Learning Pipeline
```text
1. Load dataset and inspect schema
2. Drop columns with more than 95% missing values
3. Convert date columns to datetime format
4. Create target variable (Resolution_Time)
5. Perform feature engineering
6. Handle missing values
7. Remove identifier and leakage-prone columns
8. Perform Exploratory Data Analysis (EDA)
9. Split dataset into training and testing sets
10. Train baseline regression models
11. Evaluate model performance
12. Perform feature selection and hyperparameter tuning
13. Train ensemble models and compare results
14. Finalize model and analyze feature importance
```
---

## 🔧 Feature Engineering

The following feature groups were engineered to capture operational, temporal, and behavioral patterns that influence issue resolution time.

### Time-Based Features
- Created day of week
- Created hour
- Weekend indicator for issue creation

### Priority and Issue Attributes
- Encoded issue priority
- Encoded issue type
- Encoded project type
- Encoded severity (where available)

### Activity-Based Features
- Total number of comments on the issue
- Engagement duration between issue creation and last update

### Text-Based Features
- Length of issue summary
- Length of issue description

### Workload-Based Features
- Reporter issue count

These features convert raw Jira metadata into meaningful numerical signals suitable for machine learning models.

---

## 🤖 Models Implemented

The following regression models were trained and evaluated:

- Linear Regression (baseline)
- Ridge Regression (with hyperparameter tuning)
- Lasso Regression (with hyperparameter tuning)
- Gradient Boosting Regressor
- Random Forest Regressor
- XGBoost Regressor

---

## 📈 Evaluation Metrics

Model performance was evaluated using standard regression metrics:

- Mean Absolute Error (MAE)
- Mean Squared Error (MSE)
- Root Mean Squared Error (RMSE)
- R-squared score

All error metrics are reported in days.

---

## 🏆 Results Summary

| Model | MAE (Days) | RMSE (Days) | R² |
|------|------------|-------------|----|
| Linear / Ridge / Lasso | ~45 | ~64 | ~0.56 |
| Gradient Boosting | ~24 | ~38 | ~0.84 |
| Random Forest | Near 0 | Near 0 | ~1.00 |
| XGBoost | ~1 | ~1 | ~1.00 |

The near-perfect scores achieved by Random Forest and XGBoost models were caused by data leakage from strongly correlated temporal features. After identifying and mitigating leakage-prone features, Gradient Boosting emerged as the most reliable and generalizable model.

---

## ⚠️ Data Leakage Consideration

This project explicitly addresses data leakage, a common issue in time-dependent datasets such as Jira.

- Post-resolution and strongly correlated temporal features can unintentionally expose the target variable
- Tree-based models are especially sensitive to such leakage
- Leakage-prone features were identified and removed before final model selection

This ensures realistic model evaluation and trustworthy results.

---

## ✅ Final Model Selection

The Gradient Boosting Regressor was selected as the final model due to:

- Strong predictive performance
- Stability across training and testing datasets
- Better generalization under leakage-safe conditions

---

## 📌 Key Learnings

- Feature engineering has a greater impact than model selection
- High accuracy does not always indicate a correct or deployable model
- Preventing data leakage is critical in real-world machine learning projects
- Temporal datasets require careful validation strategies

---

## 🚀 Future Improvements

- Time-aware cross-validation strategies
- NLP-based embeddings for issue summary and description
- Assignee workload and queue modeling
- Real-time resolution time prediction pipeline
- SLA breach risk classification

---

## 🧰 Tech Stack

- Python
- Pandas
- NumPy
- Scikit-learn
- XGBoost
- Matplotlib
- Seaborn

---

## 📂 Project Structure

```text
├── jira_dataset.csv
├── Jira_Resolution_Time_Prediction.ipynb
└── README.md
```

---

👤 Author

Ashok T
Domain: Data Science and Machine Learning

---

⭐ Final Note

This project emphasizes correct machine learning practices, leakage prevention, and realistic model evaluation over inflated performance metrics, making it suitable for real-world applications and professional review.



