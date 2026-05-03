# Twitter Sentiment Analysis with DVC

A reproducible machine learning pipeline for binary Twitter sentiment analysis. The project uses DVC to version pipeline stages and generated artifacts, from data ingestion through preprocessing, feature engineering, model training, and evaluation.

The pipeline currently classifies tweets into two sentiment classes:

- `happiness` -> `1`
- `sadness` -> `0`

## Table of Contents

- [Project Overview](#project-overview)
- [Repository Layout](#repository-layout)
- [Pipeline Stages](#pipeline-stages)
- [Dataset](#dataset)
- [Tech Stack](#tech-stack)
- [Setup](#setup)
- [Run the Pipeline](#run-the-pipeline)
- [Run Individual Stages](#run-individual-stages)
- [Outputs](#outputs)
- [DVC Workflow](#dvc-workflow)
- [Troubleshooting](#troubleshooting)
- [Future Improvements](#future-improvements)

## Project Overview

This project demonstrates a small MLOps workflow for text classification using Twitter sentiment data. It downloads a tweet emotions dataset, filters it to two sentiment categories, cleans the text, converts tweets into Bag-of-Words features, trains a classifier, and writes evaluation metrics.

The main project code is inside:

```text
ML-pipeline-using-DVC/
```

The root folder contains repository-level Git and DVC metadata plus this README.

## Repository Layout

```text
Twitter-Sentiment-Analysis/
|-- .dvc/                         # Root DVC metadata
|-- .dvcignore
|-- .gitignore
|-- README.md                     # Project documentation
`-- ML-pipeline-using-DVC/
    |-- .dvc/                     # DVC metadata for the ML pipeline
    |-- .dvcignore
    |-- .gitignore
    |-- dvc.yaml                  # DVC pipeline definition
    |-- dvc.lock                  # Locked pipeline state
    |-- requirements.txt          # Python dependencies
    |-- notebook/
    |   `-- twitter.ipynb         # Notebook for exploration/experiments
    |-- data/
    |   `-- .gitignore            # Generated data is ignored/tracked by DVC
    `-- src/
        |-- data_ingestion.py
        |-- data_preprocessing.py
        |-- feature_engineering.py
        |-- model_building.py
        `-- model_evaluation.py
```

## Pipeline Stages

The DVC pipeline is defined in `ML-pipeline-using-DVC/dvc.yaml`.

| Stage | Script | Purpose | Output |
| --- | --- | --- | --- |
| `data_ingestion` | `src/data_ingestion.py` | Downloads the source CSV, filters sentiments, encodes labels, and splits train/test data | `data/raw/` |
| `data_preprocessing` | `src/data_preprocessing.py` | Cleans tweet text using lowercasing, stopword removal, number removal, punctuation removal, URL removal, and lemmatization | `data/processed/` |
| `feature_engineering` | `src/feature_engineering.py` | Converts cleaned tweet text into Bag-of-Words vectors with `CountVectorizer(max_features=50)` | `data/features/` |
| `model_building` | `src/model_building.py` | Trains a `GradientBoostingClassifier` | `model.pkl` |
| `model_evaluation` | `src/model_evaluation.py` | Evaluates the trained model on the test set | `metrics.json` |

## Dataset

The ingestion script downloads the dataset from:

```text
https://raw.githubusercontent.com/entbappy/Branching-tutorial/refs/heads/master/tweet_emotions.csv
```

The original dataset includes multiple emotion labels. This project filters the data to:

- `happiness`
- `sadness`

The `tweet_id` column is dropped during ingestion, and the `sentiment` column is converted to numeric labels.

## Tech Stack

- Python
- pandas
- NumPy
- scikit-learn
- NLTK
- DVC
- PyYAML
- XGBoost dependency is listed in `requirements.txt`, although the current training script uses scikit-learn's `GradientBoostingClassifier`

## Setup

Go to the project pipeline folder:

Create and activate a virtual environment:

```bash
python -m venv venv
venv\Scripts\activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Check that DVC is available:

```bash
dvc --version
```

## Run the Pipeline

Run the complete DVC pipeline:

```bash
dvc repro
```

DVC will execute only the stages that are missing, outdated, or affected by changed dependencies.

The pipeline order is:

```text
data_ingestion -> data_preprocessing -> feature_engineering -> model_building -> model_evaluation
```

## Run Individual Stages

You can also run scripts manually from inside `ML-pipeline-using-DVC/`.

Data ingestion:

```bash
python src/data_ingestion.py
```

Data preprocessing:

```bash
python src/data_preprocessing.py
```

Feature engineering:

```bash
python src/feature_engineering.py
```

Model training:

```bash
python src/model_building.py
```

Model evaluation:

```bash
python src/model_evaluation.py
```

## Outputs

After running the pipeline, these artifacts are generated:

```text
ML-pipeline-using-DVC/
|-- data/
|   |-- raw/
|   |   |-- train.csv
|   |   `-- test.csv
|   |-- processed/
|   |   |-- train_processed.csv
|   |   `-- test_processed.csv
|   `-- features/
|       |-- train_bow.csv
|       `-- test_bow.csv
|-- model.pkl
`-- metrics.json
```

`metrics.json` contains:

- `accuracy`
- `precision`
- `recall`
- `auc`

## DVC Workflow

Show the pipeline DAG:

```bash
dvc dag
```

Check pipeline status:

```bash
dvc status
```

Reproduce the pipeline:

```bash
dvc repro
```

Track newly generated pipeline files:

```bash
git add dvc.yaml dvc.lock .gitignore
git add src/ requirements.txt README.md
git commit -m "Update sentiment analysis pipeline"
```

If a DVC remote is configured, push DVC-tracked artifacts:

```bash
dvc push
```

To fetch artifacts from a configured DVC remote:

```bash
dvc pull
```

## Troubleshooting

If NLTK resources are missing, rerun the preprocessing stage. The script downloads `wordnet` and `stopwords` automatically:

```bash
python src/data_preprocessing.py
```

If output folders already exist and a script raises a folder creation error, remove the generated output folder for that stage or run through DVC so it manages the stage outputs:

```bash
dvc repro
```

If data ingestion fails, check your internet connection because the raw dataset is downloaded from GitHub.

If `dvc pull` does not download anything, confirm that a DVC remote is configured:

```bash
dvc remote list
```

## Future Improvements

- Save the fitted vectorizer so new tweets can be transformed consistently at inference time
- Add a prediction script or API for user-provided tweets
- Replace Bag-of-Words with TF-IDF or transformer embeddings
- Add hyperparameter tuning
- Add confusion matrix and classification report outputs
- Add automated tests for preprocessing and pipeline stages
- Configure and document a DVC remote for artifact sharing
