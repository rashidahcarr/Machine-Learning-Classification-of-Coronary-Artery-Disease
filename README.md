# PhDAI 733 Group Project: Machine-Learning-Classification-of-Coronary-Artery-Disease

Real-World AI-Driven Analytics Solution Using Python.   
University of the Cumberlands, PhDAI 733 (Python Application for Analytics in AI).

**Part 1 deliverable:** Data Preprocessing and Initial Model Development

## Team

| Member  | Part 1 | Part 2 |
| ----  | ---- | ---- |
| Rashidah Carr | Problem Framing, Ethics and Fairness, APA Report, GitHub| Integration of sections, baseline and age-band checks, ethics, conclusion, APA report, GitHub |
| Gino Varghese | Dataset Exploration & Preprocessing | Four-hospital dataset, tuning, threshold sweep |
| Jude Adenuga | Initial Model Development | XGBoost, hyperparameter tuning |
| Pranathy Anumula  | Team Collaboration Process | Real-World Impact |


## Dataset

Heart Disease, UCI Machine Learning Repository (Janosi, Steinbrunn, Pfisterer, & Detrano, 1988).
https://archive.ics.uci.edu/dataset/45/heart+disease

The notebook fetches the data at run time with the ucimlrepo package (fetch_ucirepo(id=45)), so no data file is
stored in this repository and nothing needs to be uploaded. 303 patients, 13 predictors; 297 after dropping six rows
with missing fluoroscopy or thallium values. The model uses the 11 first-line predictors (ca and thal excluded).

## How to run

1. Open the shared Colab notebook above, or open notebooks/PhDAI733_GroupProject_Part1_HeartDisease.ipynb in
Google Colab (File > Open notebook > GitHub, or upload).
2. Runtime > Run all. The first cell installs ucimlrepo; the data are fetched from UCI automatically.
3. The full run takes about two minutes on the free Colab tier (the grid search in Section 7 is the slow step).

## Repository layout

```
notebooks/   the Colab notebook (committed with outputs cleared)
report/      the APA report (.docx)
```
