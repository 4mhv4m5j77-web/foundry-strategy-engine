# FINSABER: Can LLM-based Financial Investing Strategies Outperform the Market in Long Run?
**Authors:** Weixian Waylon Li, Hyeonjun Kim, Mihai Cucuringu, Tiejun Ma  |  **Venue:** KDD '26  |  **arXiv:** 2505.07078  |  **Year:** 2026

## Core Claim
Previously reported LLM investing advantages are largely artifacts of survivorship bias, look-ahead bias, and data-snooping from narrow stock universes and short evaluation windows. Under rigorous, bias-mitigated backtesting over 20 years and 100+ symbols, LLM strategies fail to outperform simple baselines like Buy and Hold on risk-adjusted metrics, and neither LLM agent generates statistically significant alpha.

## Methodology
- Built FINSABER, a comprehensive backtesting framework with multi-source data (stock prices, financial news, 10-K/10-Q filings) spanning 2000-2024, explicitly including delisted stocks to address survivorship bias.
- Evaluated two open-source LLM investors (FinMem, FinAgent) against rule-based (Buy and Hold, SMA/WMA Cross, ATR Band, Bollinger Bands, Trend Following), predictor-based (ARIMA, XGBoost), and RL-based (A2C, PPO, SAC, TD3) strategies.
- Used rolling-window evaluations across four selection strategies (Random Five, Momentum Factor, Volatility Effect, FinCon Selection Agent) over historical S&P 500 constituents (63-91 symbols per setup) to mitigate data-snooping bias.
- Conducted paired t-tests and CAPM-based alpha/beta decomposition to statistically validate findings; performed regime-specific analysis (bull/bear/sideways).

## Key Results
- Extending evaluation from the originally reported ~1 year to 20 years eliminates LLM superiority: Buy and Hold consistently ranks as a top performer across most symbols on Sharpe and Sortino ratios.
- Under composite bias-mitigated evaluation, Buy and Hold achieves the highest Sharpe (0.703) and Sortino (1.291) in the Volatility Effect setup; LLM methods lag with Sharpe around 0.24 and larger drawdowns.
- Neither FinMem nor FinAgent generates statistically significant alpha (all p-values > 0.34). Paired t-tests confirm Buy and Hold significantly outperforms both LLM strategies (p < 0.001) in composite setups.
- Regime analysis reveals LLM agents are pathologically miscalibrated: too conservative in bull markets (missing upside) and too aggressive in bear markets (amplifying drawdowns). No active strategy surpasses Buy and Hold's 0.61 Sharpe in bull regimes.
- LLM backtesting costs are substantial: ~$700 for composite experiments; FinAgent is 6x more expensive than FinMem.

## V4 Relevance
- Any V4 backtesting pipeline must address survivorship bias (use historical constituent lists with delisted symbols), look-ahead bias (strict temporal data alignment), and data-snooping bias (rolling-window evaluation across diverse symbols). FINSABER's two-step pipeline (selection then timing) is a strong template.
- LLM-generated trading signals should not be trusted without regime-aware risk controls. The finding that model complexity does not translate to market competence argues for prioritizing adaptive risk management and trend detection over scaling agent architectures.
- Benchmark design matters: always include simple baselines (Buy and Hold, ARIMA, ATR Band) and evaluate on risk-adjusted metrics (Sharpe, Sortino, MDD) rather than raw returns alone.

## Limitations & Caveats
- Look-ahead bias is not fully eliminated: pre-trained LLMs may have seen historical financial data in their training corpora, potentially biasing results in their favor (yet they still underperform).
- Traditional rule-based strategies were not individually tuned per rolling window; domain-specific tuning would likely widen the gap further against LLM methods.
- Only long-only strategies were evaluated; results may differ for long-short or hedged approaches. Analysis restricted to publicly available data (no proprietary news feeds or earnings transcripts).

## Key References
- Bailey et al. (2015) - The Probability of Backtest Overfitting (data-snooping bias)
- Garcia & Gould (1993) - Survivorship bias in portfolio management
- Fama (1970) - Efficient Market Hypothesis
- Lo (2004) - Adaptive Markets Hypothesis
- Yu et al. (2023) - FinMem: LLM trading agent with layered memory
- Yu et al. (2024) - FinCon: synthesized LLM multi-agent system
- Ding et al. (2024) - LLM agents in financial trading (survey)
- Wang et al. (2024) - LLMFactor: extracting profitable factors via LLM prompts
- Fatouros et al. (2025) - MarketSenseAI 2.0
- Chan (2021) - Quantitative Trading: How to Build Your Own Algorithmic Trading System
