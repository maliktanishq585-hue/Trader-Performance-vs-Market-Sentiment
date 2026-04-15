# Project Analysis: Bitcoin Market Sentiment & Trader Behavior

## Overview
This project explores the relationship between Bitcoin market sentiment (Fear & Greed Index) and the performance and behavior of cryptocurrency traders. It involves data preparation, detailed analysis of trading metrics against sentiment, segmentation of traders based on their behavior, and the development of a predictive model for daily PnL.

## Part A: Data Preparation

### Data Loading & Initial Checks
*   **Bitcoin Market Sentiment Data (`fear_greed_index (1).csv`):** Loaded into `fear_greed_df` (2644 rows, 4 columns).
*   **Historical Trader Data (`historical_data.csv`):** Loaded into `historical_trader_df` (211224 rows, 16 columns).
*   **Missing Values & Duplicates:** No missing values or duplicate rows were identified in either dataset.

### Data Alignment & Merging
*   Timestamps in both dataframes were converted to datetime objects.
*   A daily `date` column was extracted for consistent merging.
*   The two datasets were merged on the `date` column, resulting in `merged_df`.

### Key Metrics Calculation
Daily metrics were calculated per trader and aggregated into `trader_daily_metrics`:
*   **Daily PnL:** Sum of `Closed PnL` per trader per day.
*   **Daily Win Rate:** Percentage of trades with `Closed PnL > 0` per trader per day.
*   **Average Trade Size USD:** Average `Size USD` per trader per day.
*   **Number of Trades:** Total number of trades per trader per day.
*   **Long/Short Ratio:** Ratio of 'BUY' trades to 'SELL' trades per trader per day.

## Part B: Analysis

### Performance Analysis: Fear vs. Greed Days
*   **Average Daily PnL:** Highest during 'Fear' and 'Extreme Greed' periods, suggesting potential opportunities during these sentiments. Lowest during 'Greed' and 'Neutral'.
*   **Average Daily Win Rate:** Highest during 'Extreme Greed' and lowest during 'Extreme Fear'.

### Trader Behavior Analysis by Sentiment
*   **Average Trade Size USD:** Largest during 'Fear' (indicating larger position taking) and smallest during 'Extreme Greed'.
*   **Number of Trades:** Highest during 'Extreme Fear' (potentially active traders trying to catch falling knives or bottom fishing) and lowest during 'Extreme Greed' and 'Greed'.
*   **Long/Short Ratio:** Highest during 'Extreme Fear' (strong long bias), and lowest during 'Extreme Greed' (more balanced trading).

### Identifying Trader Segments (Clustering)
*   **Features:** `avg_daily_pnl`, `avg_daily_win_rate`, `avg_trade_size`, `avg_num_trades`, `avg_long_short_ratio` were used as features, averaged per trader.
*   **Optimal Clusters:** The Elbow Method indicated **3** as the optimal number of clusters.
*   **KMeans Clustering Results:** Three distinct trader archetypes were identified:
    *   **Cluster 0 (Conservative Retail Traders - 22 traders):** Moderate PnL, lower win rate, smaller average trade sizes, lower trading activity.
    *   **Cluster 1 (High-Frequency Bullish Traders - 6 traders):** Higher PnL, highest win rate, very high trade frequency, and a strong long bias.
    *   **Cluster 2 (High-Impact Large-Volume Traders - 4 traders):** Highest PnL, largest average trade sizes (high conviction), and high trading activity.

### Drawdown Analysis
*   Cumulative PnL, Peak PnL, and Drawdown were calculated for each trader.
*   **Average Daily Drawdown:** 'Fear' and 'Greed' sentiments were associated with the highest average daily drawdowns, highlighting periods of increased capital risk. 'Extreme Greed' showed the lowest average drawdown.

## Part C: Strategy Ideas and Rules of Thumb

### Strategy Idea 1: Counter-Sentiment & Opportunistic Trading
*   **Observation:** High PnL during 'Fear' and 'Extreme Greed'.
*   **Rule of Thumb:** Consider increasing exposure, especially via long positions, during 'Fear' (buy the dip). Focus on highly selective, high-probability trades with smaller sizes during 'Extreme Greed'. Exercise caution and potentially reduce exposure during 'Greed' and 'Neutral' periods due to lower PnL and higher drawdowns.

### Strategy Idea 2: Archetype-Based Position Sizing and Activity
*   **Observation:** Distinct successful trader segments (Cluster 1 and Cluster 2).
*   **Rule of Thumb:** Traders can adapt their style:
    *   **High-Conviction Trading:** Emulate 'High-Impact Large-Volume Traders' (Cluster 2) by using larger position sizes for strong trade ideas, combined with robust risk management.
    *   **High-Frequency Trading:** Emulate 'High-Frequency Bullish Traders' (Cluster 1) for higher activity, especially in bullish phases, involving rapid execution and potentially tighter stop-losses.

## Part D: Predictive Model for Daily PnL

### Model Objective
To predict the total daily PnL based on past performance and market sentiment.

### Methodology
*   **Daily Aggregation:** Trader metrics were aggregated to a daily level, incorporating overall market sentiment.
*   **Feature Engineering:** Lagged features (1, 2, and 3-day lags for PnL and sentiment value) and one-hot encoded sentiment classifications were created.
*   **Data Split:** Chronological 80/20 train-test split for time series prediction.
*   **Model:** RandomForestRegressor was used for prediction.

### Model Performance
*   **Mean Absolute Error (MAE):** ~$77,597.88
*   **Root Mean Squared Error (RMSE):** ~$115,764.95
*   **R-squared (R2):** 0.05

### Conclusion
The predictive model's performance indicates that forecasting total daily PnL with the current set of features is challenging. The low R-squared suggests a significant portion of the variance in daily PnL is not explained by the model, implying a need for further feature engineering or exploration of more sophisticated modeling techniques to improve accuracy.
