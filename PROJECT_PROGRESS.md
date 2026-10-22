# Project Progress Log: Gold Futures (GC=F)

Legend: ✅ verified from actual files or outputs · 🔶 proposed or not yet verified · ⚠️ open issue

---

## Current position
- **Current stage:** 3. Inspect missing values, duplicate rows and invalid observations.
- **Stage 1 complete ✅:** loading with `skiprows=[1, 2]` confirmed by the notebook output (6,561 rows, numeric types). The student decided not to add a raw-file printout cell or to change Markdown cell [3].
- **Stage 2 complete ✅** (student's notebook output, cells [7] and [9]):
  - Dates run from 2000-08-30 to 2026-10-21, and `is_monotonic_increasing` = True, so they are already in time order.
  - `describe()`: count 6,561 in every column (no missing values). All prices are positive: minimum ≈ 255, maximum Close 5,318.40, maximum High 5,586.20, maximum Open 5,415.70. The price std ≈ 920 reflects the long-term trend.
  - Volume: minimum 0, 25% = 29, median 199, mean 51,318, 75% 87,026, maximum 570,055. Very uneven, probably recorded differently in some periods; still to investigate.
- **Stage 3 complete ✅** (student's notebook output):
  - Missing values: 0. Duplicate dates (`df.index.duplicated()`): 0. An earlier `df.duplicated()` call found 41 *identical rows* on different dates in 2000–2007; these are real days and were kept (mistake corrected).
  - Full 2000–2026 data: High < Low 1, Open outside range 56, Close outside range 430, zero volume 418; all prices > 0.
  - Ten-year filter with fixed dates: `start_date='2016-10-21'`, `end_date='2026-10-20'` (the 21 Oct 2026 download day is excluded as incomplete) → **2,513 rows**. `Timestamp.now()` was replaced so the result is reproducible.
  - Ten-year data: High < Low 0, Open/Close outside range 0, so **all price errors were in old years**. Zero volume: 36 days.
  - **Volume < 1,000 on 810 of 2,513 days**, by year: 2016 41, 2017 223, 2018 228, 2019 224, 2020 86, 2021–2024 ≈ 0, 2025 5, 2026 2. Volume was recorded at a much lower level before about 2020, so the series is **not consistent** over the ten years.
- **Decision (based on this evidence):** Volume is kept as an original column but **not used as a model input** (clustering features), because its recording changes during the period. To be stated as a limitation.
- **Stage 4 complete ✅:** `gold = df_10_years.copy()` has shape **(2513, 5)**. Columns: Close, High, Low, Open, Volume, plus the Date index, from 2016-10-21 to 2026-10-20.
- **Instrument verified ✅** (Yahoo quote page, 21 Oct 2026): "Gold Dec 26 (GC=F)", COMEX, delayed quote, USD, settlement date 2026-12-29. Yahoo labels GC=F with the currently active contract (December 2026), which explains the filename. It is a **futures price**, not a spot or retail gold price. 🔶 That the history is a continuous series rolled over successive contracts, and that prices are in USD per troy ounce, come from general knowledge and were not shown on the page.
- **Stage 5 (requirements check):**
  - Ten-year window ✅ (relative to the 21 Oct 2026 download).
  - 2,513 observations ✅.
  - Seven columns ⚠️: 6 including Date; Adj Close is not provided and would equal Close for futures.
  - Variable types: datetime + continuous + discrete; no categorical.
  - Source ⚠️: GC=F instead of CSV, not approved.
- **Stage 6 in progress:**
  - `Log_Return` = ln(Close_t / Close_{t−1}) added ✅ (first row NaN, as expected).
  - `Daily_Range` = (High − Low) / Close added ✅. The first row is 0 (High = Low).
  - 🔶 A quick check on the CSV, done by Claude outside the notebook, suggests Daily_Range is also affected by the pre-2020 recording problem: 76 zero-range days, all in 2016–2020; median range about 0.5–0.7% in 2017–2019 vs about 1.1–1.6% from 2020. **Not yet verified in the student's notebook.**
- **Next planned step (start of next session):** run the cell `range_by_year` (zero-range days and median range per year). Then decide the clustering features: (a) use only Close-based features (Log_Return + rolling volatility), or (b) shorten the period to the consistent years (from about 2020). After that, add the rolling-volatility feature.

## Session 2 (22 Oct 2026)
- **Daily_Range check ✅** (student's notebook): zero-range days 2016 2, 2017 11, 2018 35, 2019 28, 2020 2, 2021–2026 0. Median range 2017–2019 0.50–0.68%; 2020–2026 1.13–2.29%. High/Low have the same pre-2020 recording problem as Volume.
- **Decision ✅:** cluster only on **Close-based** features (Log_Return, Volatility_20d, Return_20d) over the full ten years. Daily_Range and Volume are kept in the table but excluded from clustering. Reasons: Close is consistent across the period, the time-series part also uses Close, and shortening to 2021+ would lose about 40% of the data.
- **Features added ✅:**
  - `Volatility_20d` = rolling 20-day std of Log_Return; first value 2016-11-21.
  - `Return_20d` = ln(Close_t / Close_{t−20}); 2016-11-21 = −0.0455.
- **Stage 6 complete ✅:** `gold.shape` = **(2513, 9)**, i.e. 5 original + 4 derived columns, plus the Date index. Derived-feature summary:
  - Log_Return: mean 0.00047, std 0.0107, min −0.1141, max 0.0589.
  - Daily_Range: mean 0.0135, min 0, max 0.1643.
  - Volatility_20d: 0.0032–0.0356, median 0.0086.
  - Return_20d: −0.1713 to 0.1997, std 0.0432.
  - Different scales → scaling is needed before clustering.
- EDA charts already made by the student: price + volatility, then price + volatility + Return_20d.
- 🔶 To check in EDA: the −11.4% single-day return and the 16.4% daily range (real event or data artefact?).
- **Correction (course-first rule):** skewness and kurtosis were suggested but are **not taught in the course**, so they were removed. They are replaced by the course method: a histogram plus a **Q-Q plot** (`sm.qqplot(returns, line='s')`, as in Tutorial 6(a)) to check normality and heavy tails.
- **Stage 7 EDA ✅** (student's notebook):
  - Price and volatility charts. Volatility comes in bursts: calm 2017–2019, spikes in early 2020 (~2.9%) and early 2026 (~3.5%).
  - Histogram + Q-Q plot (Tutorial 6(a)): the middle follows the line, both tails depart, so the returns are heavy-tailed. The left tail is the most extreme.
  - Top 10 absolute daily moves are all in 2020–2026. The largest is **30 Jan 2026: −11.4%**, with range 16.4% and volume 508,003 (looks like a real sell-off; cause not researched). It was followed by +5.9% on 3 Feb 2026. The March 2020 big moves have tiny volume but real Close moves, which supports using Close-based features.
  - Correlation of the clustering features (Pearson, Lecture 8 p. 11): Log_Return–Volatility_20d −0.01, Log_Return–Return_20d 0.20, Volatility_20d–Return_20d 0.01, so they carry different information. A heatmap was added (seaborn; a visualisation not taken from the course).
  - Seasonality will be tested formally in Stage 14 with the course methods (`seasonal_decompose`, ACF at seasonal lags).
- **Incident:** cells were deleted by accident and restored from the Jupyter checkpoint (20:10). Advice: commit after each stage.
- **Next planned step:** Stage 8: prepare the clustering features (drop the 20 warm-up rows, then StandardScaler as in Tutorial 3 Solution).

## End of session 1 summary (21 Oct 2026)
- Done: Stages 1–5 (loading, checks, cleaning, ten-year filter giving 2,513 rows, requirements check) and part of Stage 6.
- Open: lecturer approval for GC=F, the seven-column interpretation, and the Daily_Range reliability decision.
- Remaining stages: 6 (finish), 7 EDA, 8 cluster features, 9–12 clustering, 13–16 time series/ARIMA, 17 interpretation, 18 introduction/poster/references/presentation.

---

## Session log

### Session 1 (21 Oct 2026)

**Project folder:** `Documents\GitHub\gold_forecast_yahoo_finance`
- Git repository, no commits yet. All files are untracked.
- A second folder, `Documents\GitHub\Machine_Learning_CA2_2026023`, contains an older copy of the notebook and CSV. That folder was not modified.

**Notebook state, `Gold Forecast.ipynb` (✅ read, not modified)**

| Cell | Content | Output (as saved in the notebook) |
|---|---|---|
| [0] | imports: pandas, numpy, matplotlib | — |
| [1] | `pd.read_csv('Gold_Dec_26.csv')` | Ticker row and Date row appear as data rows |
| [2] | `df.info()` | 6,563 rows, 6 columns, all `str` (text) |
| [3] | Markdown note on the extra rows | — |
| [4] | `read_csv(skiprows=[1, 2])`, rename `Price` → `Date`, `to_datetime`, `set_index` | Correct numeric table |
| [5] | `df.info()` | DatetimeIndex, **6,561 rows**, 2000-08-30 to 2026-10-21; Close/High/Low/Open `float64`, Volume `int64` |

**Download (✅ `yahooFianance.ipynb`)**
- `yf.download("GC=F", period="max")` with default settings.
- No start or end date, and no record of the download date inside the file.

### Verified findings (✅ from reading the raw CSV directly)
- **Raw file layout:** 6,564 lines.
  - Line 0 is the header `Price,Close,High,Low,Open,Volume`.
  - Line 1 is `Ticker,GC=F,...`.
  - Line 2 is `Date,,,,,`.
  - Data starts at line 3.
  - This three-line header is how yfinance saves its two-level column names.
- **The `skiprows=[1, 2]` code is correct:** it removes the Ticker and Date lines and keeps line 0 as the header. Renaming `Price` → `Date` is also correct, because the first header cell holds the dates.
  - Small wording fix: the markdown says "rows 0 and 1". Those are pandas' row numbers in the wrongly-loaded table; in the file they are **lines 1 and 2**, which is why `skiprows=[1, 2]` is right.
- **Columns available:** Date, Close, High, Low, Open, Volume. There is **no Adj Close**: recent yfinance versions adjust automatically by default, and futures pay no dividends anyway.
- **Date range in file:** 2000-08-30 to 2026-10-21.

### Open issues (to handle in later stages)

| # | Issue | Status | Stage |
|---|---|---|---|
| 1 | Dataset ticker `GC=F` differs from the brief's `CSV`; lecturer approval needed | ⚠️ unresolved | all |
| 2 | Data goes back to 2000, beyond the **ten-year** limit; a date filter is needed | ⚠️ | 4–5 |
| 3 | Only **6 columns including Date** (5 numeric). The seven-column requirement is not met by the original data; derived features and the lecturer's interpretation need to be considered | ⚠️ | 5–6 |
| 4 | Last row is **2026-10-21**, the same day as the download (file saved 15:53 Irish time, while the market was trading). It is probably an **incomplete trading day** | 🔶 to verify | 3–4 |
| 5 | Early rows have Volume = 0 and Open = High = Low = Close (no intraday range) | 🔶 check whether this affects the ten-year window | 3 |
| 6 | 2026-10-19 and 2026-10-20 show the **same Volume (123,741)**, which may be a data-quality issue | 🔶 to check | 3 |
| 7 | `GC=F` is believed to be Yahoo's *continuous front-month* COMEX gold futures series (USD per troy ounce), not spot or retail gold. Contract rolls may cause jumps. The filename "Dec_26" may wrongly suggest the December 2026 contract | 🔶 to verify on Yahoo | 2–5 |
| 8 | Download is not reproducible yet (no fixed dates, no recorded download date) | 🔶 | 4–5 |

### Errors resolved
- All columns read as text: caused by yfinance's two extra header lines. Fixed by the student with `skiprows=[1, 2]` ✅.
- Files from an earlier, unrelated stock project (CSV poster, notebook, script, references, `data/`, `figures/`) were copied into this folder by mistake and committed. They were moved to `Documents\GitHub\csv_stock_project` and removed from the unpushed "MarkeDown" commit ✅. This repository now contains gold-project files only.
- `.ipynb_checkpoints/` (Jupyter autosave copies) removed from git tracking and added to `.gitignore` ✅.

### Git / GitHub
- Repository owner: `ghaflareda-ds`. GitHub Desktop was signed in as `2026023DataAnalysis`, which has no write access, so it offered a fork. Fix: sign in to GitHub Desktop as `ghaflareda-ds` (File → Options → Accounts), then push. ⚠️ to be done by the student.

### Decisions
| Decision | Status | Reason |
|---|---|---|
| Keep `skiprows=[1, 2]` + rename `Price` → `Date` | ✅ confirmed | Matches the actual file layout |
| Use K-Means + Agglomerative (Ward) | 🔶 proposed | Both taught (Lecture 1, Lecture 2, Tutorials 1–2) |
| ARIMA via Box–Jenkins; SARIMA only with evidence | 🔶 proposed | Lectures 5–6, Tutorials 5–6 |
