# U3289171_DS_Assignment1 — Reproducibility & Workflow

This repository demonstrates **reproducible workflows** using Git, Git LFS, and DVC, aligned with Assignment 1 — Part C (10 marks).  
It is tailored to your project files:

- `notebooks/U3289171_Assignment1.ipynb`  
- `notebooks/sydney.geojson`  
- **Dataset (Google Drive)**: [Zomato Final Data](https://drive.google.com/file/d/1rBwSjUz5dtxpCd0l8ozCOh5lj05XXslw/view?usp=sharing)

---

## 1) Environment Setup

```bash
# Create and activate a virtual environment
python -m venv .venv                # Windows
. .venv/Scripts/Activate.ps1

python3 -m venv .venv               # macOS/Linux
source .venv/bin/activate

# Install dependencies
pip install -r requirements.txt

```

## 2) Git & GitHub

```bash
git init
git add .gitattributes
git lfs install

# LFS tracks heavy visualisations & notebooks by default
git add .
git commit -m "Init: Repo + LFS + DVC DS_Assignment1"
git branch -M main
git remote add origin https://github.com/U3289171/DS_Assignment1_U3289171.git
git push -u origin main
```

Notes:
- We initialise **Git LFS** for figures (`reports/figures/**`, common image types) and `.ipynb`.
- Datasets/models are managed by **DVC** here. If your rubric insists on LFS for datasets/models,
  you may uncomment patterns in `.gitattributes`, but avoid double-tracking with DVC.

## 3) DVC

```bash
# Initialise DVC and set a remote (choose one)
dvc init
# Local directory remote
dvc remote add -d localstore ./dvcstore

# Track raw data and external resources
pip install gdown
gdown 1rBwSjUz5dtxpCd0l8ozCOh5lj05XXslw -O notebooks/zomato_df_final_data.csv

dvc add notebook/zomato_df_final_data.csv
dvc add notebook/sydney.geojson

git add notebook/zomato_df_final_data.csv.dvc notebook/sydney.geojson.dvc .gitignore .dvc .dvcignore dvc.yaml params.yaml
git commit -m "DVC: track raw data & pipeline"
```

Reproduce the **full pipeline** end-to-end:

```bash
dvc repro
```

Push data/model artifacts to your DVC remote & code to GitHub:

```bash
dvc push
git push
```

## 4) How to Run the Project

```bash
# 1) Activate environment
source . .venv/Scripts/Activate.ps1 on Windows

# 2) Install deps
pip install -r requirements.txt

# 3) Reproduce pipeline
dvc repro


## 4) Expected Results (Template)

- A baseline binary classification distinguishing **high-cost** restaurants (`cost >= 60 AUD`) vs others.
- `reports/metrics.json` with baseline classification accuracy.
- A cost distribution histogram at `reports/figures/cost_hist.png`.

Extend the pipeline by:
- Richer feature engineering (e.g., cuisines, location grids using `sydney.geojson`)
- Regression experiments (predicting `cost`) alongside classification
- Alternative models (SVM, RandomForest, GradientBoosting) with tracked metrics

## 5) Project Layout

```
.
├── .dvcignore
├── .gitattributes
├── .gitignore
├── dvc.yaml
├── params.yaml
├── requirements.txt
├── notebooks
    └── sydney.geojson
    └── U3289171_Assignment1.ipynb
    

```

## 6) Reproducibility Checklist

- Git repo initialised; commits pushed to GitHub
- Git LFS initialised; heavy figures & notebooks tracked
- DVC initialised; raw data tracked and pipeline defined
- `dvc repro` rebuilds all stages from code/param changes
- `params.yaml` centralises parameters; changing them triggers the right stages
