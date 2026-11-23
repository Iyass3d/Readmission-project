# Predicting 30-Day Hospital Readmission in Diabetic Patients

**Course:** 465CIS – Healthcare Informatics (Lab Project)
**Dataset:** Diabetes 130-US Hospitals (1999–2008), 101,766 hospital stays

---

## What is this project?

Hospitals are penalised when patients come back within 30 days of leaving.
This project builds machine-learning models that answer one question:

> **Will this diabetic patient be readmitted within 30 days of discharge?**

The prediction is made **at discharge**, using only information that is already known at that moment.
The goal is to help the care team decide which patients need extra follow-up.

---

## Files in this repository

| File | What it is |
|---|---|
| `Lab_project.ipynb` | The full notebook: data cleaning, analysis, models and results (Parts A–F) |
| `diabetic_data.csv` | The dataset |
| `HIS_report.pdf` | The written project report |
| `README.md` | This file |

---

## How to run the notebook

1. Install Python 3.11 (Anaconda is fine) and these libraries:

   ```bash
   pip install pandas numpy matplotlib seaborn scikit-learn
   ```

   *(Needs scikit-learn 1.2 or newer and seaborn 0.13 or newer.)*

2. Keep `diabetic_data.csv` in the **same folder** as the notebook.
3. Open `Lab_project.ipynb` and choose **Run All**.

The results are reproducible: a fixed random seed (`random_state = 42`) is used everywhere.

---

## What was done (Parts A–F)

| Part | Topic | In one sentence |
|---|---|---|
| **A** | Problem framing | Predict readmission within 30 days, at discharge, using only variables known at that time |
| **B** | Data quality and EDA | Cleaned the data, studied missing values, and checked how each variable relates to readmission |
| **C** | Feature engineering | Grouped diagnosis codes and rare categories, and created new features (e.g. total prior visits) |
| **D** | Modelling | Trained and tuned four models: Logistic Regression, Random Forest, SVM and KNN |
| **E** | Interpretation and fairness | Found the most important features and compared model errors across gender, age and race |
| **F** | Summary for the hospital | A one-page plain-language summary for the Chief Medical Officer |

---

## Key choices (why the results can be trusted)

- **Target:** a patient is a "positive" if readmitted in **under 30 days** (about 11% of patients, so the data is imbalanced).
- **Patients who died or went to hospice were removed**, because they cannot be readmitted. After cleaning, 99,340 stays remain.
- **Patient-level split:** the same patient never appears in both the training and test sets, which prevents data leakage.
- **Accuracy alone is misleading** (predicting "nobody comes back" is already about 89% accurate), so we also report precision, recall, F1 and ROC-AUC.
- Class imbalance is handled with balanced class weights.

---

## Main results

Test-set ROC-AUC (higher is better, 0.5 = random guessing):

| Model | ROC-AUC |
|---|---|
| **Random Forest** | **0.678** |
| Logistic Regression | 0.668 |
| SVM (linear) | 0.667 |
| KNN | 0.654 |

**What we learned**

- Readmission is **hard to predict**: the best model is only modestly better than chance (ROC-AUC about 0.68).
- The strongest predictors are **where the patient goes after discharge**, and **how many times they were hospitalised before**.
- Blood-sugar test results (HbA1c) and medication changes added almost nothing.
- Used as a ranking tool, the model is still useful: following up the **top 20% highest-risk patients** reaches about **40% of all readmissions**.
- **Fairness:** the model catches fewer readmissions in men than in women (recall 0.39 vs 0.48). Using a gender-specific threshold reduces this gap. Results for small race groups are unreliable because of small sample sizes.

---

## Contributions

| Member | Role | Parts |
|---|---|---|
| Iyas Alshahrani | Problem framing, data quality and feature engineering | A, B, C |
| [Name 2] | Modelling, interpretation and hospital summary | D, E, F |
| Both | Report writing, notebook review and reproducibility check | All |

Both members reviewed the whole project and can explain any part of it.

---

## Limitations

- The data covers only **diabetic patients** from 1999–2008, so the model should not be used for other patients or for current practice without re-testing.
- The model is a **support tool, not a decision maker**. A clinician must always make the final decision.
