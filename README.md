# Gold Futures: Market Conditions and Price Forecasting

## 1. Introduction

### 1.1 Motivation
I chose gold because its price is followed by many investors and businesses, and because I wanted to work with real financial data that changes every day. In the ten years of data I use, the gold futures price went from about 1,128 USD to more than 5,300 USD, with some calm periods and some very turbulent ones. This made me curious: are there different types of market behaviour in the gold price, and can past prices help to forecast future prices?

For a business, these questions are useful. Knowing when the market is calm or nervous can help with planning and risk management. Testing whether a forecasting model really works is also important, because a weak forecast can lead to bad decisions.

### 1.2 Problem Domain
This project is about financial time-series analysis. A time series is a set of values recorded in time order, here one gold price per trading day. Gold prices change over time: they can show long-term trends, short-term ups and downs, calm and volatile periods, and unusual days with very large moves. Many forecasting models, like ARIMA, need the series to be stationary, which means its average and variability do not change over time (Iqbal, n.d.b). Stock and commodity prices are often not stationary, so they need careful preparation.

I use two types of methods from the module. First, clustering, which is unsupervised learning: it groups similar observations without using any labels (Iqbal, n.d.a). I use it to group trading days into market conditions. Second, time-series forecasting with ARIMA, to test if the past gold price can forecast the future price (Iqbal, n.d.c).



### 1.3 Objectives
The objectives of this project are to:
1. Prepare and explore ten years of daily gold futures data, and check its quality.
2. Create features that describe daily market behaviour, such as daily returns and volatility.
3. Group the trading days into market conditions with two clustering algorithms: K-Means and Agglomerative hierarchical clustering.
4. Compare the two clustering results with the silhouette score and the Davies–Bouldin index, and choose a preferred solution.
5. Analyse the gold price over time (trend, stationarity, seasonality and unusual movements) and test whether an ARIMA or SARIMA model is suitable.
6. Evaluate the forecasts on data the model has not seen, compare them with a simple naïve forecast, and explain the strengths and limitations of the model.



### 1.4 Dataset Rationale
The data comes from Yahoo Finance for the ticker **GC=F** (Yahoo Finance, 2026). This is the price of **gold futures** traded on the COMEX exchange, in US dollars. It is not the price of physical gold in shops, but it is a widely used price for the gold market. I downloaded the data with the `yfinance` library on 21 October 2026, as shown in the course.

The file has 6,561 trading days from August 2000 to October 2026, with no missing values. Because the assessment asks for data from the last ten years, I use the period from **21 October 2016 to 20 October 2026**, which gives **2,513 trading days**. I removed the last day because it was downloaded while the market was still open. The original columns are Date, Open, High, Low, Close and Volume. Close is the main variable for my analysis.

I think this dataset is suitable because it has many daily observations, it is free and reliable, and it contains both calm and very volatile periods, which is useful for clustering and for testing forecasts.

**Points to confirm:**
- [Yahoo Finance ticker](https://finance.yahoo.com/lookup/) The assessment brief names the Yahoo Finance ticker **CSV**, but I use **GC=F**. I need to confirm with my lecturer that this dataset is accepted.
- [Gold Dataset](https://finance.yahoo.com/quote/GC=F/) The original data has 6 columns including Date. I created 4 more features (log return, daily range, 20-day volatility and 20-day return). I need to confirm if these count for the seven-column requirement.
