# Interpretable Hypothesis-Driven Trading: A Rigorous Walk-Forward Validation Framework for Market Microstructure Signals
**Authors:** Gagan Deep, Akash Deep, William Lamptey  |  **Venue:** arXiv (q-fin.TR)  |  **arXiv:** 2512.12924  |  **Year:** 2025

## Core Claim
Rigorous walk-forward validation with strict information-set discipline dramatically tempers backtested trading performance (0.55% annualized vs. typical published claims of 15-30%), demonstrating that most published strategy returns are artifacts of overfitting and lookahead bias. The framework is hypothesis-source agnostic and extends to LLM-generated trading hypotheses.

## Methodology
- Rolling walk-forward validation across 34 independent quarterly test periods (W=252 training days, H=63 test days, step=63) on 100 US equities from 2015-2024
- Five hand-crafted hypothesis types tested: institutional accumulation, flow momentum, mean reversion, breakouts, and range-bound value signals derived from daily OHLCV data
- Epsilon-greedy RL agent (epsilon_train=0.7, epsilon_test=0.1) learns which hypothesis types to execute based on historical win rates and average returns
- Strict information-set discipline: features, signals, and execution decisions use only data available up to time t; no future information leakage
- Realistic transaction costs ($1 commission + 5bps slippage), max 5 concurrent positions, 20% max per position, 30-day max holding period

## Key Results
- Annualized return of 0.55% with Sharpe ratio 0.33, maximum drawdown of only -2.76% (vs. SPY -23.8%), beta = 0.058
- Aggregate returns are NOT statistically significant: t-stat=0.96, p-value=0.34, 95% CI includes zero [-0.12%, +0.43%]
- Strong regime dependence: +2.4% annualized during high-volatility periods (2020-2024) vs. -0.16% during stable markets (2015-2019)
- Only 41% of folds profitable (14/34), trade-level win rate 46.5%, 140 total trades across all folds
- Market-neutral characteristics: correlation with SPY of 0.53, annualized alpha of 0.06%

## V4 Relevance
- Adopt the paper's walk-forward protocol (W=252, H=63, 34 folds) as a baseline validation standard; any V4 strategy claiming alpha must survive this level of rigor before deployment
- The hypothesis-source agnosticism is directly applicable: V4 can use LLMs to generate trading hypotheses in natural language, then validate them through this same walk-forward framework with interpretable audit trails
- Daily OHLCV microstructure signals require elevated volatility to function; V4 should implement regime-conditional activation that scales signal weight with realized volatility rather than running signals uniformly

## Limitations & Caveats
- Survivorship bias present: only stocks with continuous 2015-2024 history included; delisted/acquired stocks excluded, biasing results upward
- Only five hand-crafted hypothesis types tested; the framework's extensibility to LLMs/genetic programming is discussed but not empirically validated
- Statistical tests not adjusted for multiple comparisons across hypothesis types, and the aggregate p-value of 0.34 means the strategy itself does not demonstrate significant alpha

## Key References
- Harvey, C.R., Liu, Y., & Zhu, H. (2016). ...and the Cross-Section of Expected Returns. *Review of Financial Studies*
- Bailey, D.H. & Lopez de Prado, M. (2014). The Deflated Sharpe Ratio. *Journal of Portfolio Management*
- Pardo, R. (1992, 2008). Walk-forward analysis as gold standard for trading-strategy validation
- Gu, S., Kelly, B., & Xiu, D. (2020). Empirical Asset Pricing via Machine Learning. *Review of Financial Studies*
- Rudin, C. (2019). Stop Explaining Black Box ML Models for High-Stakes Decisions
- McLean, R.D. & Pontiff, J. (2016). Does Academic Research Destroy Stock Return Predictability?
- Bailey, D.H., Borwein, J., Lopez de Prado, M., & Zhu, Q.J. (2017). CSCV for Probability of Backtest Overfitting
- Arian, H. et al. (2024). Comparing validation methods for ML in finance
- Easley, D. et al. (2012). VPIN: Volume-Synchronized Probability of Informed Trading
