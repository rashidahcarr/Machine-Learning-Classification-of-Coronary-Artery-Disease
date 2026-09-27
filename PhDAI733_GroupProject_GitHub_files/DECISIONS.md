# Decisions log

Decisions with lasting effect on the analysis, with the reasoning, so the rationale survives into Part 2.
Add a dated entry when a choice changes what later sections can assume.

| Date | Decision | Alternatives considered | Rationale |
|---|---|---|---|
| 2026-09-22 | Use the Heart Disease (UCI) dataset | Credit Card Default (UCI) | Team decision after meeting; clinical framing |
| 2026-09-23 | Title: "Machine Learning Classification of Coronary Artery Disease" | Several | Team decision; matches the repository name |
| 2026-09-24 | Use the Cleveland file only (303 patients) via `ucimlrepo` | All four site files (920) | Cleveland is the standard, nearly complete file with no placeholder zeros; the other three sites have heavy structural missingness. Cross-site validation deferred to Part 2 |
| 2026-09-24 | Drop the 6 rows (2%) missing `ca` or `thal` | Impute | Keeps every model on the same 297 patients so feature-set comparisons are apples to apples; cost is negligible |
| 2026-09-24 | Collapse target `num` (0 to 4) to binary disease vs none | Keep five classes | Clinical question is presence of disease; severity classes are tiny (13 patients in the smallest) |
| 2026-09-24 | Model excludes `ca` and `thal`; uses the 11 first-line predictors | Include all 13 predictors | Fluoroscopy and thallium come late in the pathway; a referral model must work from the first-line workup. Quantifying what they add is a Part 2 comparison |
| 2026-09-25 | Preprocessing as a scikit-learn `ColumnTransformer` inside a `Pipeline` (scale continuous, one-hot categorical with drop first, pass binary through) | Manual `get_dummies` + scaler | Fitting happens inside the pipeline on training data only, so leakage is impossible by construction |
| 2026-09-25 | 80/20 stratified split, `random_state=42` | 75/25 (hint sheet default) | More training rows for a 297-patient dataset; stratification keeps 46% prevalence in both halves |
| 2026-09-25 | Initial model: Random Forest (100 trees) | Logistic regression first | Captures non-linear effects and interactions; a baseline and logistic regression are queued for Part 2 |
| 2026-09-25 | `GridSearchCV` scored on disease recall (5-fold stratified) | Accuracy (hint sheet default) | A missed diagnosis is the costly error. Note: the tuned model did not beat the initial one on the 60-patient test set (recall .714 vs .750); threshold tuning is the Part 2 lever |
| 2026-09-24 | No forward-fill (contrary to the hint sheet example) | `fillna(method='ffill')` | Rows are independent patients; forward-fill would copy one patient's results into another's. Not needed in any case after the six rows were dropped |
