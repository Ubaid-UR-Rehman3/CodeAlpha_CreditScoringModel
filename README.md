# Credit Scoring Model

## 1. Project Overview

This project was completed as part of the **CodeAlpha Machine Learning Internship — Task 1: Credit Scoring Model**.

The objective is to build and compare machine learning classification models that predict an individual's creditworthiness using historical financial and personal information.

The target variable is `Creditworthiness`:

- `1` = Good creditworthiness
- `0` = Bad creditworthiness

The project compares Logistic Regression, Decision Tree, and Random Forest classifiers using accuracy, precision, recall, F1-score, and ROC-AUC.

## 2. Dataset Overview

- **Dataset:** Credit Scoring Dataset — Creditworthiness Prediction
- **Author:** Muhammad Junaid
- **File:** `credit_dataset.csv`
- **Records:** 12,000
- **Columns:** 14
- **Dataset type:** Synthetic structured data

### Features

**Numerical features**
- Age
- Income
- Debt
- Credit_Score
- Loan_Amount
- Loan_Term
- Num_Credit_Cards

**Categorical features**
- Gender
- Education
- Payment_History
- Employment_Status
- Residence_Type
- Marital_Status

**Target**
- Creditworthiness

### Data quality

- Missing values: 0
- Duplicate rows: 0
- Good-creditworthiness records (class 1): 8,431 (70.26%)
- Bad-creditworthiness records (class 0): 3,569 (29.74%)

The dataset is imbalanced, with more records belonging to class 1.

**Important limitation:** The dataset is synthetic. Its results should not be interpreted as evidence of real-world credit risk or loan default prediction performance.

## 3. Exploratory Data Analysis

Exploratory Data Analysis included:

- Dataset dimensions and data types
- Missing-value and duplicate checks
- Target-class distribution
- Numerical feature distributions
- Categorical feature distributions
- Numerical feature boxplots by target class
- Correlation analysis
- Mutual information analysis

The numerical features showed substantial overlap between the two target classes. Their individual correlations with the target were very small, and mutual information scores were also generally low.

These observations suggested that the dataset might contain weak predictive signals.

## 4. Data Preprocessing

The dataset was divided into training and testing sets:

- Training set: 80% (9,600 records)
- Testing set: 20% (2,400 records)
- Random state: 42
- Stratification: enabled

Preprocessing was implemented using a Scikit-learn `ColumnTransformer`.

### Numerical features
- StandardScaler was used for scaling.

### Categorical features
- OneHotEncoder was used to convert categories into numerical representations.
- `handle_unknown="ignore"` was used to handle previously unseen categories.

The preprocessing transformer was fitted only on the training data. The fitted transformer was then applied to the test data to prevent data leakage.

Processed feature dimensions:
- Training: 9,600 × 25
- Testing: 2,400 × 25

No unverified derived financial ratios were added.

## 5. Models Implemented

Three classification models were trained:

1. Logistic Regression
2. Decision Tree Classifier
3. Random Forest Classifier

A Dummy Classifier using the most frequent class was also evaluated as a baseline.

## 6. Evaluation Metrics

Because the target classes are imbalanced, accuracy alone is not sufficient.

The following metrics were considered:

- **Precision:** Of the records predicted as Bad, how many were actually Bad?
- **Recall:** Of all actual Bad records, how many did the model identify?
- **F1-score:** Harmonic mean of precision and recall.
- **ROC-AUC:** Measures how well a model ranks the two classes across classification thresholds.

For the precision, recall, and F1-score comparison below, class `0` (Bad creditworthiness) was treated as the positive class. ROC-AUC was also calculated for class 0 using its predicted probability.

## 7. Model Results

| Model | Accuracy | Class 0 Precision | Class 0 Recall | Class 0 F1-score | Class 0 ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Logistic Regression | 0.7025 | 0.0000 | 0.0000 | 0.0000 | 0.5198 |
| Decision Tree | 0.5742 | 0.3031 | 0.3319 | 0.3168 | 0.5043 |
| Random Forest | 0.7004 | 0.2727 | 0.0042 | 0.0083 | 0.4810 |

### Baseline

The Dummy Classifier achieved 0.7025 accuracy by always predicting the majority class.

However, it failed to identify any class 0 records. This demonstrates why accuracy alone can be misleading for an imbalanced classification problem.

### Results interpretation

- Logistic Regression predicted the majority class at its default threshold and did not identify Bad-credit records.
- Decision Tree achieved the highest class 0 F1-score among the three models at the default threshold, but its recall was only 0.3319.
- Random Forest identified very few class 0 records at its default threshold.
- All three ROC-AUC scores were close to 0.50, indicating weak class discrimination.

No model demonstrated strong predictive performance on this dataset.

## 8. Classification Threshold Exploration

The Logistic Regression model's predicted probabilities for class 0 ranged from approximately 0.2435 to 0.3485.

An exploratory threshold analysis produced the following class 0 results:

| Threshold | Precision | Recall | F1-score |
|---:|---:|---:|---:|
| 0.25 | 0.2977 | 1.0000 | 0.4589 |
| 0.28 | 0.3018 | 0.8501 | 0.4455 |
| 0.30 | 0.3091 | 0.4412 | 0.3635 |
| 0.32 | 0.3019 | 0.0896 | 0.1382 |
| 0.35 | 0.0000 | 0.0000 | 0.0000 |
| 0.40 | 0.0000 | 0.0000 | 0.0000 |

Lowering the threshold increased recall but also resulted in more false-positive predictions.

**Evaluation limitation:** These thresholds were explored using the test set. Therefore, this analysis is exploratory and should not be treated as an unbiased final test evaluation of a selected threshold. A separate validation set should be used for threshold selection in a future experiment.

## 9. Conclusion

This project implemented and compared three supervised classification algorithms for predicting creditworthiness.

The Decision Tree achieved the highest class 0 F1-score at the default threshold, but its ROC-AUC was only 0.5043 and it missed 477 of the 714 actual class 0 records in the test set.

The results suggest that the supplied synthetic dataset contains limited predictive information for the tested models and settings.

Further work could investigate alternative datasets with stronger, documented relationships between financial history and credit outcomes, improved feature engineering supported by clear feature definitions, and validation-based model and threshold selection.

This project is an educational machine learning experiment, not a production-ready credit assessment system.

## 10. Tools and Libraries

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Jupyter Notebook

## 11. How to Run

1. Clone or download this repository.
2. Open the project folder in VS Code.
3. Create and activate a virtual environment on Windows:

   ```powershell
   python -m venv .venv
   .venv\Scripts\activate
   ```

4. Install the required libraries:

   ```powershell
   pip install pandas numpy matplotlib seaborn scikit-learn jupyter
   ```

5. Open the notebook in VS Code or Jupyter.
6. Run the notebook cells in order.

Make sure the dataset is available at the path expected by the notebook.

## 12. Repository Structure

```text
CodeAlpha_CreditScoringModel/
│
├── data/
│   └── credit_dataset.csv
│
├── notebooks/
│   └── credit_scoring.ipynb
│
├── README.md
│
└── .gitignore
```

## 13. Internship Task

**CodeAlpha Machine Learning Internship — Task 1: Credit Scoring Model**