# Machine Learning in R for Research: a self-guided course

A step-by-step course for people with **no coding experience**. You will
learn what logistic regression, LASSO, random forest, XGBoost and LightGBM
do, apply them to **real public clinical data**, and finish with a
**reusable R Markdown template** for your own research studies.

Every lesson is an R Markdown (`.Rmd`) file that you open in RStudio. Each
one has plain-language concept explanations, code where **every line is
commented**, "check your understanding" questions, and small exercises.

---

## 1. Before you start (one time, ~30 min)

1. Install **R**: <https://cran.r-project.org> (tested with R 4.6.1; any recent version should work).
2. Install **RStudio Desktop**: <https://posit.co/download/rstudio-desktop/>
3. Double-click **`ML_in_R_course.Rproj`**. RStudio opens *inside* this
   folder. **Always open the course this way**, so that file paths work.
4. Open `00_setup_and_R_basics.Rmd` and follow it. It installs the packages.

---

## 2. The course map

| # | File | You will learn | Time |
|---|------|----------------|------|
| 0 | `00_setup_and_R_basics.Rmd` | RStudio, R Markdown, the ~10 R ideas you need | 1–2 h |
| 1 | `01_data_import_and_exploration.Rmd` | Download real data from the web, clean it, Table 1, plots, **train/test split** | 2 h |
| 2 | `02_logistic_regression.Rmd` | Odds ratios vs prediction, the **5-step tidymodels workflow**, cross-validation, ROC/AUC, thresholds, **LASSO** and tuning | 3 h |
| 3 | `03_random_forest.Rmd` | Decision trees, random forests, tuning `mtry`/`min_n`, permutation importance | 2 h |
| 4 | `04_xgboost.Rmd` | Gradient boosting, key hyperparameters, space-filling grids, **SHAP values** | 3 h |
| 5 | `05_lightgbm.Rmd` | LightGBM: leaf-wise boosting, native categorical/missing data | 1–2 h |
| 6 | `06_model_comparison.Rmd` | Bootstrap 95% CIs, paired comparison, **calibration**, **decision curve analysis**, TRIPOD+AI reporting | 3 h |
| 7 | `07_research_project_TEMPLATE.Rmd` | A complete paper-style analysis on a **second** dataset, ready to copy for your own data | 2 h + |

**Run the lessons in order.** Lesson 1 creates `data/`, Lessons 2–5 save
results in `results/`, and Lesson 6 reads them.

### Suggested pace (about 1 lesson per week)

* **Week 1:** Lessons 0–1. **Week 2:** Lesson 2 (the most important: take your time).
* **Week 3:** Lesson 3. **Week 4:** Lessons 4–5. **Week 5:** Lesson 6.
* **Week 6:** Lesson 7, then repeat Lesson 7 with a dataset of your choice
  (see section 5).

---

## 3. How to work through each lesson

1. **Read the concept section first.** Don't run anything yet.
2. Run the code **one chunk at a time** with the green ▶ (`Ctrl+Shift+Enter`).
3. After each chunk, **look at the output** and read the comments again.
4. Answer the **"Check your understanding"** questions out loud or in writing.
5. Do the **"Try it yourself"** exercises. Changing code and seeing what
   breaks is how you learn.
6. Click **Knit** (`Ctrl+Shift+K`) to produce the HTML report.
7. Write your own notes directly in the `.Rmd` text. It's your lab notebook.

**Data used:**

* Lessons 1–6: **Cleveland Heart Disease** (303 patients, predicting coronary
  artery disease). UCI ML Repository, <https://doi.org/10.24432/C52P4X>
* Lesson 7: **Breast Cancer Wisconsin Diagnostic** (569 biopsies, predicting
  malignancy). UCI ML Repository, <https://doi.org/10.24432/C5DW2B>

Both are downloaded **directly from the web** by the code.

---

## 4. Applying this to your own research study

1. **Write the question first** (Background, Objectives, outcome definition,
   predictors) in the template, *before* analysing.
2. **Check your sample size.** The number of *events* (outcome = Yes)
   matters more than the number of patients. With fewer than ~100–200 events,
   prefer logistic/LASSO and expect wide CIs. See Riley et al., *BMJ*
   2020;368:m441, and the `pmsampsize` R package.
3. Copy `07_research_project_TEMPLATE.Rmd` and replace **only** the
   data-import and data-cleaning chunks. Everything else adapts
   automatically as long as you create `study_data` with an `outcome`
   column (`"Yes"`/`"No"`, with `"Yes"` first).
4. **Remove leakage:** no IDs, no dates, nothing measured *after* the
   outcome, nothing derived from the outcome.
5. Report with **TRIPOD+AI** (<https://www.tripod-statement.org>):
   discrimination, calibration, clinical utility, uncertainty, code.
6. Remember **internal validation ≠ external validation**. A model is only
   proven when tested in new patients (another hospital or time period).

**Is your outcome a number (e.g. blood pressure) instead of yes/no?** Use
`set_mode("regression")`, `linear_reg()` instead of `logistic_reg()`, and
metrics such as `metric_set(rmse, rsq, mae)`. Everything else is the same.

**Is your outcome rare (< 10%)?** Accuracy becomes meaningless. Focus on
AUC, PR-AUC (`pr_auc`), calibration and decision curves. Do *not*
automatically up-/down-sample: it distorts calibration (van den Goorbergh et
al., *JAMIA* 2022). Adjusting the threshold is usually better.

---

## 5. Where to find public data for practice and research

| Source | What's there | Notes |
|--------|--------------|-------|
| **UCI ML Repository**: <https://archive.ics.uci.edu> | Hundreds of datasets: heart disease, diabetes readmission (100k encounters, good for LightGBM), chronic kidney disease, heart-failure records | Free, direct download |
| **NHANES (CDC)**: <https://wwwn.cdc.gov/nchs/nhanes/> | US national health and nutrition survey: labs, exams, questionnaires | R package `nhanesA`. Survey weights matter for population estimates |
| **BRFSS (CDC)**: <https://www.cdc.gov/brfss/> | Large US behavioural risk-factor survey (400k+/year) | Good "big data" practice for boosting |
| **PhysioNet**: <https://physionet.org> | ICU and physiological data (e.g. MIMIC-IV demo is open) | Full MIMIC needs free credentialing and training |
| **GEO / ArrayExpress** | Gene-expression (omics) data | Many predictors, few patients: LASSO territory |
| **Kaggle Datasets**: <https://www.kaggle.com/datasets> | Many health datasets | Check the original source and licence |
| **WHO GHO, World Bank, Our World in Data** | Country-level indicators | Good for regression (numeric outcomes) |
| R package **`medicaldata`** | Curated clinical teaching datasets | `install.packages("medicaldata")` |

Always check the **licence**, **citation** and **ethics** requirements, and
cite the dataset in your paper.

---

## 6. Glossary

| Term | Plain meaning |
|------|---------------|
| **Overfitting** | The model memorises the training data (noise included) and does worse on new patients |
| **Training / test set** | Data used to build the model / data hidden until the final, single evaluation |
| **Cross-validation (CV)** | Repeatedly train on part of the training data and evaluate on the rest, to estimate performance without touching the test set |
| **Hyperparameter** | A setting you choose before training (e.g. number of trees, learning rate). Tuned with CV |
| **Data leakage** | Information from the test data (or from the outcome) sneaking into training, which gives over-optimistic results |
| **Recipe** | The list of preprocessing steps (imputation, dummies, scaling), learned on training data only |
| **ROC AUC / C-statistic** | Probability the model ranks a random case above a random non-case (0.5 = chance, 1 = perfect) |
| **Calibration** | Agreement between predicted probabilities and observed frequencies |
| **Brier score** | Mean squared error of predicted probabilities (lower is better) |
| **Sensitivity / specificity** | % of cases correctly detected / % of non-cases correctly cleared |
| **Net benefit / DCA** | Clinical usefulness of using a model to make decisions at a given risk threshold |
| **Bootstrap** | Resampling with replacement to estimate uncertainty (CIs) |
| **SHAP value** | A variable's contribution (+ or −) to one patient's prediction |
| **Permutation importance** | Drop in performance when one variable is randomly shuffled |
| **Events per variable (EPV)** | Number of outcome events divided by the number of model parameters. A rough check of whether you have enough data |
| **External validation** | Testing the final model in a completely different dataset |

---

## 7. Learning resources (all free online)

* **Tidy Modeling with R** (Kuhn & Silge): <https://www.tmwr.org>. The
  reference for the tidymodels code used here.
* **An Introduction to Statistical Learning with R (ISLR2)**:
  <https://www.statlearning.com>. The best conceptual introduction to ML.
* **R for Data Science (2e)**: <https://r4ds.hadley.nz>. For data handling
  and plots.
* **TRIPOD+AI** reporting guideline: Collins et al., *BMJ* 2024;385:e078378.
* Christodoulou et al., *J Clin Epidemiol* 2019: systematic review comparing ML
  with logistic regression for clinical prediction.

---

## 8. Troubleshooting

| Problem | Fix |
|---------|-----|
| `there is no package called 'xxx'` | Run `install.packages("xxx")` in the Console |
| `cannot open file 'data/...'` | Open the course via `ML_in_R_course.Rproj`, and run Lesson 1 first |
| `could not find function "..."` | Run the chunk with `library(...)` at the top of the lesson |
| Results differ slightly from a colleague's | Different package versions or CPU. `set.seed()` fixes randomness, but not every numeric detail |
| Download fails | Check your internet connection. After Lesson 1, a local copy is in `data/` |
| Knit fails but chunks run | Knitting starts from a *clean* session: something you ran in the Console is missing from the file |
| Tuning is slow | Use `v = 5` folds and a smaller `size`/`grid` while learning |

---

## 9. Tested with

All lessons were run from a clean start, in order, in October 2026 with:

| Software | Version |
|----------|---------|
| R | 4.6.1 (Windows) |
| tidymodels | 1.5.0 (parsnip 1.6.1, recipes 1.4.0, tune 2.1.0, yardstick 1.4.0) |
| ranger | 0.18.0 |
| xgboost | 3.2.1.1 |
| lightgbm | 4.7.0 |
| bonsai | 0.4.1 |
| gtsummary | 2.6.1 |
| skimr | 2.2.2 |

R packages change over time. If a lesson breaks after a package update,
please open an issue describing the error and your `sessionInfo()` output.
Results (AUCs, chosen hyperparameters) may differ slightly between package
versions and computers.

---

## 10. How this course was made

The course materials were developed with the assistance of an AI model
(Claude, by Anthropic), then run and tested end to end. Readers should
check the methods against the references provided and seek statistical
advice before using them in a study. The course is provided as-is for
educational purposes.

---

## 11. License

* **Code** (the R code in the `.Rmd` files): [MIT License](../LICENSE).
* **Text and teaching material**: [Creative Commons Attribution 4.0
  (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).
* **Datasets** are **not** included in this repository. The code downloads
  them from the UCI Machine Learning Repository, under the licences listed
  on their UCI pages. Cite the original datasets (references in Lessons 1
  and 7).
