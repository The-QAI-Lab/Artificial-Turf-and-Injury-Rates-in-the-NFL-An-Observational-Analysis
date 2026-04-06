# Findings Report

**Project:** DATA 720 Final Project — NFL Injury Analysis  
**Audit Date:** 2026-04-05  

---

## Project Overview

This project investigates whether artificial turf is associated with a higher injury rate than natural grass in NFL games. The dataset is the publicly available **NFL Playing Surface Analytics** dataset from Kaggle (`sudhanshu727/nfl-playing-surface-analytics`), which contains play-level records and injury records for NFL players.

**Research question:** Do artificial turf surfaces produce higher injury rates than natural grass in NFL games?

**Hypothesis:** Artificial turf will be associated with a higher injury rate than natural grass.

**Why it matters:** Player safety is a major concern in professional football. Turf type is a controllable environmental factor, and if artificial surfaces are demonstrably more dangerous, that has direct implications for stadium design, player contracts, and league policy. The question is also well-suited to a data science course project because it requires data loading, merging, cleaning, rate calculation, visualization, and a formal statistical test — all core skills in the DATA 720 curriculum.

---

## Files Reviewed

| File | Role |
|------|------|
| `data720 Final Project Proposal_Analysis Notebook.ipynb` | Main deliverable. Combined proposal + analysis notebook. Contains all code, markdown, and embedded outputs. |
| `kaggle.json` | Kaggle API credentials used by `kagglehub` to download the dataset at runtime. |

**Dataset files (downloaded at runtime):**
- `PlayList.csv` — 267,005 rows × 14 columns. Play-level records with surface type (`FieldType`), position, stadium type, weather, and play type.
- `InjuryRecord.csv` — 105 rows × 9 columns. Injury events with body part, surface label, and days-missed severity flags (`DM_M1`, `DM_M7`, `DM_M28`, `DM_M42`).
- `PlayerTrackData.csv` — Large player-tracking file. Intentionally skipped in this analysis.

---

## What Appears Complete

The following analysis steps are fully implemented and have verified cell outputs embedded in the notebook:

1. **Environment setup** — Auto-installs missing packages (`kagglehub`, `pandas`, `numpy`, `seaborn`, `matplotlib`, `scipy`). Confirmed working (cell 1 output: `"Environment ready."`).

2. **Dataset download** — Uses `kagglehub.dataset_download()` to pull the dataset. Confirmed working (cell 2 output shows the cache path and lists the three CSV files).

3. **Helper functions** — Three well-written utility functions:
   - `find_file()` — Fuzzy file discovery by name.
   - `find_col()` — Fuzzy column detection, case/spacing insensitive.
   - `standardize_surface()` — Maps raw surface labels to `"Artificial Turf"`, `"Natural Grass"`, or `"Other / Unknown"`.

4. **Data loading** — Both `PlayList.csv` (267,005 × 14) and `InjuryRecord.csv` (105 × 9) load correctly. Column names confirmed.

5. **Surface standardization** — `FieldType` column detected automatically. `SurfaceGroup` column created. The dataset only contains `"Synthetic"` and `"Natural"` values, which map cleanly to `"Artificial Turf"` and `"Natural Grass"` with no `"Other / Unknown"` entries.

6. **Merge** — Left join on `PlayKey`. Merged shape: 267,006 × 23. (One extra row vs. the 267,005 in `PlayList.csv` — likely a duplicate `PlayKey` in the injury table. This is a minor anomaly worth noting but does not materially affect results.)

7. **Injury flag** — Binary `Injury` column created from `BodyPart`, `DM_M1`, `DM_M7`, `DM_M28`, `DM_M42`. Result: 77 injured plays, 266,929 non-injured plays.

8. **Quality checks** — Surface category counts and missing value checks confirmed. No missing values in `SurfaceGroup` or `Injury`.

9. **Injury rate summary table** — Computed and displayed correctly (see Analytical Findings below).

10. **Primary bar chart** — "Injury Rate by Playing Surface" bar chart generated and displayed.

11. **Chi-square test** — Contingency table built and `chi2_contingency()` applied. Results displayed.

12. **Optional position-level analysis** — Injury rate by position and surface computed and visualized. Grouped bar chart generated.

13. **Proposal/report markdown** — Sections present: Overview, Research Question, Hypothesis, Planned Analysis, Expected Deliverables, Setup Notes, Dataset Description, Data Preparation, Initial Quality Checks, Main Analysis, Statistical Test, Interpretation Template, Optional Supporting Analysis, Proposal Summary, Conclusion, AI Use Disclosure.

---

## What Is Still Missing

Despite the strong foundation, the notebook has several gaps that must be addressed before it qualifies as a final report:

### Critical gaps

1. **Interpretation Template is not filled in.** Cell `1453902c` contains a placeholder:
   > "The injury rate on artificial turf was **X%** ... The chi-square test produced a p-value of **P** ..."
   
   The actual numbers are available in the notebook outputs directly above this cell. This placeholder must be replaced with the real values.

2. **No written Results section.** The notebook has a "Proposal Summary" and a "Conclusion" but neither contains a clear, written statement of the findings in plain language. A final report needs a dedicated Results section that states what was found.

3. **No Discussion section.** There is no interpretation of what the results mean, no acknowledgment of limitations, and no connection to the broader literature or real-world implications.

4. **No Introduction section.** The "Overview" cell reads like a proposal, not a polished introduction. It should be revised to frame the completed study, not the planned one.

5. **No Methods section.** The notebook describes what was done in scattered markdown cells, but there is no consolidated Methods section explaining the analytical approach, the unit of analysis, how the injury flag was constructed, and why chi-square was chosen.

6. **Notebook structure does not match the expected report format.** The expected structure is: Introduction → Data Analysis → Methods → Results → Discussion. The current structure is a mix of proposal language and analysis cells without clear section boundaries.

### Minor gaps

7. **The merged shape anomaly (267,006 vs. 267,005) is not explained.** One extra row appears after the left join. This is likely a duplicate `PlayKey` in `InjuryRecord.csv` and should be investigated and noted.

8. **The injury flag counts 77 injuries, but `InjuryRecord.csv` has 105 rows.** The discrepancy (28 injury records that did not match any play) is not acknowledged. This could be due to `PlayKey` values in the injury table that don't exist in the play table, or it could indicate a data quality issue worth mentioning.

9. **The optional position analysis is labeled "not required for the proposal."** For a final report, this section should either be integrated as a supporting finding or explicitly framed as supplementary.

10. **No saved figures or outputs.** The `figures/` and `outputs/` directories are empty. For a final submission, saving key figures as files is good practice.

11. **`readme.md` is empty.** Should contain at minimum a brief project description and instructions for running the notebook.

---

## Technical Validation

**Notebook execution status:** The notebook was not re-executed in this audit environment (Jupyter is not available in the shell). However, all cells have embedded outputs from a prior successful run, and the outputs are internally consistent. The following observations are based on those embedded outputs.

| Check | Status |
|-------|--------|
| Imports resolve | ✅ All packages imported successfully in prior run |
| Dataset downloads | ✅ `kagglehub` download confirmed (path shown in output) |
| CSV files found | ✅ `PlayList.csv`, `InjuryRecord.csv`, `PlayerTrackData.csv` discovered |
| Column detection | ✅ `FieldType` detected as surface column; `PlayKey` detected as merge key |
| Surface standardization | ✅ All values map to `"Artificial Turf"` or `"Natural Grass"` |
| Merge | ✅ Shape 267,006 × 23 (minor anomaly: 1 extra row vs. source) |
| Injury flag | ✅ 77 injured, 266,929 not injured |
| Missing values | ✅ Zero missing in `SurfaceGroup` and `Injury` |
| Summary table | ✅ Computed correctly |
| Bar chart | ✅ Rendered |
| Chi-square test | ✅ Computed correctly |
| Position analysis | ✅ Computed and rendered |

**Potential issues to verify on next run:**
- The `kaggle.json` file is present in the project root. Confirm it is also placed at `~/.kaggle/kaggle.json` (or that `kagglehub` can find it), otherwise the download step will fail on a fresh machine.
- The notebook was run on a Linux machine (`/home/will/...` in the output path). On macOS, the cache path will differ but the logic should still work.

---

## Analytical Findings

The notebook ran successfully and produced the following results:

### Injury counts and rates by surface

| Surface | Total plays | Injuries | Injury rate |
|---------|-------------|----------|-------------|
| Artificial Turf | 110,104 | 41 | 0.0372% |
| Natural Grass | 156,902 | 36 | 0.0229% |

Artificial turf had a **62% higher injury rate** than natural grass (0.0372% vs. 0.0229%).

### Chi-square test

- **Contingency table:** Artificial Turf: 110,063 non-injured / 41 injured; Natural Grass: 156,866 non-injured / 36 injured.
- **Chi-square statistic:** 4.1025
- **p-value:** 0.042819
- **Degrees of freedom:** 1

**Interpretation:** The p-value (0.043) is below the conventional 0.05 threshold. The null hypothesis — that injury occurrence is independent of surface type — is **rejected at the 5% significance level**. The evidence supports the hypothesis that artificial turf is associated with a higher injury rate than natural grass.

### Key visualization

A bar chart titled "Injury Rate by Playing Surface" clearly shows the higher rate on artificial turf. The chart is clean and readable.

### Optional position-level analysis

The position-level breakdown shows that the turf effect is not uniform across positions. Defensive backs (DB), linebackers (LB, ILB), and running backs (RB) show the highest injury rates on artificial turf. However, this analysis is exploratory and not formally tested.

### Caveats (not currently in the notebook)

- The injury rate is very low overall (~0.03%), and the absolute number of injuries is small (77 total). The chi-square result is statistically significant but the effect size is modest.
- The unit of analysis is a play-player record, not a unique player or game. This means players who appear in many plays contribute more to the denominator, which is appropriate for a rate calculation but should be stated explicitly.
- The dataset covers a specific time window (not stated in the notebook — worth adding).
- Confounders (e.g., weather, stadium type, play type) are not controlled for.

---

## Submission Readiness

**Overall assessment: Partially ready.**

The data pipeline, analysis, and statistical test are solid and produce credible results. The notebook runs end-to-end and the core findings are correct. However, the notebook reads like a proposal + analysis draft, not a polished final report. The interpretation placeholder is unfilled, and the report is missing a proper Results section, Discussion, and Introduction.

| Dimension | Assessment |
|-----------|------------|
| Data pipeline | ✅ Strong — robust, reproducible, well-commented |
| Analysis correctness | ✅ Correct — rates, chi-square, and position breakdown all look right |
| Visualization | ✅ Adequate — primary bar chart is clear; position chart is a bonus |
| Statistical test | ✅ Appropriate — chi-square is the right test for this question |
| Reproducibility | ✅ Good — auto-installs packages, downloads data at runtime |
| Code quality | ✅ Good — helper functions, clear variable names, comments |

---

## Recommended Next Steps

### Required

1. **Fill in the Interpretation Template.** Replace the `X%`, `Y%`, and `P` placeholders with the actual values from the notebook outputs:
   - Artificial turf injury rate: **0.037%**
   - Natural grass injury rate: **0.023%**
   - p-value: **0.043**
   - Conclusion: reject the null hypothesis; evidence supports the hypothesis.

2. **Write a Results section.** Add a new markdown cell (or expand the Interpretation Template) that states the findings in plain language, including the injury counts, rates, chi-square statistic, p-value, and a one-sentence conclusion.

3. **Write a Discussion section.** Address:
   - What the results mean in plain language.
   - Limitations: small absolute injury count, no confounder control, unit of analysis is play-player records.
   - Whether the finding is practically significant, not just statistically significant.
   - Possible next steps (e.g., controlling for play type or weather).

4. **Revise the Introduction/Overview.** Change the proposal-style language ("This project will...") to past tense ("This project analyzed..."). Frame it as a completed study.

5. **Add a Methods section.** Consolidate the scattered data preparation notes into a single Methods cell that explains: dataset source, merge strategy, how the injury flag was constructed, and why chi-square was chosen.

6. **Investigate the merge anomaly.** The merged table has 267,006 rows vs. 267,005 in `PlayList.csv`. Check for duplicate `PlayKey` values in `InjuryRecord.csv` and note the finding.

7. **Explain the 77 vs. 105 injury discrepancy.** `InjuryRecord.csv` has 105 rows but only 77 matched plays. Identify the unmatched records and note whether they affect the analysis.
