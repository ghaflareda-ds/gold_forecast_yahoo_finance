# Assessment Requirements and Working Instructions

*Gold futures project. Machine Learning for Business, HDip in Science in Data Analytics for Business, CCT College Dublin.*
*This file is the reference for the whole project. Do not delete it.*

---

## 1. Assessment domain and data

| Item | Value |
|---|---|
| Proposed instrument | Gold futures, Yahoo Finance ticker `GC=F` |
| Source page | https://finance.yahoo.com/quote/GC=F/history/ |
| Data file | `Gold_Dec_26.csv` (downloaded with `yfinance`, see `yahooFianance.ipynb`) |
| Working notebook | `Gold Forecast.ipynb` |

### ⚠️ Unresolved dataset-compliance issue
The original assessment brief specifies the Yahoo Finance page for ticker **`CSV`** (https://finance.yahoo.com/quote/CSV/history/), which is Carriage Services, Inc. This project uses **`GC=F`** (gold futures), a different ticker and a different type of instrument.

- **Status: NOT approved.** No approval from the lecturer has been seen or recorded.
- **Risk:** if the brief restricts the dataset to `CSV`, a gold-based submission may not meet the dataset requirement.
- **Action needed:** get written confirmation from the lecturer before final submission.
- Never describe the gold dataset as "approved" or as "the dataset specified in the brief" unless this is resolved.

### Other dataset requirements
- The data must come from the permitted source and cover a period **within the last ten years**.
- **Sufficient observations** for clustering and time-series analysis.
- **At least seven columns after cleaning**, with suitable variable types (categorical, continuous, discrete where appropriate).
  - Do **not** assume engineered features automatically satisfy this.
  - Do **not** manufacture columns just to reach seven.
  - The lecturer's interpretation may need to be confirmed.

---

## 2. Marking criteria

| Question | Marks | Requirement |
|---|---|---|
| **Q1** | 15 | Precise introduction: motivation, problem domain, assessment objectives, rationale for the selected dataset. |
| **Q2** | 30 | Select and implement **two clustering algorithms**. Compare them using the **silhouette score** and the **Davies–Bouldin index**, explain how the metrics differ, and justify the preferred solution. |
| **Q3** | 30 | Explore the time series (supported trends, seasonality, anomalies), explain **ARIMA/SARIMA parameter selection**, evaluate forecasting performance, and discuss strengths and limitations. |
| **Q4** | 25 | Interpret and justify findings with suitable visualisations, code comments and explanations, conclusions, **Harvard** citations and references, a **poster**, and a **video presentation of no more than five minutes**. AI-generated voices and agents are **not permitted** in the video. |

- All practical work must be in a **Jupyter Notebook (`.ipynb`)**. Standalone Python scripts are not accepted as a replacement.

---

## 3. Course-first rule
- The uploaded course materials (lectures 1–9, tutorials 1–8, *yahooFianance.ipynb*) are the **primary methodological reference**.
- Use the algorithms, terminology, formulas, libraries and techniques taught in the course wherever appropriate.
- Inspect course files before recommending methods. Never invent course content, page numbers or references.
- Methods not covered by the course must be labelled as extensions and justified.
- **Before suggesting any method or statistic, check that it appears in the course materials.** If it does not, either replace it with the course's own method or mark it clearly as **(extension, not from the course)** with the reason, and let the student decide.
  - Example: skewness/kurtosis are not taught, so the Q-Q plot from Tutorial 6(a) is used instead.

## 4. Notebook documentation rule
**Before** every meaningful code cell, add a Markdown cell explaining:
- what the code does;
- why the operation is necessary;
- which assessment objective it supports;
- why the chosen method is appropriate.

**After** the code has been run, add a Markdown interpretation of the *actual* output where useful.
- Never write interpretations as if code has run when it has not.
- Never invent results, scores, charts, forecasts or citations.

## 5. Way of working (tutoring mode)
- Build the existing notebook step by step. Do **not** replace it, erase work, or generate the complete solution.
- Each step follows the same cycle:
  - **A** explain the task;
  - **B** give the Markdown cell;
  - **C** give one small code cell;
  - **D** the student runs it and shares the output;
  - **E** interpret the real output, then move on.
- Make targeted corrections only. Keep cells that already work.
- Use simple, readable, course-style code with meaningful names and useful comments. No unnecessary complexity.

## 6. Project roadmap (in order, one stage at a time)
1. Inspect the CSV structure and correct the data-loading process.
2. Verify column names, data types, dates and numeric values.
3. Inspect missing values, duplicate rows and invalid observations.
4. Clean the dataset and confirm its actual dimensions.
5. Assess whether the dataset meets the assessment requirements.
6. Engineer justified features where appropriate.
7. Conduct exploratory data analysis with meaningful visualisations.
8. Prepare suitable features for clustering.
9. Implement the first clustering algorithm.
10. Evaluate its results.
11. Implement the second clustering algorithm.
12. Compare the two algorithms (silhouette, Davies–Bouldin, visualisations, interpretation).
13. Explore the gold-price time series.
14. Investigate stationarity, differencing, trends and potential seasonality.
15. Select and justify ARIMA/SARIMA parameters.
16. Fit and evaluate the forecasting model with a chronological train/test split.
17. Interpret results, limitations and conclusions.
18. Prepare the introduction, poster, Harvard references and five-minute presentation materials.

## 7. Academic integrity
- Use only actual data and outputs. No fabricated data, scores, charts, forecasts, citations or findings.
- Explain why methods are chosen; avoid complicated methods added only for appearance.
- Keep the `CSV` vs `GC=F` discrepancy visible until it is resolved.
- Follow CCT College Dublin's rules on disclosing AI assistance.
