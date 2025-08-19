# Crypto Sentiment Analysis

A regime-aware cryptocurrency trading policy based on Fear & Greed Index sentiment analysis.

## Overview

This project implements a sophisticated trading strategy that adjusts cryptocurrency portfolio allocation based on market sentiment regimes (Fear, Greed, Neutral). The analysis uses historical trading data from Hyperliquid and the Fear & Greed Index to develop and backtest regime-specific coin selection policies.

## Features

- **Sentiment-Based Regime Classification**: Uses Fear & Greed Index to classify market conditions
- **Regime-Aware Coin Selection**: Different favor/avoid coin lists for each sentiment regime
- **Liquidity Filtering**: Guards against low-liquidity trades during volatile periods
- **Performance Backtesting**: Comprehensive analysis comparing strategy vs baseline performance
- **Policy Export**: Generates JSON configuration files for live trading implementation

## Data Sources

- **Hyperliquid Trading Data**: Historical cryptocurrency trading records with PnL, volumes, and timestamps
- **Fear & Greed Index**: Daily sentiment indicators for market regime classification

## Strategy Components

### 1. Data Processing Pipeline
- Normalizes trading data and sentiment indicators
- Converts timestamps to UTC for consistent analysis
- Calculates PnL metrics and trading statistics

### 2. Regime Classification
- **Fear Regime**: High-volatility, risk-off periods
- **Greed Regime**: High-confidence, risk-on periods  
- **Neutral Regime**: Balanced market conditions

### 3. Coin Selection Rules
- **Fear Regime**: Favor high-performing coins (BTC, ETH, SOL, etc.), avoid volatile altcoins
- **Greed Regime**: Currently disabled (weight = 0.0) based on backtesting results
- **Neutral Regime**: Currently disabled (weight = 0.0) for conservative approach

### 4. Weighting Strategy
```
Fear:   favor=1.0, neutral=0.5, avoid=0.0
Greed:  favor=0.0, neutral=0.0, avoid=0.0 (OFF)
Neutral: favor=0.0, neutral=0.0, avoid=0.0 (OFF)
```

### 5. Liquidity Guard
- Filters out low-liquidity trades on Greed days
- Uses per-day median size threshold for quality control

## Key Results

Based on backtesting analysis:
- **Fear Regime**: Strategy shows positive performance vs baseline
- **Conservative Approach**: Greed and Neutral regimes disabled to avoid negative drag
- **Risk Management**: Liquidity filtering improves trade quality

## Files Structure

```
crypto (1).ipynb          # Main analysis notebook
README.md                 # This documentation
policy_v1.json           # Exported trading policy configuration
daily_compare_v1.csv     # Daily performance comparison data
regime_compare_stats_v1.csv # Regime-level performance statistics
```

## Usage

### Running the Analysis
1. Ensure you have the required data files:
   - `fear_greed_index.csv`
   - `historical_data.csv`

2. Update the data paths in the notebook:
   ```python
   DATA_DIR = '/path/to/your/data'  # Update this path
   ```

3. Run all cells in `crypto (1).ipynb` to:
   - Load and process data
   - Generate regime classifications
   - Backtest the strategy
   - Export policy configurations

### Policy Implementation
The generated `policy_v1.json` contains:
- Coin favor/avoid lists per regime
- Weighting schemes
- Liquidity guard settings

## Dependencies

```python
import pandas as pd
import numpy as np
import json
from pathlib import Path
```

## Performance Metrics

The analysis tracks multiple performance indicators:
- **Daily Net PnL**: Profit/Loss per trading day
- **Win Rate**: Percentage of profitable trades
- **Regime Comparison**: Strategy vs baseline performance by market sentiment
- **Improvement Metrics**: Absolute and relative performance gains

## Configuration

### Minimum Thresholds
- **Minimum Coin Days**: 10 days of trading history required
- **Minimum Account Days**: 5 days per regime for inclusion
- **Quality Filters**: Win rate and liquidity thresholds for coin selection

### Risk Management
- **Liquidity Guard**: Enabled for Greed regime days
- **Conservative Weights**: Zero allocation to underperforming regimes
- **Per-Day Filtering**: Median size threshold for trade quality

## Future Enhancements

- Real-time sentiment data integration
- Additional technical indicators
- Machine learning-based regime prediction
- Risk-adjusted performance metrics
- Multi-timeframe analysis

## Author

Aryan Nayak

## License

This project is for educational and research purposes.

## Disclaimer

This analysis is for informational purposes only and should not be considered as financial advice. Cryptocurrency trading involves substantial risk of loss.
