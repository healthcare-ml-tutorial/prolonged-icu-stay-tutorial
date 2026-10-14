# Predicting Prolonged ICU Stay (>7 Days) Using MIMIC-III

## Project Overview

This repository contains a reproducible machine-learning tutorial that predicts whether an adult patient's ICU stay will exceed 7 days using demographic, admission, and first-24-hour laboratory information from MIMIC-III.

The project was developed for an educational self-learning tutorial in healthcare machine learning.

## Research Question

**Can information available during the first 24 hours of an ICU stay predict whether the ICU stay will last more than 7 days?**

## Outcome

- `OUTPUT_LABEL = 0`: ICU length of stay ≤ 7 days
- `OUTPUT_LABEL = 1`: ICU length of stay > 7 days

## Data Source

This project uses MIMIC-III data accessed through Google BigQuery.

MIMIC-III is a credentialed dataset. Raw MIMIC data are **not included in this repository**. Anyone wishing to reproduce the analysis must obtain their own authorized MIMIC-III access through PhysioNet and configure their own Google Cloud project.

## Tables Used

The analysis uses the following MIMIC-III tables:

- `ICUSTAYS`
- `PATIENTS`
- `ADMISSIONS`
- `LABEVENTS`

## Features

The model uses:

- Age
- Gender
- Admission type
- First-24-hour laboratory measurements

Laboratory variables include:

- Creatinine
- Blood urea nitrogen (BUN)
- Sodium
- Potassium
- Glucose
- Hemoglobin
- White blood cell count
- Platelet count

For repeated laboratory tests in the first 24 hours, minimum, maximum, and mean values are calculated.

## Cohort Definition

The analysis:

1. Selects the first ICU stay for each patient.
2. Restricts the cohort to adults aged 18 years or older.
3. Excludes records with missing ICU length of stay.
4. Defines prolonged ICU stay as more than 7 days.
5. Uses only information available within the first 24 hours of ICU admission.

## Machine-Learning Workflow

The tutorial demonstrates:

1. BigQuery extraction and feature engineering
2. Train / validation / test splitting
3. Training-set-only under-sampling for class imbalance
4. Missing-value imputation
5. One-hot encoding of categorical variables
6. Standardization with `StandardScaler`
7. Logistic Regression
8. Hyperparameter tuning using validation ROC-AUC
9. Final evaluation on an untouched test set
10. Random Forest comparison
11. Model interpretation using Logistic Regression coefficients

## Models

### Logistic Regression

Logistic Regression is used as the baseline model because it is relatively interpretable and is commonly used for binary prediction tasks.

### Random Forest

Random Forest is included as a nonlinear comparison model that can capture interactions and nonlinear relationships between predictors.

## Evaluation Metrics

The project reports:

- ROC-AUC
- PR-AUC
- Accuracy
- Recall
- Precision
- Specificity
- Confusion matrix

Because prolonged ICU stays are less common than shorter ICU stays, PR-AUC and recall are particularly useful alongside ROC-AUC.

## Reproducibility

The analysis was developed and executed locally in **VS Code / Jupyter**.

MIMIC-III was accessed through **Google BigQuery** using Google Cloud Application Default Credentials.

Before running the notebook, install and configure the Google Cloud CLI, then authenticate:

```bash
gcloud init
gcloud auth application-default login
```

In the notebook, replace:

```python
PROJECT_ID = "YOUR_GCP_PROJECT_ID"
```

with your own Google Cloud billing project ID.

The BigQuery extraction requires the user's own credentialed MIMIC-III access.

## Repository Contents

```text
.
├── Prolonged_ICU_Stay_Tutorial.ipynb
├── README.md
├── requirements.txt
└── .gitignore
```

## Privacy and Data-Use Requirements

This repository intentionally does **not** contain:

- raw MIMIC-III CSV files
- extracted patient-level datasets
- Google Cloud credentials
- service-account keys
- personal identifiers

Users must comply with the MIMIC-III data-use agreement and all applicable institutional requirements.

## Limitations

- This is a retrospective analysis using a single-center dataset.
- The 7-day threshold is a project definition and is not a universal clinical standard.
- Only a selected set of early clinical variables is used.
- Missing laboratory measurements may reflect clinical practice patterns.
- Predictive associations do not establish causality.
- External validation would be required before clinical deployment.

## Educational Use

This repository is intended for educational purposes only. The models are not intended for direct clinical use.
