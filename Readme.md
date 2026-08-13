# Optimizing Call Center Efficiency: Predictive Analysis of Sri Lanka GIC Call Records

A machine learning project exploring whether operational factors of the **Sri Lanka Government Information Center (GIC)** can be used to predict average caller waiting time.

The project was developed as part of **Machine Learning** and focuses on regression modelling using historical GIC call-center data.

---

## Project Overview

The Sri Lanka Government Information Center (GIC) handles citizen inquiries through the **1919 / 0114191919** contact service.

Call-center performance can be affected by several operational factors, including:

* Call volume
* Number of operators
* Shift allocation
* Staffing hours
* Knowledge Base (KB) coverage

The objective of this project was to investigate whether these operational characteristics could be used to predict the **average waiting time in the call queue**.

The analysis follows a machine learning workflow consisting of:

1. Data preprocessing
2. Feature engineering
3. Exploratory data analysis
4. Regression model training
5. Hyperparameter tuning
6. Model evaluation
7. Feature importance analysis
8. SHAP-based model interpretation

---

## Objective

The main research question explored in this project was:

> **Can future call waiting times be predicted from operational factors such as staffing, call volume, shift distribution, and Knowledge Base coverage?**

The target variable is:

**Average Waiting Time in Queue (seconds)**

---

## Dataset

The project uses monthly call-center records from the **Sri Lanka Government Information Center**.

The original dataset covers the period:

**2016–2021**

The dataset contains operational information related to:

* Total number of calls
* Total number of queries
* Knowledge Base queries
* Staffing hours
* Weekday operator shifts
* Weekend operator shifts
* Average waiting time in queue

The dataset was obtained from Sri Lanka's open government data platform.

---

## Feature Engineering

Several additional features were derived from the original variables.

### KB Coverage Ratio

Measures the proportion of queries addressed through the Knowledge Base:

```text
KB_coverage_ratio =
    KB queries / Total queries
```

### Staffed Ratio

Represents the proportion of the week during which the call center was fully staffed:

```text
Staffed_ratio =
    Fully staffed hours per week / 168
```

### Total Weekday Operators

The number of operators across the four weekday shifts:

```text
Total_ops_weekday =
    Shift 1 + Shift 2 + Shift 3 + Shift 4
```

### Total Weekend Operators

The number of operators across the four weekend shifts.

### Calls per Operator

An approximate measure of call workload per available operator:

```text
Call_per_operator =
    Total calls / Total operators
```

The original shift-level operator columns were removed after the aggregate operator features were created.

The target variable, originally represented as a time value such as `00:01:50`, was converted into seconds for regression modelling.

---

## Exploratory Data Analysis

The project includes exploratory analysis of:

* Feature distributions
* Target distribution
* Correlation between operational variables
* Relationships between staffing, workload, KB coverage, and waiting time

An outlier filtering step was also applied to the target variable using the 99th percentile.

---

## Machine Learning Models

Four regression approaches were evaluated:

### 1. Bayesian Ridge Regression

A regularized linear regression model used as a relatively simple baseline.

### 2. Support Vector Regression (SVR)

An RBF-kernel SVR was used to capture nonlinear relationships between the operational variables and waiting time.

Hyperparameters including:

* `C`
* `epsilon`
* `gamma`

were tuned using `RandomizedSearchCV`.

### 3. LightGBM

A tree-based gradient boosting model was tested with constrained parameters because of the relatively small dataset size.

### 4. Gaussian Process Regression

A Gaussian Process model using an RBF kernel combined with a White Kernel was also evaluated.

---

## Evaluation Metrics

The models were evaluated using:

* **RMSE** - Root Mean Squared Error
* **MAE** - Mean Absolute Error
* **R²** - Coefficient of Determination

The notebook compares the performance of all four models on the held-out test set.

### Results

Among the evaluated models, **SVR achieved the lowest RMSE and MAE** in the experiment.

However, the predictive performance was limited, with a weak/negative R² indicating that the available variables and dataset size were not sufficient to produce a strong predictive model.

This is an important limitation of the experiment rather than something that should be interpreted as a production-ready forecasting system.

---

## Model Interpretability

### Permutation Importance

Permutation importance was used to investigate which engineered features had the greatest influence on the SVR predictions.

The analysis identified:

1. `Total_ops_weekday`
2. `Total_ops_weekend`

as the most influential features in the experiment.

### SHAP

SHAP KernelExplainer was also applied to the SVR model to provide an additional interpretation of feature contributions.

---

## Project Workflow

```text
GIC Call Records
       │
       ▼
Data Loading
       │
       ▼
Data Cleaning & Preprocessing
       │
       ▼
Feature Engineering
       │
       ├── KB Coverage Ratio
       ├── Staffed Ratio
       ├── Total Weekday Operators
       ├── Total Weekend Operators
       └── Calls per Operator
       │
       ▼
Exploratory Data Analysis
       │
       ▼
Train / Test Split
       │
       ▼
Feature Scaling
       │
       ▼
┌─────────────────────────────┐
│ Regression Models           │
│                             │
│ • Bayesian Ridge            │
│ • SVR                       │
│ • LightGBM                  │
│ • Gaussian Process          │
└─────────────────────────────┘
       │
       ▼
Model Evaluation
       │
       ├── RMSE
       ├── MAE
       └── R²
       │
       ▼
Model Interpretation
       │
       ├── Permutation Importance
       └── SHAP
```

---

## Technologies Used

* Python
* Pandas
* NumPy
* Matplotlib
* Seaborn
* Scikit-learn
* LightGBM
* SHAP
* Google Colab / Jupyter Notebook

---

## Repository Structure

```text
.
├── GIC.csv
├── .gitignore
├── Optimizing_Call_Center_Efficiency.ipynb
├── Forecasting_Call_Waiting_Times.pdf
└── README.md
```

### Files

| File                                      | Description                              |
| ----------------------------------------- | ---------------------------------------- |
| `GIC.csv`                                 | Source GIC call-center dataset           |
| `Optimizing_Call_Center_Efficiency.ipynb` | Complete analysis and modelling notebook |
| `Forecasting_Call_Waiting_Times.pdf`      | Presentation slides for the project      |
| `README.md`                               | Project documentation                    |

---

## Limitations

This project was developed as an academic machine learning experiment and has several limitations.

### Small Dataset

The number of observations is relatively small for training a reliable predictive model.

### Limited Features

Waiting time is influenced by many real-world factors that are not represented in the available dataset, such as:

* Individual call complexity
* Operator experience
* Queue dynamics
* Hourly call arrival patterns
* Call duration
* Service-level policies
* Unexpected demand spikes

### Model Performance

The models did not achieve a strong R² score. Therefore, the results should **not** be interpreted as a production-ready forecasting solution.

The project is better viewed as an exploration of how machine learning can be applied to operational call-center data and how additional data could potentially improve future modelling.

---

## Academic Context

**Course:** CM3720 – Machine Learning

**Project:** Forecasting Call Waiting Times for the Sri Lanka Government Information Center

**Author:** Shiny Fernando

**Date:** November 2025

---

## Presentation

The accompanying presentation explains the problem formulation, dataset, feature engineering, modelling approaches, and experimental results.

See:

`Forecasting_Call_Waiting_Times.pdf`

---

## Author

**Shiny Fernando**

BSc (Hons) Artificial Intelligence
University of Moratuwa, Sri Lanka
