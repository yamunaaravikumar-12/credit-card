# Credit Card Default Prediction

> **Machine learning | Binary classification | Python**

Can customer payment information help identify the likelihood of a future credit card default? This project explores that question by preparing a credit-card dataset, examining patterns in the data, and comparing three classification models.

---

## Project snapshot

| Item | Details |
|---|---|
| Task | Predict next-month payment default |
| Learning type | Supervised machine learning |
| Problem type | Binary classification |
| Dataset size | 30,000 customer records |
| Input variables | 23, excluding the ID and target |
| Train/test split | 80% / 20% |
| Models tested | Logistic Regression, Decision Tree, Random Forest |
| Main evaluation focus | F1-score, alongside recall and ROC-AUC |

## What is the model predicting?

The output column is `default payment next month`.

- `0` — the customer did not default
- `1` — the customer defaulted

The notebook uses customer and account information such as credit limit, demographic categories, repayment status, bill amounts, and previous payment amounts as input features.

The dataset contains 23,364 records in class `0` and 6,636 records in class `1`. This class imbalance matters: a model can achieve reasonable accuracy while still missing many customers in the default class.

## From raw data to prediction

The notebook moves through four broad phases:

### 1. Understand the data

- Load the Excel dataset into Pandas.
- Inspect rows, columns, data types, and summary statistics.
- Correct the dataset's header layout and remove an extra index column when present.
- Check for missing values and duplicate rows.

### 2. Explore and prepare

- Apply IQR-based capping to selected numerical columns.
- Plot distributions for age and credit limit.
- Compare default rates across selected customer categories.
- Inspect correlations among selected numerical variables.
- Separate the predictors from the target and exclude the customer ID.

### 3. Build the modelling pipeline

The notebook uses Scikit-learn preprocessing pipelines so that the transformations used for training are also applied consistently during prediction.

| Feature type | Preparation |
|---|---|
| Numerical | Median imputation, then standard scaling |
| Categorical (`SEX`, `EDUCATION`, `MARRIAGE`) | Most-frequent imputation, then one-hot encoding |

After preprocessing, `SelectKBest` with the ANOVA F-test (`f_classif`) retains the top 20 features. The data is divided into training and testing sets using an 80:20 stratified split.

### 4. Train and compare

Three classifiers are trained using the same preprocessing and feature-selection steps:

- **Logistic Regression** — provides a linear classification baseline.
- **Decision Tree** — learns decision rules from the input features.
- **Random Forest** — combines multiple decision trees to produce a classification.

---

## Experiment results

These are the scores saved in the notebook's current output:

| Classifier | Accuracy | Precision | Recall | F1-score | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Logistic Regression | 0.806 | 0.681 | 0.234 | 0.349 | 0.715 |
| Decision Tree | 0.713 | 0.362 | 0.393 | 0.377 | 0.605 |
| Random Forest | **0.811** | 0.621 | 0.372 | **0.466** | **0.744** |

**Random Forest has the highest F1-score and ROC-AUC in this recorded run.** Logistic Regression has higher precision, while the Decision Tree has higher recall than the other two models.

### How to read these scores

- **Accuracy:** How often the model's predictions are correct overall.
- **Precision:** How many predicted defaults were actually defaults.
- **Recall:** How many actual defaults the model successfully identified.
- **F1-score:** A combined measure of precision and recall.
- **ROC-AUC:** How well the model distinguishes between the two classes across thresholds.

For this dataset, it is important to look beyond accuracy. In particular, recall indicates how many actual default cases are being found. The best model depends on the cost of false alarms compared with missed defaults.

---

## Repository contents

```text
Credit-Card-Default-Prediction/
│
├── creditcard-project .ipynb
├── credit card.xls
└── README.md
```

The notebook and dataset should be kept in the same directory unless the dataset path in the notebook is changed.

## Run it locally

**Requirements:** Python and Jupyter Notebook.

### 1. Get the project

```bash
git clone <your-repository-url>
cd <your-repository-folder>
```

Or download the notebook and dataset manually.

### 2. Install the libraries

```bash
pip install numpy pandas matplotlib seaborn scikit-learn xlrd jupyter
```

### 3. Add the dataset

Place the Excel file named `credit card.xls` beside the notebook. The data-loading cell expects this filename:

```python
data = pd.read_excel("credit card.xls")
```

### 4. Launch Jupyter

```bash
jupyter notebook
```

Open `creditcard-project .ipynb` and run the cells in order.

> **Notebook setup note:** The feature/target split cell refers to `target_column`. Define it before that cell:
>
> ```python
> target_column = "default payment next month"
> ```

---

## Things to keep in mind

**Outlier treatment and evaluation:** In the current notebook, IQR limits are calculated and applied before the train/test split. Since the test data contributes to those limits, this can introduce information leakage. A stronger evaluation would calculate the limits from the training data only, then apply those same limits to the test data.

**Interpret the results carefully:** The reported metrics describe this dataset and this particular run. They do not establish that the model will perform equally well on new customers or in a real lending environment.

## Possible next steps

- Move outlier capping into a training-only preprocessing step.
- Tune model parameters and compare additional classifiers.
- Explore methods for handling class imbalance.
- Examine feature importance and explain individual predictions.
- Validate the model on separate, unseen data.

## Learning outcomes

This project provides practice with an end-to-end classification workflow: working with tabular data, exploratory analysis, handling numerical and categorical features, building Scikit-learn pipelines, selecting features, training classifiers, and interpreting evaluation metrics.

## Disclaimer

This is an educational machine learning project. It is not intended for real credit approval or lending decisions. Any real-world application would require careful validation, fairness and bias assessment, explainability, privacy protection, and appropriate regulatory review.

---

**Author:** R.S.Yamunaa
**Project:** Credit Card Default Prediction
