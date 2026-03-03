# Trading-R1: Financial Trading with LLM Reasoning via Reinforcement Learning
**Authors:** Yijia Xiao, Edward Sun, Tong Chen, Fang Wu, Di Luo, Wei Wang (UCLA, UW, Stanford, Tauric Research)  |  **Venue:** arXiv preprint  |  **arXiv:** 2509.11420  |  **Year:** 2025

## Core Claim
A financially-aware LLM (Qwen3-4B backbone) trained via a three-stage SFT+RL curriculum on 100k structured financial reasoning samples produces interpretable investment theses and outperforms both general-purpose LLMs (GPT-4.1, LLaMA-3.3) and reasoning-enhanced models (DeepSeek, O3-mini, O4-mini) on risk-adjusted trading metrics across six major equities/ETFs.

## Methodology
- **Tauric-TR1-DB dataset:** 100k samples spanning 18 months (Jan 2024 -- May 2025), 14 equities, 5 heterogeneous data sources (technical, fundamentals, news/sentiment, insider transactions, macro indicators) from Finnhub, SimFin, Google News, stockstats.
- **Reverse reasoning distillation:** Proprietary models (o3-mini, o4-mini) generate final recommendations; a planner LLM + GPT-4.1-nano reconstruct step-by-step reasoning traces synthetically, creating SFT supervision without access to hidden CoT.
- **Three-stage easy-to-hard curriculum:** Stage I (Structure) teaches thesis formatting via SFT+RFT; Stage II (Claims) trains evidence-grounded reasoning; Stage III (Decision) aligns outputs with volatility-aware market labels using GRPO.
- **Volatility-driven labeling:** Forward returns over 3/7/15-day EMA horizons, normalized by rolling 20-period volatility, combined (weights 0.3/0.5/0.2) and discretized into 5 actions (Strong Sell to Strong Buy) via asymmetric percentile thresholds.
- **Evaluation:** Out-of-sample backtest June 1 -- August 31, 2024 on AAPL, GOOGL, AMZN, NVDA, MSFT, META, SPY. Metrics: Cumulative Return, Sharpe Ratio, Hit Rate, Max Drawdown.

## Key Results
- Trading-R1 achieves Sharpe ratios of 2.72 (NVDA), 1.80 (AAPL), 1.72 (AMZN), 0.87 (MSFT), 0.86 (META), 1.60 (SPY) -- consistently positive across all assets.
- 8.08% cumulative return on NVDA vs. 3.15% for GPT-4.1; outperforms GPT-4.1 on AAPL (Sharpe 1.80 vs. 1.24).
- Hit rates reach 70.0% (NVDA) and 64.0% (SPY), leading all model categories.
- Max drawdowns are consistently lower (3.68% NVDA vs. 7.88% for best RLM), demonstrating better risk control.
- Clear performance hierarchy: SLM < RLM < LLM < Trading-SFT ~ Trading-RFT < Trading-R1. Off-the-shelf reasoning models (O3-mini, O4-mini) underperform general LLMs on trading due to unguided reasoning drift.

## V4 Relevance
- **Thesis-as-signal architecture:** Trading-R1's structured investment thesis output (with tagged sections for fundamentals, technicals, sentiment, conclusion) could serve as an intermediate representation in V4 -- the LLM generates structured reasoning that feeds into XGBoost as categorical/text features rather than raw predictions.
- **Volatility-aware labeling scheme:** The multi-horizon EMA return normalization and asymmetric quantile discretization is directly applicable for generating training labels in V4's alpha factor pipeline, avoiding overfitting to raw price moves.
- **Reverse reasoning distillation is a practical data flywheel:** Using cheap models to reconstruct reasoning from proprietary model outputs is a scalable way to build domain-specific SFT datasets without expensive annotation -- relevant for bootstrapping V4's LLM component.

## Limitations & Caveats
- Backtest period is only 3 months (June--August 2024) on large-cap US equities during a generally bullish market; no bear market or cross-asset generalization demonstrated.
- 4B parameter model with medium-term (~1 week) holding period; latency constraints and the exclusion of HFT/long-horizon strategies limit practical deployment scope.
- Training universe is 14 mega-cap tickers with structural bullish bias (47% Buy/Strong Buy labels); performance on small-caps, international markets, or regime changes is untested.

## Key References
- DeepSeek-AI et al. (2025) -- DeepSeek-R1 multi-stage RL training
- Shao et al. (2024) -- GRPO (Group Relative Policy Optimization)
- Wang et al. (2023) -- QuantAgent dual-loop alpha factor generation
- Wang et al. (2023) -- AlphaGPT human-in-the-loop alpha mining
- Wu et al. (2023) -- BloombergGPT
- Xiao et al. (2025) -- TradingAgents multi-agent framework
- Xie et al. (2023) -- PIXIU/FinMA financial LLM fine-tuning
- Lopez-Lira and Tang (2023) -- LLM sentiment for long-short strategies
- Koa et al. (2024) -- SEP: RL with memorization and reflection for trading
- Hu et al. (2021) -- LoRA parameter-efficient fine-tuning
