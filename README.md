# Twitter Sentiment Analysis using XGBoost + MLOps with DVC

![Project Overview](./b4037892-df86-41f6-aada-8d4d6498097f.png)

## Table of Contents
- [Project Overview](#project-overview)
- [Dataset](#dataset)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Pipeline Implementation](#pipeline-implementation)
- [MLOps Concepts](#mlops-concepts)
- [Results](#results)
- [Future Improvements](#future-improvements)
- [License](#license)

---

## Project Overview
This project performs **sentiment analysis on Twitter data** using **XGBoost** and integrates **MLOps practices** such as **data & model versioning using DVC**. The goal is to build a **reproducible and scalable ML pipeline** that tracks datasets, preprocessing steps, model artifacts, and experiments efficiently.

**Key Features:**
- Text preprocessing, feature engineering, and XGBoost classification
- End-to-end ML pipeline in `src/` folder
- DVC for **data, model, and pipeline version control**
- Reproducible experiments and evaluation metrics tracking
- Streamlined workflow for MLOps concepts demonstration

---

## Dataset
The dataset contains Twitter posts and corresponding sentiment labels.

- Features: `tweet_text`
- Target: `sentiment` (positive/negative/neutral)
- Stored under the `data/` folder and tracked using DVC

---

## Project Structure
```text
ML-pipeline-using-DVC/
│
├── .dvc/                       # DVC cache and configuration
├── data/                        # Raw and processed data tracked by DVC
├── notebook/                    # Jupyter notebooks for EDA and experiments
├── src/                         # Python scripts for the ML pipeline
│   ├── data_ingestion.py        # Load and version datasets
│   ├── data_preprocessing.py    # Clean and transform text data
│   ├── feature_engineering.py   # Generate features using TF-IDF
│   ├── model_building.py        # Train XGBoost classifier
│   └── model_evaluation.py      # Evaluate model and generate metrics
├── dvc.yaml                     # DVC pipeline stages
├── dvc.lock                     # Locked pipeline state
├── requirements.txt             # Python dependencies
└── README.md                    # Project documentation

```
Installation

Clone the repository:
```
git clone <your-repo-url>
cd ML-pipeline-using-DVC
```

Install dependencies:
```

pip install -r requirements.txt
```

Pull data and models via DVC:
```
dvc pull
```

Use :
it will automatically execute ingestion → preprocessing → feature engineering → training → evaluation with full versioning.
```
dvc repro
```



Pipeline Implementation
1. Data Ingestion (src/data_ingestion.py)

Load raw Twitter data

Version data using DVC

2. Data Preprocessing (src/data_preprocessing.py)

Clean tweets (remove punctuation, stopwords)

Encode categorical variables

Normalize numerical features

3. Feature Engineering (src/feature_engineering.py)

Transform text into TF-IDF vectors

Prepare feature matrix for XGBoost

4. Model Building (src/model_building.py)

Train XGBoost classifier

Save trained model artifact tracked by DVC

5. Model Evaluation (src/model_evaluation.py)

Compute metrics: Accuracy, F1-score, Confusion Matrix

Visualize evaluation results

MLOps Concepts

DVC Versioning: Track dataset, preprocessing, and models

Pipeline Automation: Run full pipeline stages with dvc repro

Reproducibility: Any change in data triggers rerun automatically

Experiment Tracking: Keep history of model parameters, metrics, and artifacts


Demonstrates XGBoost’s strong performance on text classification with a fully reproducible MLOps workflow.

Future Improvements

Real-time sentiment analysis using csv dataser

Drift monitoring and retraining logic

Upgrade text representation with transformer embeddings (BERT / RoBERTa)

License

This project is licensed under MIT License.
