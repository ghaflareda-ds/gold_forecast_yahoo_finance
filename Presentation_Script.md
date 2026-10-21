# Five-minute presentation script: CSV market conditions and price forecasting

**How to use this:** read it aloud a few times, then speak from the poster in your own words. The spoken part is about 630 words, roughly 4½ minutes at a relaxed pace (about 140 words a minute), which leaves a safety margin under the 5-minute limit. Time yourself once and cut a sentence if you run over. The bracketed notes tell you what to show on screen. Record in your own voice.

---

### 0:00 – 0:35 · Problem and motivation  *(show: poster title and section 1)*

Hello, my name is [name]. My project looks at Carriage Services, a US funeral and cemetery company listed on the New York Stock Exchange under the ticker CSV.

Stock data is interesting for this module because every trading day is a set of measurements we can cluster, and the sequence of days is a time series we can forecast. So my research question has two parts: first, can trading days be grouped into meaningful market conditions? And second, can an ARIMA model forecast the closing price better than a simple naïve forecast?

### 0:35 – 1:05 · Dataset and preparation  *(show: section 2, stat boxes and price chart)*

The data comes from the Yahoo Finance page for CSV, downloaded with yfinance, the library used in our module. I saved a dated snapshot so the results are reproducible. It covers 2,449 trading days, from January 2017 to September 2026, so it is within the last ten years.

There were no duplicates, missing values or invalid prices, so no rows were removed, and I checked three rows against the website. Because the price columns are almost perfectly correlated, I created four behaviour features: daily log return, intraday range, 20-day volatility and relative volume.

### 1:05 – 2:10 · Clustering  *(show: section 3, selection chart, table, coloured price chart)*

I compared K-Means and agglomerative clustering, both from the course. The features were standardised first, because both methods use Euclidean distance.

For k from 2 to 10 I looked at the elbow, the silhouette score and the Davies–Bouldin index. One lesson was that single, complete and average linkage scored a silhouette of up to 0.87, but only by splitting off one to ten extreme days. So a high score doesn't automatically mean useful clusters. I used Ward linkage instead.

I chose k = 3. With four or more clusters, tiny outlier groups appear, and at three both algorithms separate calm days, busy high-volume days and high-volatility days.

Ward had the better silhouette, 0.31 against 0.26. K-Means had the better Davies–Bouldin index, 1.39 against 1.48, and was far more stable when I removed 20% of the data. So I prefer K-Means. A nice finding is that the volatile days cluster in 2018, 2020, 2022 and 2023, even though the algorithm never saw any dates.

### 2:10 – 3:25 · Time series and ARIMA  *(show: section 4, ACF/PACF and forecast chart)*

For forecasting, the target was the daily closing price. I kept the last six months, April to September 2026, as a test set, so the model is only tested on future data.

The ADF test gave a p-value of 0.59 for the price, so it is not stationary; after one difference it was stationary, so d equals 1. The ACF and PACF of the differenced series were basically flat, and there were no spikes at weekly, monthly or yearly lags, so I did not use SARIMA, because there was no evidence of seasonality.

An AIC search chose ARIMA(2,1,2), while BIC chose ARIMA(0,1,0), which is a random walk. The residuals had no autocorrelation left, but they had heavy tails and changing variance.

### 3:25 – 4:20 · Results and limitations  *(show: forecast table and section 5 chart)*

On the test set, ARIMA's one-day-ahead error was 0.684 dollars, and the naïve forecast, "tomorrow equals today", was 0.679. Over six months it was 7.13 against 7.08. So ARIMA did not beat the naïve forecast. To check this wasn't just one unlucky period, I repeated the test on four six-month windows. ARIMA won two and lost two, always by less than one cent.

My conclusion is that this price behaves close to a random walk. The limitations are that it's one stock with no external data like news, the choice of k is partly a judgement, and the clusters are only weakly separated.

### 4:20 – 5:00 · Conclusion  *(show: section 6)*

To summarise: clustering found three understandable market conditions, and K-Means was the more reliable solution. The forecasting showed honestly that ARIMA can't beat a naïve forecast for this stock.

The two parts connect: the clusters show that volatility changes over time, and that is exactly the assumption ARIMA breaks. Future work could try volatility models or add market data. Thank you.

---

## Likely lecturer questions and model answers

**1. Why did you use derived features instead of the original columns for clustering?**
Open, High, Low, Close and Adj Close correlate at 0.99–1.00, so they all measure the price level. Clustering on them would just group days by date. The derived features describe behaviour (movement, range, volatility, activity) and are on comparable scales.

**2. Why did you scale the data?**
K-Means and Ward both use Euclidean distance. Relative volume varies about 20 times more than daily returns, so without scaling it would dominate the distances. StandardScaler gives every feature a mean of 0 and a standard deviation of 1, as in Tutorial 3.

**3. What is the difference between the silhouette score and the Davies–Bouldin index?**
Silhouette is calculated for every point: (b − a) / max(a, b), where a is the average distance to its own cluster and b to the nearest other cluster. It ranges from −1 to 1, and higher is better. Davies–Bouldin works with centroids: for each cluster it takes the worst ratio of (spread + spread) to the distance between centroids, then averages; lower is better. They can disagree, as they did here, and both can be fooled by outlier clusters.

**4. Why k = 3 when the silhouette score was highest at k = 2?**
k = 2 was better on silhouette and the dendrogram; k = 3 was better on Davies–Bouldin for both algorithms. At k = 2 the two algorithms split on different things (K-Means on volatility, Ward on volume), while at k = 3 both capture both. k ≥ 4 creates clusters under 1% of days. I state in the notebook that k = 2 is also defensible.

**5. Why is K-Means preferred if Ward has the higher silhouette?**
Because I didn't judge on one metric. K-Means has the better DBI, and much better stability (ARI 0.877 vs 0.476 on 80% samples). It is also easier to interpret and every cluster is at least 7% of days.

**6. How did you choose p, d and q?**
d = 1 from the ADF test (the level is non-stationary, the first difference is stationary). The ACF/PACF were flat, suggesting p = q = 0. Then a grid search over p, q = 0–3 on the training data: AIC chose (2,1,2) and BIC chose (0,1,0). Lecture 5 uses minimum AIC but also prefers the simpler model when the difference is small, so I evaluated both.

**7. Why no SARIMA?**
There was no evidence of seasonality: no significant ACF values at lags 5, 10, 21, 63, 126 or 252, and the monthly and weekday boxplots overlap. Fitting seasonal terms without evidence would add parameters for a pattern that isn't there.

**8. Why a chronological split and not a random one?**
In forecasting the model must never learn from the future. A random split would let it see days after the ones it predicts (data leakage) and make it look better than it really is.

**9. Did you fit the model on the test data for the rolling one-step forecast?**
No. The coefficients were estimated only on the training data and then kept fixed. For each test day the model uses actual prices up to the previous day, which is how a forecast would be used in practice.

**10. Your ARIMA model lost to the naïve forecast. Isn't that a failed model?**
It's a valid finding. After differencing there is almost no autocorrelation, so there is nothing for ARIMA to exploit, and the coefficients of ARIMA(2,1,2) nearly cancel out. I checked four test windows and the result held. Reporting this honestly is better than tuning until a score looks good.

**11. What do MAE, RMSE and MAPE tell you here?**
MAE is the average error in dollars (0.68 for one day ahead). RMSE penalises big misses more. MAPE is the error as a percentage of the price (1.7%). MAPE is OK because the prices are far from zero; I wouldn't use it on returns.

**12. Can the clusters be used to improve the forecasts?**
I didn't test that, so I can't claim it. I only showed descriptively that forecast errors were larger on busy and volatile days. Using clusters as model inputs would be future work.

**13. Does the dataset meet the seven-column requirement?**
The source gives seven columns including Date, or six variables without it. I added four derived features with documented formulas, so the analysis uses ten variables plus Date. I flagged this for confirmation because it depends on how the brief counts columns.

**14. What does the heteroskedasticity result mean?**
H = 2.47 with p < 0.01 means the residual variance is not constant: the errors are bigger in some periods. ARIMA assumes constant variance, so its prediction intervals are too wide in calm times and too narrow in volatile times. This matches the volatility clusters.
