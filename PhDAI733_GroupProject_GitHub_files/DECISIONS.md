# Decisions log

Decisions with lasting effect on the analysis, with the reasoning, so the rationale survives from one part to the next.
Add a dated entry when a choice changes what later sections can assume.

## Part 1

| Date | Decision | Alternatives considered | Rationale |
|---|---|---|---|
| 2026-09-22 | Use the Heart Disease (UCI) dataset | Credit Card Default (UCI) | Team decision after meeting; clinical framing |
| 2026-09-23 | Title: "Machine Learning Classification of Coronary Artery Disease" | Several | Team decision; matches the repository name |
| 2026-09-24 | Use the Cleveland file only (303 patients) via `ucimlrepo` | All four site files (920) | Cleveland is the standard, nearly complete file with no placeholder zeros; the other three sites have heavy structural missingness. Cross-site validation deferred to Part 2 (superseded in Part 2, see below) |
| 2026-09-24 | Drop the 6 rows (2%) missing `ca` or `thal` | Impute | Keeps every model on the same 297 patients so feature-set comparisons are apples to apples; cost is negligible |
| 2026-09-24 | Collapse target `num` (0 to 4) to binary disease vs none | Keep five classes | Clinical question is presence of disease; severity classes are tiny (13 patients in the smallest) |
| 2026-09-24 | Model excludes `ca` and `thal`; uses the 11 first-line predictors | Include all 13 predictors | Fluoroscopy and thallium come late in the pathway; a referral model must work from the first-line workup. Quantifying what they add is a Part 2 comparison |
| 2026-09-25 | Preprocessing as a scikit-learn `ColumnTransformer` inside a `Pipeline` (scale continuous, one-hot categorical with drop first, pass binary through) | Manual `get_dummies` + scaler | Fitting happens inside the pipeline on training data only, so leakage is impossible by construction |
| 2026-09-25 | 80/20 stratified split, `random_state=42` | 75/25 (hint sheet default) | More training rows for a 297-patient dataset; stratification keeps 46% prevalence in both halves |
| 2026-09-25 | Initial model: Random Forest (100 trees) | Logistic regression first | Captures non-linear effects and interactions; a baseline and logistic regression are queued for Part 2 |
| 2026-09-25 | `GridSearchCV` scored on disease recall (5-fold stratified) | Accuracy (hint sheet default) | A missed diagnosis is the costly error. Note: the tuned model did not beat the initial one on the 60-patient test set (recall .714 vs .750); threshold tuning is the Part 2 lever |
| 2026-09-24 | No forward-fill (contrary to the hint sheet example) | `fillna(method='ffill')` | Rows are independent patients; forward-fill would copy one patient's results into another's. Not needed in any case after the six rows were dropped |

## Part 2

| Date | Decision | Alternatives considered | Rationale |
|---|---|---|---|
| 2026-10-14 | Pool all four hospital files (920 patients) and keep the 11 first-line predictors | Train on Cleveland only and test on the other three hospitals | Answers Part 1 next step 3 with a larger sample and a test set three times the size (184 vs 60). Every hospital appears in both training and test data, so per-site results are not a fully external test |
| 2026-10-14 | Fill missing values within each hospital (median for continuous, mode for coded) | Drop incomplete rows; fill with pooled values | Dropping rows would remove most non-Cleveland patients; within-site values keep one hospital's norms from being imposed on another. Known limitation: the fill happens before the train-test split, so scores may be slightly optimistic |
| 2026-10-14 | Leave cholesterol values recorded as 0 (all 123 Swiss patients, 49 VA patients) as recorded | Recode zeros as missing | Team chose to keep the original pipeline. Documented as a limitation in Sections 3, 8.6, and 9: the zeros can let the model partly identify the two high-prevalence hospitals |
| 2026-10-14 | Tune every model on ROC-AUC, then tune the decision threshold separately | Tune on recall at 0.50 (Part 1 approach) | Recall at a fixed cutoff mixes ranking quality with cutoff choice; ROC-AUC measures ranking on its own (Part 1 next steps 1 and 2) |
| 2026-10-15 | Compare a majority-class baseline and default-settings scores before tuning | Report tuned scores only | Sets the floor (AUC .50) and shows how much tuning adds (Random Forest +.015, XGBoost +.040) |
| 2026-10-15 | Add XGBoost as a fourth candidate in the same tuning loop | Separate XGBoost cell | One loop and one comparison table for all four models |
| 2026-10-15 | **Keep the Random Forest as the final model** | Switch to XGBoost (CV AUC .8719 vs .8716) | The .0003 lead is smaller than the variation between cross-validation folds. Switching would have replaced the model behind every completed result in Sections 7.3 to 8. The Random Forest also needed less tuning (default AUC .857 vs .832). The tuning cell prints XGBoost as the top scorer and then the team's final choice, so the decision is visible |
| 2026-10-14 | Decision threshold 0.42, chosen to maximize cross-validated F1 for the disease class | Default 0.50; a fixed recall target | On the test set it cut missed diagnoses from 10 to 7 of 102 at a cost of 9 extra false alarms |
| 2026-10-15 | Report results by sex, age band (45 and under, 46 to 60, over 60), and hospital | Overall metrics only | Part 1 promised group-level checks. Younger diseased patients are missed most often, and healthy patients over 60 are flagged most often |
| 2026-10-14 | `logging.basicConfig(..., force=True)` | Default `basicConfig` | Colab configures logging before the notebook runs; without `force=True`, every `logger.info` message is hidden |
| 2026-10-15 | Present the model as a prioritization aid, not a rule-out test | Rule-out use | 12% of patients in the low-risk tier (below 0.42) still had disease |
