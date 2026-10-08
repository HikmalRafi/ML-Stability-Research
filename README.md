# Machine Learning Model Stability Analysis

[🇮🇩 Bahasa Indonesia](README_ID.md) | 🇬🇧 English

> Experimental analysis of the stability of Decision Tree, K-Nearest Neighbors, and Random Forest under variations in train-test data splitting.

---

## 📌 Overview

Machine Learning models are often evaluated using only a single train-test split.

However, changing the random seed used during data splitting can change which samples are included in the training and testing sets. As a result, model performance may also change even when the dataset, algorithm, and model configuration remain the same.

This project investigates the **stability of Machine Learning models against variations in train-test data splitting**.

The main research question is:

> **How much does model performance change when the train-test split is varied using different random seeds?**

Three classification algorithms are evaluated:

- Decision Tree
- K-Nearest Neighbors (KNN)
- Random Forest

The experiments are conducted on three public classification datasets:

- Wine Dataset
- Breast Cancer Wisconsin Diagnostic Dataset
- Raisin Dataset

---

## 🎯 Research Objectives

The main objectives of this research are:

1. Evaluate model performance across multiple train-test splits.
2. Measure the variability of model accuracy across different random seeds.
3. Compare the stability of Decision Tree, KNN, and Random Forest.
4. Determine whether the model with the highest average accuracy is also the most stable model.
5. Observe whether model stability changes across datasets with different characteristics.

---

## 🧪 Experimental Design

Each dataset is evaluated using the same general experimental procedure.

### Data Split

```text
Training data : 80%
Testing data  : 20%
```

Stratified splitting is used to preserve the class distribution between training and testing data.

### Random Seeds

Each experiment is repeated using:

```text
random_state = 1 to 30
```

The `random_state` used in the train-test split is varied, while the main model configuration is kept fixed.

This allows the experiment to focus on the effect of changing the composition of training and testing data.

### Models

| Model | Main Configuration |
|---|---|
| Decision Tree | Fixed internal random state |
| KNN | K = 5 |
| Random Forest | 100 trees, fixed internal random state |

`StandardScaler` is applied to KNN because KNN relies on distance calculations and is sensitive to differences in feature scale.

---

## 📊 Datasets

### 1. Wine Dataset

A multiclass classification dataset containing chemical measurements of wine samples.

```text
Samples  : 178
Features : 13
Classes  : 3
```

---

### 2. Breast Cancer Wisconsin Diagnostic Dataset

A binary classification dataset containing numerical features derived from characteristics of cell nuclei.

```text
Samples  : 569
Features : 30
Classes  : 2
```

---

### 3. Raisin Dataset

A binary classification dataset containing geometrical characteristics of two raisin varieties: **Kecimen** and **Besni**.

```text
Samples  : 900
Features : 7
Classes  : 2
```

---

## 📏 Evaluation Metrics

Model performance and stability are evaluated using:

- Mean Accuracy
- Minimum Accuracy
- Maximum Accuracy
- Median Accuracy
- Standard Deviation
- Accuracy Range

### Standard Deviation

A smaller standard deviation indicates that model performance changes less across different train-test splits.

### Accuracy Range

```text
Range = Maximum Accuracy - Minimum Accuracy
```

A smaller range indicates a smaller difference between the best and worst observed performance.

---

# 📈 Preliminary Results

## Wine Dataset

| Model | Mean Accuracy | Minimum | Maximum | Std. Dev. | Range |
|---|---:|---:|---:|---:|---:|
| Decision Tree | 90.93% | 80.56% | 97.22% | ~3.86% | 16.66% |
| KNN | 96.57% | 91.67% | 100.00% | 2.15% | 8.33% |
| Random Forest | **98.61%** | 94.44% | 100.00% | **1.59%** | **5.56%** |

Random Forest achieved both the highest average accuracy and the best stability on the Wine Dataset.

---

## Breast Cancer Wisconsin Diagnostic Dataset

| Model | Mean Accuracy | Minimum | Maximum | Std. Dev. | Range |
|---|---:|---:|---:|---:|---:|
| Decision Tree | 92.92% | 86.84% | 97.37% | 3.17% | 10.53% |
| KNN | **97.16%** | 92.98% | 99.12% | 1.41% | 6.14% |
| Random Forest | 96.32% | **93.86%** | 99.12% | **1.39%** | **5.26%** |

KNN achieved the highest average accuracy, while Random Forest showed slightly better stability.

---

## Raisin Dataset

| Model | Mean Accuracy | Minimum | Maximum | Std. Dev. | Range |
|---|---:|---:|---:|---:|---:|
| Decision Tree | 80.74% | 75.00% | 84.44% | 2.46% | 9.44% |
| KNN | 85.00% | 81.11% | 89.44% | **2.24%** | **8.33%** |
| Random Forest | **85.85%** | 81.11% | **90.00%** | 2.39% | 8.89% |

Random Forest achieved the highest average accuracy, while KNN showed slightly better stability.

---

## 🔎 Current Observations

The experiments indicate that model performance can change when the train-test split changes.

The experiments also show that:

> **The model with the highest average accuracy is not necessarily the most stable model.**

Current observations:

```text
Wine
Random Forest → Highest accuracy + highest stability

Breast Cancer
KNN           → Highest average accuracy
Random Forest → Highest stability

Raisin
Random Forest → Highest average accuracy
KNN           → Highest stability
```

These results are still preliminary and should not be interpreted as universal conclusions about the algorithms.

The results only describe the datasets and experimental configurations used in this research.

---

## 📁 Project Structure

```text
ML-Stability-Research/
│
├── data/
│
├── notebooks/
│   ├── wine/
│   │   ├── 01_wine_exploration.ipynb
│   │   ├── 02_wine_decision_tree.ipynb
│   │   ├── 03_wine_knn.ipynb
│   │   ├── 04_wine_random_forest.ipynb
│   │   └── 05_wine_comparison.ipynb
│   │
│   ├── breast_cancer/
│   │   ├── 01_breast_cancer_exploration.ipynb
│   │   ├── 02_breast_cancer_decision_tree.ipynb
│   │   ├── 03_breast_cancer_knn.ipynb
│   │   ├── 04_breast_cancer_random_forest.ipynb
│   │   └── 05_breast_cancer_comparison.ipynb
│   │
│   └── raisin/
│       ├── 01_raisin_exploration.ipynb
│       ├── 02_raisin_decision_tree.ipynb
│       ├── 03_raisin_knn.ipynb
│       ├── 04_raisin_random_forest.ipynb
│       └── 05_raisin_comparison.ipynb
│
├── results/
│   ├── wine/
│   ├── breast_cancer/
│   └── raisin/
│
├── src/
│
├── README.md
├── README_ID.md
├── requirements.txt
└── .gitignore
```

---

## 🔄 Research Workflow

```text
Dataset
   │
   ▼
Data Exploration
   │
   ▼
Feature / Target Separation
   │
   ▼
30 Train-Test Splits
(random_state 1–30)
   │
   ├───────────────┬───────────────┐
   ▼               ▼               ▼
Decision Tree     KNN        Random Forest
   │               │               │
   └───────────────┴───────────────┘
                   │
                   ▼
            Accuracy Results
                   │
                   ▼
          Statistical Analysis
                   │
        ┌──────────┼──────────┐
        ▼          ▼          ▼
       Mean      Std Dev     Range
                   │
                   ▼
          Stability Comparison
```

---

## ✅ Progress

### Dataset Experiments

- [x] Wine Dataset
  - [x] Data exploration
  - [x] Decision Tree
  - [x] KNN
  - [x] Random Forest
  - [x] Model comparison

- [x] Breast Cancer Wisconsin Diagnostic Dataset
  - [x] Data exploration
  - [x] Decision Tree
  - [x] KNN
  - [x] Random Forest
  - [x] Model comparison

- [x] Raisin Dataset
  - [x] Data exploration
  - [x] Decision Tree
  - [x] KNN
  - [x] Random Forest
  - [x] Model comparison

### Research Analysis

- [ ] Cross-dataset comparison
- [ ] Combined visualization
- [ ] Additional evaluation metrics
- [ ] Statistical significance analysis
- [ ] Interpretation of model stability
- [ ] Literature comparison

### Scientific Article

- [ ] Introduction
- [ ] Literature Review
- [ ] Methodology
- [ ] Results
- [ ] Discussion
- [ ] Conclusion
- [ ] Manuscript Formatting
- [ ] Final Review
- [ ] Journal Submission

---

## 🛠 Technologies

This project uses:

- Python
- Pandas
- NumPy
- Scikit-learn
- Matplotlib
- Jupyter Notebook
- Visual Studio Code
- Git
- GitHub

---

## ⚙️ Environment Setup

Create a virtual environment:

```bash
python3 -m venv .venv
```

Activate it on Linux:

```bash
source .venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Start Jupyter Lab:

```bash
jupyter lab
```

---

## ♻️ Reproducibility

The experiments are designed to be reproducible.

Important experimental parameters such as:

- Train-test ratio
- Random seeds
- Model configurations
- Preprocessing procedures

are documented and kept consistent throughout the experiments.

Raw experiment results are also stored as CSV files inside the `results/` directory.

---

## 🚧 Research Status

> **Status: Active Research / Work in Progress**

The current results are preliminary and may be expanded or refined before preparation of the final scientific manuscript.

Future stages may include additional evaluation metrics, statistical testing, cross-dataset analysis, and methodological validation.

---

## 👤 Author

**Fatih Hikmal Rafi**

Undergraduate Informatics Student

Research interests:

- Machine Learning
- Embedded Systems
- TinyML
- Internet of Things
- Reproducible Machine Learning

---

## ⚠️ Disclaimer

This repository is maintained for academic and research purposes.

The Breast Cancer Wisconsin Diagnostic Dataset is used only as a Machine Learning benchmark dataset.

The experiments in this repository are **not intended to provide medical diagnosis or clinical recommendations**.
