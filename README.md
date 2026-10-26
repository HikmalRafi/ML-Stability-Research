# Machine Learning Model Stability Analysis

[🇬🇧 English](README.md) | [🇮🇩 Bahasa Indonesia](README_ID.md)

An empirical study of how train-test data split variations affect classification performance, model stability, and model-ranking consistency.

---

## Overview

Machine learning models are often evaluated using a single train-test split. However, the composition of the training and testing sets can change when a different random seed is used. This may lead to different performance estimates and, in some cases, different conclusions about which model performs best.

This project investigates the **stability of classification performance under variations in train-test splitting**.

The experiments compare three classification algorithms across three tabular datasets using 100 random seeds per dataset. In addition to measuring performance variability, the analysis examines whether the model with the highest average score consistently ranks first across different splits.

### Research Question

> How does variation in train-test splitting affect classification performance estimates, model stability, and the consistency of model rankings?

## Research Objectives

- Measure classification performance across multiple train-test splits.
- Compare performance variability using standard deviation and range.
- Examine whether the highest-mean-performing model is also the most stable.
- Analyze changes in model rankings across different splits.
- Compare conclusions drawn from Accuracy and Macro F1-score.
- Provide a reproducible experimental workflow and accessible result files.

## Experimental Design

| Component | Configuration |
|---|---|
| Datasets | 3 |
| Classification algorithms | 3 |
| Random seeds | 100 per dataset |
| Total model evaluations | 900 |
| Training set | 80% |
| Testing set | 20% |
| Data splitting | Stratified train-test split |
| Primary metrics | Accuracy and Macro F1-score |

For each dataset, the same split seed is used for all three algorithms. The model configurations are kept fixed so the experiment focuses on the effect of changing the composition of the training and testing sets.

### Classification Algorithms

| Algorithm | Configuration |
|---|---|
| Decision Tree | Fixed internal `random_state=42` |
| K-Nearest Neighbors (KNN) | `n_neighbors=5`, with `StandardScaler` in a pipeline |
| Random Forest | `n_estimators=100`, fixed internal `random_state=42` |

For KNN, `StandardScaler` is placed inside a scikit-learn pipeline to ensure that scaling parameters are fitted using the training data only.

The model configurations are held constant across the experiment. Therefore, the study focuses on split-induced variation rather than hyperparameter optimization or variation in model-internal random seeds.

## Datasets

Three public classification datasets are used.

| Dataset | Samples | Features | Classes | Task |
|---|---:|---:|---:|---|
| Wine | 178 | 13 | 3 | Multiclass classification |
| Breast Cancer Wisconsin Diagnostic | 569 | 30 | 2 | Binary classification |
| Raisin | 900 | 7 | 2 | Binary classification |

The Wine dataset contains chemical measurements of wine samples. The Breast Cancer Wisconsin Diagnostic dataset contains numerical features derived from cell-nucleus characteristics. The Raisin dataset contains geometric measurements used to classify two raisin varieties.

## Evaluation Metrics

### Performance Metrics

- **Accuracy:** the proportion of correctly classified samples.
- **Macro F1-score:** the unweighted average of F1-scores across classes, giving each class equal importance.

### Stability Metrics

- **Mean:** average performance across the 100 splits.
- **Median:** the middle performance value.
- **Standard deviation (SD):** measures variation in performance across splits.
- **Minimum and maximum:** the lowest and highest observed performance.
- **Range:** the difference between maximum and minimum performance.

A lower standard deviation or range indicates less observed variation under the corresponding measure. These measures capture different aspects of stability and should be interpreted alongside average performance.

### Ranking Analysis

The final analysis also examines:

- **Winner counts:** how often each model achieves the highest score on an individual split, with ties recorded separately.
- **Rank reversals:** cases where the winner on an individual split excludes the model with the highest overall mean score.
- **Performance gaps:** the difference between the highest and second-highest scores on a split.
- **Metric agreement:** whether Accuracy and Macro F1 identify the same set of top-performing models.

## Results

The following summaries are based on the current final experiment outputs. Performance values, standard deviations, and ranges are expressed as percentages or percentage points, as appropriate.

### Accuracy

| Dataset | Model | Mean | SD | Range |
|---|---|---:|---:|---:|
| Breast Cancer | Decision Tree | 93.11% | 2.67 pp | 10.53 pp |
| Breast Cancer | KNN | **96.62%** | **1.49 pp** | **7.02 pp** |
| Breast Cancer | Random Forest | 96.18% | 1.62 pp | **7.02 pp** |
| Raisin | Decision Tree | 81.27% | 2.66 pp | 13.89 pp |
| Raisin | KNN | 85.17% | **2.14 pp** | **10.00 pp** |
| Raisin | Random Forest | **86.09%** | 2.25 pp | 11.11 pp |
| Wine | Decision Tree | 90.17% | 4.74 pp | 19.44 pp |
| Wine | KNN | 96.25% | 2.51 pp | 13.89 pp |
| Wine | Random Forest | **98.31%** | **1.89 pp** | **5.56 pp** |

### Macro F1-score

| Dataset | Model | Mean | SD | Range |
|---|---|---:|---:|---:|
| Breast Cancer | Decision Tree | 92.62% | 2.82 pp | 11.13 pp |
| Breast Cancer | KNN | **96.32%** | **1.64 pp** | 7.89 pp |
| Breast Cancer | Random Forest | 95.89% | 1.74 pp | **7.35 pp** |
| Raisin | Decision Tree | 81.24% | 2.67 pp | 14.10 pp |
| Raisin | KNN | 85.14% | **2.14 pp** | **10.00 pp** |
| Raisin | Random Forest | **86.07%** | 2.25 pp | 11.12 pp |
| Wine | Decision Tree | 90.27% | 4.75 pp | 20.18 pp |
| Wine | KNN | 96.35% | 2.48 pp | 13.70 pp |
| Wine | Random Forest | **98.36%** | **1.84 pp** | **5.59 pp** |

*pp = percentage points. Bold values highlight the highest mean or the lowest variability within each dataset and metric, where applicable.*

### Key Observations

1. **The highest average performance does not always imply the lowest variability.** On the Raisin dataset, Random Forest achieves the highest mean score, while KNN has lower standard deviation and range for both Accuracy and Macro F1.
2. **The results depend on the dataset.** Random Forest achieves the highest mean scores and lowest observed variability on Wine, whereas KNN achieves the highest mean Accuracy on Breast Cancer.
3. **Stability depends on the metric used.** For Breast Cancer, KNN has lower standard deviation, while Random Forest has a slightly smaller Macro F1 range.
4. **Model rankings can change between splits.** The model with the highest overall mean does not win every individual split.

These findings describe the datasets and configurations examined in this project. They should not be interpreted as universal rankings of the algorithms.

## Rank-Reversal Analysis

The table below reports the number of splits in which the individual-split winner excludes the model with the highest overall mean score.

| Dataset | Accuracy | Macro F1 |
|---|---:|---:|
| Breast Cancer | 31 / 100 | 46 / 100 |
| Raisin | 25 / 100 | 27 / 100 |
| Wine | 5 / 100 | 8 / 100 |

These counts exclude cases classified as ties with the overall best model. They illustrate that conclusions about the best-performing model may depend on the particular train-test split, especially for some datasets and evaluation metrics.

## Repository Structure

```text
ML-Stability-Research/
├── data/
│   ├── raisin/
│   └── wine/
├── notebooks/
│   ├── final_experiment/
│   │   ├── 01_final_experiment.ipynb
│   │   └── 02_final_analysis.ipynb
│   ├── breast_cancer/
│   ├── raisin/
│   └── wine/
├── results/
│   ├── final/
│   ├── breast_cancer/
│   ├── raisin/
│   └── wine/
├── LICENSE
├── README.md
└── README_ID.md
```

The dataset-specific notebooks contain exploratory and earlier experiments. The `final_experiment` notebooks are the primary entry points for reproducing and analyzing the final experiment.

## Reproducing the Experiments

### 1. Clone the Repository

```bash
git clone https://github.com/HikmalRafi/ML-Stability-Research.git
cd ML-Stability-Research
```

### 2. Create a Virtual Environment

```bash
python3 -m venv .venv
source .venv/bin/activate
```

On Windows, activate the environment with:

```powershell
.venv\Scripts\activate
```

### 3. Install Dependencies

```bash
python -m pip install --upgrade pip
python -m pip install numpy pandas scikit-learn matplotlib jupyter
```

### 4. Run the Notebooks in Order

1. Open [`01_final_experiment.ipynb`](notebooks/final_experiment/01_final_experiment.ipynb) and run its cells to execute the experiments.
2. Open [`02_final_analysis.ipynb`](notebooks/final_experiment/02_final_analysis.ipynb) and run its cells to analyze the results.

Launch Jupyter Notebook with:

```bash
jupyter notebook
```

Run the experiment notebook before the analysis notebook when regenerating results. The analysis notebook depends on the experiment output files.

For exact reproducibility, use the documented configurations and the same dependency versions as the original environment. The repository does not currently include a pinned dependency file.

## Results and Data Files

The main experiment outputs are stored in [`results/final/`](results/final/).

| File | Description |
|---|---|
| [`final_experiment_results.csv`](results/final/final_experiment_results.csv) | Raw results from all model evaluations |
| [`accuracy_summary.csv`](results/final/accuracy_summary.csv) | Descriptive statistics for Accuracy |
| [`macro_f1_summary.csv`](results/final/macro_f1_summary.csv) | Descriptive statistics for Macro F1 |
| [`accuracy_win_counts.csv`](results/final/accuracy_win_counts.csv) | Accuracy winners and ties |
| [`f1_win_counts.csv`](results/final/f1_win_counts.csv) | Macro F1 winners and ties |
| [`accuracy_rank_reversal_summary.csv`](results/final/accuracy_rank_reversal_summary.csv) | Accuracy ranking consistency |
| [`f1_rank_reversal_summary.csv`](results/final/f1_rank_reversal_summary.csv) | Macro F1 ranking consistency |
| [`accuracy_gap_summary.csv`](results/final/accuracy_gap_summary.csv) | Accuracy gaps between leading models |
| [`f1_gap_summary.csv`](results/final/f1_gap_summary.csv) | Macro F1 gaps between leading models |

Additional CSV files contain non-tie performance gaps, winner-gap summaries, and comparisons between the two evaluation metrics.

## Limitations

This study has several limitations:

- Only three datasets and three classification algorithms are examined.
- Model hyperparameters are fixed rather than optimized separately for each dataset.
- The results measure variability associated with train-test split changes under the selected configurations; they do not establish universal algorithm rankings.
- Repeated holdout splits may overlap and are not necessarily statistically independent. The observed scores should not be treated as 100 independent observations for naive inferential testing.
- The findings may differ with other datasets, split ratios, preprocessing methods, model configurations, or evaluation procedures.

These limitations should be considered when interpreting the findings or extending the study.

## Research Status

**Current stage:** Final experiment outputs and descriptive analyses are available; scientific manuscript preparation is in progress.

The current scope focuses on split-induced performance variability, ranking consistency, and the relationship between average performance and stability. Further methodological changes should be made only when they address a clearly defined research question or a justified reviewer or supervisor concern.

## Author

**Fatih Hikmal Rafi**

Undergraduate Informatics Student

Research interests:
- Machine Learning
- Reproducible Machine Learning
- Embedded Systems
- TinyML
- Internet of Things

## License

See [`LICENSE`](LICENSE) for the licensing terms applicable to this repository. Dataset-specific terms may differ from the repository's software license.

## Disclaimer

This repository is intended for academic and research purposes. The Breast Cancer Wisconsin Diagnostic dataset is used solely as a machine learning benchmark. The experiments are **not intended for medical diagnosis or clinical recommendations**.
