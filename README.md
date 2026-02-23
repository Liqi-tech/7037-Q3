# Question3: Sleuthing via covariances and the basics of performance benchmarking

This project analyzes the **CS Global Macro Index at 2x Vol Net of 95 bps** (managed by former Bridgewater Co-CIO Bob Elliott) to:
- Identify the fund's underlying risk exposures
- Evaluate the appropriateness of traditional equity factor benchmarks
- Develop a custom cross-asset factor model for global macro funds
- Determine whether the fund captures structural risk premiums

### Research Question
**"What risk exposures does the CS Global Macro Index capture, and does it systematically harvest structural risk premiums?"**

## 🚀 Quick Start

### Prerequisites
- Python 3.8+
- Jupyter Notebook/Lab
- Required data files：
1. Fund Data: CS Global Macro Index at 2x Vol Net of 95bps 2025.09.xlsx
2. FF5 Factor Data: ff.five_factor.parquet
3. USD Exchange Rate Factor (USD_FX): DTWEXBGS.xlsx              
4. Interest Rate Factor (BOND): iShares-7-10-Year-Treasury-Bond-ETF_fund.xlsx       
5. Commodity Trend Factor (COMM): iShares-SP-GSCI-Commodity-Indexed-Trust_fund.xlsx      
6. Cross-Asset Momentum Factor (TSMOM): Time Series Momentum Factors Monthly.xlsx            


### 📈 Data Sources
| Data | Column Name | Source | Series | Period |
|--------|--------|--------|--------|--------|
| Fund Returns | monthly_return、excess_return | Credit Suisse | CS Global Macro Index | 2006-2025 |
| FF5 Factors | mkt_rf、smb、hml、rmw、cma | French Data Library | Fama-French 5 Factors | 1963-2025 |
| USD Exchange Rate | USD_FX | FRED | DTWEXBGS Index | 2006-2025 |
| Bond Factor | BOND | iShares | IEF ETF Total Return | 2002-2026 |
| Commodity Factor | COMM | iShares | GSG ETF Total Return | 2006-2026 |
| Momentum Factor | TSMOM | AQR | TSMOM Factor Returns | 1985-2025 |

### Regression Models
**FF5 Benchmark Model:**
- Fund_Excess_Return = α + β₁·MKT_RF + β₂·SMB + β₃·HML + β₄·RMW + β₅·CMA + ε
  
**Custom 4-Factor Macro Model:**
- Fund_Excess_Return = α + β₁·USD_FX + β₂·BOND + β₃·COMM + β₄·TSMOM + ε

### Statistical Analysis
- Ordinary Least Squares (OLS) regression
- Heteroskedasticity-robust standard errors
- Multicollinearity diagnostics (VIF)
- Model comparison via R-squared and information criteria
- Factor correlation analysis

## 📊 Results(Within the same time interval (2006-09-30 00:00:00 to 2025-01-31 00:00:00))

### 1. FF5 Model Limitations
- **Low Explanatory Power**: R² = 0.1166 (only 11.7% variance explained)
- **Missing Macro Exposures**: Cannot capture FX, commodity, or momentum risks
- **Inappropriate Benchmark**: Designed for equities, not multi-asset strategies

### 2. Custom Model Superiority
- **Higher Explanatory Power**: R² = 0.3760 (3.2× improvement)
- **Economically Meaningful**: Factors correspond to actual macro exposures
- **Better Risk Attribution**: Identifies true performance drivers

### 3. Risk Premium Evidence
The fund systematically harvests three structural risk premiums supported by academic literature:

| Factor | Risk Premium | Academic Support |
|--------|-------------|------------------|
| USD_FX | Carry Trade | Lustig et al. (2011) |
| COMM | Commodity | Gorton & Rouwenhorst (2006) |
| TSMOM | Time-Series Momentum | Moskowitz et al. (2012) |

### 4. Covariance Structure
- **Low Average Correlation**: 0.236 between factors
- **Independent Risk Sources**: No severe multicollinearity (all |r| < 0.7)
- **Economic Coherence**: USD-COMM negative correlation (-0.553) matches theory

## 📈 Visualizations

The analysis generates several key visualizations:

1. **Model Comparison**: R-squared and Alpha comparison bar charts
2. **Factor Exposures**: Coefficient plots with significance indicators
3. **Actual vs Predicted**: Scatter plots for both models
4. **Correlation Matrix**: Heatmap of factor correlations
5. **Cumulative Returns**: Comparison of actual vs model-predicted returns

## 🤔 Interpretation

### Why FF5 is Inappropriate
The Fama-French 5-factor model, while powerful for equities, is conceptually mismatched for global macro funds because:

1. **Wrong Asset Class**: Global macro trades FX, bonds, commodities - not just stocks
2. **Wrong Risk Factors**: Misses key macro risks (rates, FX, inflation, momentum)
3. **Wrong Correlation Structure**: Equity correlations ≠ macro asset correlations
4. **Cannot Explain Crisis Alpha**: Misses defensive positioning in market stress

### Custom Model Advantages
1. **Relevant Factors**: Captures actual macro strategy exposures
2. **Economic Intuition**: Factor relationships match financial theory
3. **True Diversification**: Low factor correlations show genuine risk diversification
4. **Actionable Insights**: Helps understand fund's actual investment process

## 📚 Academic References

1. **Carry Trade Premium**  
   Lustig, H., Roussanov, N., & Verdelhan, A. (2011). Common Risk Factors in Currency Markets. *Review of Financial Studies*, 24(11), 3731-3777.

2. **Commodity Risk Premium**  
   Gorton, G., & Rouwenhorst, K. G. (2006). Facts and Fantasies about Commodity Futures. *Financial Analysts Journal*, 62(2), 47-68.

3. **Time-Series Momentum**  
   Moskowitz, T. J., Ooi, Y. H., & Pedersen, L. H. (2012). Time Series Momentum. *Journal of Financial Economics*, 104(2), 228-250.

4. **Factor Investing**  
   Asness, C. S., Moskowitz, T. J., & Pedersen, L. H. (2013). Value and Momentum Everywhere. *Journal of Finance*, 68(3), 929-985.

