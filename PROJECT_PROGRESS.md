# Project Progress Log: Gold Futures (GC=F)

Legend: ✅ verified from actual files or outputs · 🔶 proposed or not yet verified · ⚠️ open issue

---

## Current position
- **Current stage:** 1. Inspect the CSV structure and correct the data-loading process.
- **Next planned step:** add a cell that prints the first raw lines of `Gold_Dec_26.csv`, so the notebook documents *why* `skiprows=[1, 2]` is needed. Then move to Stage 2 (verify columns, types and dates).

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

### Decisions
| Decision | Status | Reason |
|---|---|---|
| Keep `skiprows=[1, 2]` + rename `Price` → `Date` | ✅ confirmed | Matches the actual file layout |
| Use K-Means + Agglomerative (Ward) | 🔶 proposed | Both taught (Lecture 1, Lecture 2, Tutorials 1–2) |
| ARIMA via Box–Jenkins; SARIMA only with evidence | 🔶 proposed | Lectures 5–6, Tutorials 5–6 |
