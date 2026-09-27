# Machine Learning Classification of Coronary Artery Disease

PhDAI 733 Group Project (Python Application for Analytics in AI), University of the Cumberlands.
Real-World AI-Driven Analytics Solution Using Python.

**Part 1 deliverable:** Data Preprocessing and Initial Model Development.

**Repository:** https://github.com/rashidahcarr/Machine-Learning-Classification-of-Coronary-Artery-Disease

**Shared team notebook (Colab):** https://colab.research.google.com/drive/1-2iWPOEXS-5jqGCdap0BR78PVC3D5qFn?usp=sharing

## Team

Jude Adenuga, Pranathy Anumula, Rashidah Carr, and Gino Varghese. Responsibilities were divided by
notebook section (data exploration and cleaning, preprocessing and split, model development and tuning,
problem framing, ethics, and report); Section 9 of the notebook describes the collaboration process.

## Dataset

Heart Disease, UCI Machine Learning Repository (Janosi, Steinbrunn, Pfisterer, & Detrano, 1988), Cleveland file.
https://archive.ics.uci.edu/dataset/45/heart+disease

The notebook fetches the data at run time with the `ucimlrepo` package (`fetch_ucirepo(id=45)`), so **no data file is
stored in this repository** and nothing needs to be uploaded. 303 patients, 13 predictors; 297 after dropping six rows
with missing fluoroscopy or thallium values. The model uses the 11 first-line predictors (`ca` and `thal` excluded).

## Files

| File | What it is |
|---|---|
| `notebooks/PhDAI733_GroupProject_Part1_HeartDisease.ipynb` | The Colab notebook, committed with outputs cleared (the executed copy lives in the shared Colab) |
| `report/PhDAI733_GroupProject_Part1_Report.docx` | The APA report for Part 1 |
| `DECISIONS.md` | Decisions with lasting effect and the reasoning behind them |
| `requirements.txt` | Python packages the notebook needs |
| `.gitignore` | Keeps data files and notebook checkpoints out of the repository |

## How to run

1. Open the shared Colab notebook above, or open `notebooks/PhDAI733_GroupProject_Part1_HeartDisease.ipynb` in
   Google Colab (File > Open notebook > GitHub, or upload).
2. Runtime > Run all. The first cell installs `ucimlrepo`; the data are fetched from UCI automatically.
3. The full run takes about two minutes on the free Colab tier (the grid search in Section 7 is the slow step).

All randomness is seeded with `RANDOM_STATE = 42`, so a rerun reproduces the reported numbers.

## Repository layout

```
notebooks/   the Colab notebook (committed with outputs cleared)
report/      the APA report (.docx)
DECISIONS.md decisions with lasting effect and the reasoning behind them
requirements.txt
```

## Workflow

- `main` always holds the last reviewed, fully executing notebook.
- Work on your section in the shared Colab, then push to a branch named for it (`steps-1-3-eda`, `modeling`, `ethics`).
- Commit at least once per working session with a descriptive message. Clear outputs before committing
  (Edit > Clear all outputs in Colab) so diffs stay readable; executed copies live in the team Drive folder.
- Open a pull request into `main`; one other member reviews and confirms the notebook runs top to bottom in a fresh runtime.
- Record any decision that later sections depend on in `DECISIONS.md`.
