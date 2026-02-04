# ab-testing-MobileGame

This project analyzes an A/B test from a mobile game (Cookie Cats) to evaluate whether **moving the first gate from level 30 to level 40** affects player engagement and retention. The notebook walks through data loading, EDA, statistical testing (proportions and means), and two complementary inference approaches: **delta (two‑proportion z‑test)** and **bootstrap**.

> **Key insight**
>
> *Day‑1 retention* shows **no statistically significant difference** between variants, while *Day‑7 retention* shows a **statistically significant** difference between variants under the bootstrap analysis (95% CI does **not** include 0).

---

## 📦 Dataset

The data contains user‑level records with at least the following fields (inferred from notebook code and EDA):

- `version` — experiment group indicator: **A** (control, gate at level 30) vs **B** (treatment, gate at level 40)
- `sum_gamerounds` — total gameplay rounds in the observation window
- `retention_1` — indicator for returning on **Day‑1** after install (0/1)
- `retention_7` — indicator for returning on **Day‑7** after install (0/1)
- (Other helper fields in SQL summaries: `label`, `cnt`, `game`, `userid`)

Data are loaded in the notebook (via `gdown`/CSV or from a small `sqlite3` demo).

---

## 🎯 Experiment Design

- **Objective:** Does moving the first “gate” from level 30 → 40 improve engagement/retention?
- **Primary metrics:** Binary indicators`retention_1` (Day‑1) and `retention_7` (Day‑7)
- **Secondary metric:** `sum_gamerounds` - Total gameplay rounds
- **Composite Metric:** Combined Day1 and Day7 rentention
- **Hypotheses (for retention):**
  - H0: p_A = p_B
  - H1: p_A ≠ p_B (two‑sided)

---

## 🔍 Methods

### 1) Exploratory Data Analysis (EDA)
- Inspect distributions and summary statistics for `sum_gamerounds`, `retention_1`, `retention_7`
- Compare A vs B across metrics (tables and plots)

### 2) Inference for Proportions (Retention)
- **Delta method / two‑proportion z‑test**: pooled‑variance normal approximation for \(p_1 - p_2\).
- **Bootstrap**: non‑parametric resampling to form the sampling distribution of the difference in means for `retention_1` and `retention_7`, with a 95% CI.

**Reported bootstrap CIs (from the notebook):**
- **Day‑1 retention:** 95% CI ≈ **[-0.002, 0.012]** → *not significant* (CI includes 0)
- **Day‑7 retention:** 95% CI ≈ **[0.008, 0.022]** → *significant* (CI excludes 0)

### 3) Inference for Means (Engagement)
- Compare `sum_gamerounds` between A/B (visualization + summary; parametric or non‑parametric test options depending on distributional checks)

> The notebook also contrasts **parametric** vs **non‑parametric** choices (e.g., when normality/variance assumptions are shaky).

---

## ✅ Results (at a glance)

- **Day‑1 retention:** No statistically significant difference between A and B.
- **Day‑7 retention:** Statistically significant difference between A and B (bootstrap 95% CI excludes 0).
- **Gameplay rounds (`sum_gamerounds`):** Similar at high level; see notebook for plots and group summaries.

**Interpretation:** Moving the first gate to level 40 **did not hurt early (Day‑1) retention** and shows **meaningful change by Day‑7**. This suggests player experience is not negatively impacted in the short term and might support better medium‑term engagement/retention dynamics.

---

## 🧱 Repository Structure

```
├── A_B_Testing_【Python】.ipynb     # Main analysis notebook (EDA + tests + bootstrap)
├── README.md                        # You are here
└── data/                            # (Optional) If you store CSV locally
```

> The notebook also includes small **SQLite** snippets for aggregate summaries.

---

## 🛠️ Environment & Dependencies

The notebook uses the following major libraries (as detected in code):
- `pandas`, `numpy`
- `matplotlib`, `seaborn`
- `scipy`, `statsmodels`
- `sqlite3`, `gdown`
- `tqdm`, `IPython.display`

Install (minimal):
```bash
pip install pandas numpy matplotlib seaborn scipy statsmodels tqdm gdown
```

---

## 🚀 How to Run

1. Clone the repo and open the notebook:
   ```bash
   git clone https://github.com/yourusername/ab-testing-cookiecats.git
   cd ab-testing-cookiecats
   jupyter notebook A_B_Testing_【Python】.ipynb
   ```
2. (If pulling from Drive) Ensure the download step (via `gdown`) succeeds or place the CSV locally and update the load path.
3. Run cells in order; figures and tables will be generated inline.

---

## 📚 Notes & Best Practices

- Ensure test horizon and **sample size** are sufficient; avoid peeking too early.
- Prefer **two‑sided** tests unless you have strong prior reason to use one‑sided.
- For binary metrics and large samples, two‑proportion **z‑test** is standard; **bootstrap** is a robust non‑parametric cross‑check.
- Report **effect sizes** with **confidence intervals**, not only p‑values.
- Guard against multiple‑testing if analyzing many metrics/segments.

---

## 🧭 Roadmap (Optional Enhancements)

- Add a minimal **CLI** to reproduce the bootstrap analysis from CSV
- Add **power & sample‑size** calculator for retention deltas
- Extend to **sequential testing** or **Bayesian** A/B analysis
- Automate **data validation** and **outlier handling**

---

## 📜 License

