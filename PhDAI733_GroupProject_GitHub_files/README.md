# PhDAI 733 Group Project: Machine Learning Classification of Coronary Artery Disease

Real-World AI-Driven Analytics Solution Using Python.
University of the Cumberlands, PhDAI 733 (Python Application for Analytics in AI).

| Part | Deliverable | Status |
| --- | --- | --- |
| Part 1 (Module 5) | Data Preprocessing and Initial Model Development | Submitted |
| Part 2 (Module 8) | Model Optimization, Real-World Impact, and Ethical Evaluation | Final |

**Shared team notebook (Colab):** https://colab.research.google.com/drive/1-2iWPOEXS-5jqGCdap0BR78PVC3D5qFn?usp=sharing

## The question

Can a model built only from a patient's first-line cardiac workup (history, symptoms, resting ECG, basic blood tests,
and an exercise stress test) estimate the probability of significant coronary narrowing well enough to help a
cardiology clinic decide who should go to angiography first?

## Team

| Member | Part 1 | Part 2 |
| --- | --- | --- |
| Rashidah Carr | Problem framing, ethics and fairness, report, GitHub (Introduction, Sections 8 and 10) | Integration of sections, baseline and age-band checks, ethics, real-world application, conclusion, APA report, GitHub |
| Gino Varghese | Dataset exploration and preprocessing (Sections 1 to 3) | Four-hospital dataset, tuning, threshold sweep, and final evaluation (Sections 2 to 8) |
| Jude Adenuga | Initial model development (Sections 4 to 7) | *add Part 2 role* |
| Pranathy Anumula | Team collaboration process (Section 9) | *add Part 2 role* |

The XGBoost comparison model in Part 2 was added by a teammate; add their name in the row above.

## Dataset

Heart Disease, UCI Machine Learning Repository (Janosi, Steinbrunn, Pfisterer, & Detrano, 1988).
https://archive.ics.uci.edu/dataset/45/heart+disease

**No data file is stored in this repository.** Both notebooks fetch the data from UCI at run time.

- **Part 1** uses the Cleveland file only, through `ucimlrepo` (`fetch_ucirepo(id=45)`): 303 patients, 297 after
  dropping six rows missing the fluoroscopy or thallium values.
- **Part 2** reads all four processed hospital files directly from the UCI archive (Cleveland 303, Hungary 294,
  Switzerland 123, Long Beach VA 200), for 920 patients.

Both parts use the same 11 first-line predictors. The fluoroscopy vessel count (`ca`) and thallium scan (`thal`)
are excluded because they come late in the diagnostic pathway.

## Part 2 results at a glance

| | Result |
| --- | --- |
| Models compared | Majority baseline, logistic regression, Random Forest, gradient boosting, XGBoost |
| Cross-validated ROC-AUC (tuned) | XGBoost .8719, Random Forest .8716, logistic regression .8700, gradient boosting .8653, baseline .5000 |
| Final model | Random Forest (100 trees, maximum depth 5), kept over XGBoost by team decision (see `DECISIONS.md`) |
| Decision threshold | 0.42 (maximizes cross-validated F1 for the disease class) |
| Test set (184 patients) | ROC-AUC .920, recall .931 (7 of 102 diseased patients missed), specificity .646 |

See the notebook and `report/PhDAI733_GroupProject_Part2_Report.docx` for results by sex, age, and hospital,
the ethical analysis, and the real-world application.

## Files

| File | What it is |
| --- | --- |
| `notebooks/PhDAI733_GroupProject_Part1_HeartDisease.ipynb` | Part 1 Colab notebook (outputs cleared) |
| `notebooks/PhDAI733_GroupProject_Part2_HeartDisease.ipynb` | Part 2 Colab notebook (outputs cleared) |
| `report/PhDAI733_GroupProject_Part1_Report.docx` | Part 1 APA report |
| `report/PhDAI733_GroupProject_Part2_Report.docx` | Part 2 APA report |
| `DECISIONS.md` | Decisions with lasting effect and the reasoning behind them |
| `requirements.txt` | Python packages the notebooks need |
| `.gitignore` | Keeps data files and notebook checkpoints out of the repository |

## How to run

1. Open the notebook in Google Colab (File > Open notebook > GitHub, or upload it).
2. Runtime > Run all. The first cell installs the packages, and the data are fetched from UCI automatically.
3. Part 1 takes about two minutes. Part 2 takes about three to five minutes on the free Colab tier, mostly in the
   Random Forest grid search in Section 7.1.

All randomness is seeded with `RANDOM_STATE = 42`, so a rerun reproduces the reported numbers. Very small
differences in the fourth decimal place (for example .8716 against .8717) can appear across library versions.

## Repository layout

```
notebooks/        the Colab notebooks (committed with outputs cleared)
report/           the APA reports (.docx)
DECISIONS.md      decisions with lasting effect and the reasoning behind them
requirements.txt
.gitignore
```

## Workflow

- `main` always holds the last reviewed, fully executing notebook.
- Work on your section in the shared Colab, then push to a branch named for it (for example `part2-tuning` or `part2-ethics`).
- Commit at least once per working session with a descriptive message. Clear outputs before committing
  (Edit > Clear all outputs in Colab) so diffs stay readable. Executed copies live in the team Drive folder.
- Open a pull request into `main`. One other member reviews it and confirms the notebook runs top to bottom in a fresh runtime.
- Record any decision that later sections depend on in `DECISIONS.md`.
