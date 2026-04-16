# Trader Performance vs Market Sentiment - Hyperliquid × Fear/Greed Index

**Assignment:** Primetrade.ai Data Science Intern - Round-0  
**Dataset Period:** May 2023 - May 2025  
**Trades Analysed:** ~174,000 (perpetuals only)  
**Unique Traders:** 32 accounts

---

## Setup & How to Run

```bash
# 1. Clone / unzip the repo
# 2. Place data files in the same folder as the notebook:
#    - fear_greed_index.csv
#    - historical_data.csv

# 3. Install dependencies
pip install pandas numpy matplotlib seaborn scikit-learn scipy

# 4. Launch Jupyter
jupyter notebook trader_sentiment_analysis.ipynb
```

---

## Methodology

1. **Data Cleaning:** Loaded both CSVs, parsed timestamps (IST format `DD-MM-YYYY HH:MM`), aligned on daily date keys. Filtered perpetual trades only (excluded spot dust, settlements, liquidations). No missing values found in either dataset.

2. **Feature Engineering:** Computed per-trader daily metrics - total PnL, win rate, trade count, avg position size, long/short ratio. Also created binary sentiment flag (Fear/Greed/Neutral).

3. **Analysis:** 
   - t-tests for statistical significance of PnL differences across sentiment regimes
   - Behavioral breakdowns (frequency, size, directional bias) by sentiment
   - K-Means clustering (k=3) on 5 behavioral features to identify trader archetypes

4. **Bonus:** Random Forest classifier to predict daily profitability from sentiment + behavior features (~65% CV accuracy).

---

## Key Insights

| # | Insight | Evidence |
|---|---------|---------|
| 1 | **Fear days are significantly more profitable** | Mean PnL: Fear $6,082 vs Greed $2,057 (p < 0.05) |
| 2 | **Traders are more active + go long during Fear** | 107 trades/day (Fear) vs 70 (Greed); long ratio 55.7% vs 47.7% |
| 3 | **Only Winning Traders profit consistently across all regimes** | Losers turn negative on Greed days (-$142 avg) |

---

## Strategy Recommendations

**Strategy 1 - "Fear is Alpha":**  
During Fear days (index < 40), Moderate and Winning Traders should increase trade frequency ~30% and maintain long bias (~55%). This is historically when the highest PnL is captured.

**Strategy 2 - "Greed Caution":**  
During Greed days (index > 60), Losing Traders should cut position sizes by 30% and avoid chasing momentum longs. Their win rate drops to 36% and average PnL turns negative in this regime.

