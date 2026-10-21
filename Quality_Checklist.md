# Final quality checklist

**Files in this folder**

| File | What it is |
|---|---|
| `CSV_Stock_Analysis.ipynb` | Deliverable 1: the executed notebook (48 code cells, each preceded by an explanation) |
| `CSV_Poster.pptx` / `CSV_Poster.pdf` | Deliverable 2: A1 landscape poster (editable PowerPoint and print PDF) |
| `References.md` | Deliverable 3: Harvard reference list |
| `Presentation_Script.md` | Deliverable 4: 5-minute script and defence Q&A |
| `Quality_Checklist.md` | Deliverable 5: this file |
| `data/` | Dated data snapshot and download metadata, needed to reproduce the exact results |
| `figures/` | All 18 charts produced by the notebook |

Legend: ✅ met and verified · ⚠️ met but needs your action or confirmation · ❌ not met

## 1. Requirements mapped to evidence

| Requirement | Status | Where |
|---|---|---|
| Dataset from the specified Yahoo Finance page, ticker CSV | ✅ | Notebook steps 2.1 (download) and 2.8 (3 rows match the web page); poster §2 |
| Data within the last ten years | ✅ | Notebook step 2.5: 3 Jan 2017 – 30 Sep 2026, inside the window measured from the download date (21 Oct 2026) |
| At least seven columns after cleaning | ⚠️ | Notebook step 2.9 and section 7.2: 7 columns including Date (6 variables plus Date), plus 4 documented derived features. **Confirm with your lecturer** whether Date and derived features count. |
| Reasonable number of observations | ✅ | 2,449 trading days |
| Cleaning documented, no silent deletion | ✅ | Notebook steps 2.2–2.9: no duplicates, missing or invalid values; 0 rows removed |
| **Part 1:** introduction, motivation, problem, objectives, dataset rationale (15) | ✅ | Notebook section 1; poster §1–2 |
| Feature engineering explained (formula, purpose, limits) | ✅ | Notebook steps 3.1–3.4 (one cell per feature) |
| EDA: trend, variability, anomalies, possible seasonality, correlation | ✅ | Notebook steps 3.5–3.10 |
| **Part 2:** two clustering algorithms from the course (30) | ✅ | K-Means and Agglomerative (Ward); notebook section 4; poster §3 |
| Scaling justified | ✅ | Notebook step 4.2 |
| Parameter selection with evidence (k, linkage) | ✅ | Notebook steps 4.3–4.7: elbow, silhouette, DBI, smallest cluster, dendrogram, k=2 vs k=3 profiles |
| Silhouette and Davies–Bouldin comparison | ✅ | Notebook step 4.12 and the "Preferred clustering solution" sub-section; poster table |
| Cluster interpretation using real characteristics | ✅ | Notebook steps 4.9–4.11 (profiles, scatter plots, timeline) |
| Stability / sensitivity | ✅ | Notebook steps 4.13–4.14 (agreement; ARI across seeds and 80% subsamples) |
| Preferred solution justified with more than one metric | ✅ | Notebook "Preferred clustering solution" sub-section (end of section 4) |
| **Part 3:** time-series exploration, trend, seasonality (30) | ✅ | Notebook steps 5.1–5.6 |
| Stationarity (ADF) and differencing | ✅ | Notebook step 5.3 (d = 1) |
| ACF/PACF and justified p, d, q | ✅ | Notebook steps 5.4 and 5.7 (AIC/BIC grid) |
| SARIMA considered only with evidence | ✅ | Notebook steps 5.5–5.6: no seasonal evidence, so SARIMA not fitted (justified) |
| Model fitting and diagnostics | ✅ | Notebook steps 5.8–5.9 (summary, diagnostic plots, Ljung–Box) |
| Chronological split, out-of-sample evaluation, baselines | ✅ | Notebook steps 5.1 and 5.10–5.13 (naïve and mean baselines, MAE/RMSE/MAPE, 4-window robustness check) |
| In-sample vs out-of-sample distinction | ✅ | Notebook step 5.12 |
| Strengths and limitations | ✅ | Notebook "Strengths and limitations of the ARIMA approach" (end of section 5) |
| **Part 4:** interpretation, visualisations, code comments, conclusions (25) | ✅ | Notebook step 6.1, Discussion, section 7; interpretation after every key output |
| Harvard references | ⚠️ | `References.md`, notebook section 7, poster. **Check the starred book/paper details** and the year format for the lecture slides. |
| Poster | ⚠️ | `CSV_Poster.pptx/.pdf`. **Replace `[Student name]` and `[Student number]`.** Check your college's required poster size; it is built at A1 landscape. |
| Video ≤ 5 minutes in your own voice | ⚠️ | Script is about 4½ minutes. **You must record it yourself.** |
| Notebook reproducible, top to bottom | ✅ | Run twice from a clean kernel; all outputs identical. Instructions at the end of the notebook. |
| No fabricated data, results or references | ✅ | Every number in the notebook, poster and script was copied from executed output |

## 2. Remaining uncertainties and risks

1. **Seven-column rule (most important).** The brief's exact wording wasn't available to me. The dataset has 6 original variables plus Date; I did not invent columns to reach seven. Ask your lecturer before submitting.
2. **Official assessment brief not seen.** I used the requirements and marks in your instructions (15 / 30 / 30 / 25). If the brief has word limits, a template or a poster size, adjust to match.
3. **Choice of k = 3** is a reasoned judgement. k = 2 is also defensible, and this is acknowledged in the notebook.
4. **Forecasting result is negative**: ARIMA does not beat the naïve forecast. This is honest and valid; don't present it as a predictive success.
5. **Extensions beyond the course:** Davies–Bouldin code, ARI and Ljung–Box are marked as extensions in notebook 1.5. Everything else follows the lecture and tutorial methods.
6. **Environment.** Results were produced with Python 3.14, pandas 2.3.3, scikit-learn 1.8.0 and statsmodels 0.15.0. Older library versions may show tiny numerical differences.
7. **Data revisions.** Keep the `data/` folder with the notebook. A fresh download could differ slightly if Yahoo revises its data.

## 3. Academic integrity

These materials were developed with AI assistance (Claude). If CCT College Dublin's assessment policy requires you to disclose AI use, add a declaration as the policy describes, for example in a final notebook cell or your submission form. Before you submit and present, make sure you understand and can explain every step: the defence Q&A in `Presentation_Script.md` is designed to help with that.

## 4. How to reproduce

```
pip install pandas numpy matplotlib seaborn scikit-learn scipy statsmodels yfinance jupyter
jupyter nbconvert --to notebook --execute CSV_Stock_Analysis.ipynb
```

Or open the notebook in Jupyter and choose *Kernel → Restart & Run All*. It takes about one minute.
