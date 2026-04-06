# NFL Injury Analysis: Artificial Turf vs. Natural Grass

**Author:** William McDonald  
**Course:** DATA 720 — Programming Methods for Data Science  
**Institution:** University of North Carolina  

---

## What This Project Is

This project analyzes whether artificial turf surfaces are associated with higher injury rates than natural grass in NFL games. Using publicly available play-level and injury data, it applies data wrangling, exploratory analysis, visualization, and a formal statistical test to answer a concrete, real-world question about player safety.

**Research question:** Are artificial turf surfaces associated with higher injury rates than natural grass?

**Finding:** Yes — artificial turf showed a statistically significant higher injury rate (0.037% vs. 0.023%, χ² = 4.10, p = 0.043).

---

## Why We Did It

Player safety is a persistent concern in professional football. Field surface type is one of the few environmental factors that teams and leagues can directly control. If artificial turf is demonstrably more dangerous, that has direct implications for stadium design, player contracts, and league policy. This project demonstrates how publicly available sports data can be used to test that kind of hypothesis rigorously and reproducibly.

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Python 3 | Core programming language |
| Jupyter / JupyterLab | Interactive notebook environment |
| `kagglehub` | Programmatic dataset download from Kaggle |
| `pandas` | Data loading, merging, and aggregation |
| `numpy` | Numerical operations |
| `scipy` | Chi-square test of independence |
| `seaborn` / `matplotlib` | Visualization |
| Kiro (AI assistant) | Analysis support, report writing, and result verification (see below) |

---

## How AI Was Used

[Kiro](https://kiro.dev) (an AI coding and analysis assistant) was used throughout this project to:

- **Audit the notebook** — Kiro reviewed the notebook end-to-end, verified that the analysis pipeline was correct, confirmed the statistical outputs, and identified gaps in the report structure.
- **Confirm results** — Kiro cross-checked the injury rate calculations, chi-square statistic, and p-value against the embedded cell outputs to ensure consistency.
- **Improve the report** — Kiro rewrote proposal-style language into completed-study framing, added the Results, Discussion, and Methods sections, filled in the interpretation placeholder with actual numbers, and polished the markdown throughout.
- **Generate the companion paper** — Kiro produced `William_McDonald_DATA720_Final_Paper.docx` from the verified notebook findings.

All analysis code, data outputs, and final results were reviewed and verified by the student. AI was used as a tool to accelerate writing and catch errors — not as a source of facts or analysis decisions.

---

## How to Adapt This for Your Own Research

This project is a template for any observational sports (or general) dataset analysis. Here's how to replicate the approach for a different question:

### 1. Find a dataset
Use [Kaggle](https://www.kaggle.com/datasets) or another public data source. Look for datasets that have both an event-level table (e.g., plays, games, transactions) and an outcome table (e.g., injuries, wins, sales).

### 2. Set up your environment
```bash
pip install kagglehub pandas numpy scipy seaborn matplotlib
```
Place your Kaggle API token at `~/.kaggle/kaggle.json`.

### 3. Download the data programmatically
```python
import kagglehub
dataset_dir = kagglehub.dataset_download("username/dataset-name")
```

### 4. Merge and prepare
- Load both tables with `pandas`
- Standardize categorical variables (e.g., surface type labels)
- Merge on a shared key using a left join to preserve all event records
- Create a binary outcome flag from the outcome table

### 5. Compute rates and visualize
```python
summary = df.groupby("GroupColumn").agg(
    total=("Outcome", "size"),
    events=("Outcome", "sum"),
    rate=("Outcome", "mean")
)
```
Plot with `seaborn.barplot`.

### 6. Test for significance
For two categorical variables, use a chi-square test:
```python
from scipy.stats import chi2_contingency
contingency = pd.crosstab(df["Group"], df["Outcome"])
chi2, p, dof, expected = chi2_contingency(contingency)
```

### 7. Interpret carefully
- Report the rate difference and the p-value together
- State whether the result is statistically significant
- Acknowledge that observational data shows association, not causation
- Note any confounders you could not control for

---

## Reproducibility

To run this notebook:

1. Clone or download this project folder
2. Place a valid Kaggle API token at `~/.kaggle/kaggle.json`
3. Open `data720 Final Project Proposal_Analysis Notebook.ipynb` in JupyterLab
4. Run all cells — missing packages are auto-installed on first run
