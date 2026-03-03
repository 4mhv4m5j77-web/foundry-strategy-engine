# HedgeAgents: A Balanced-aware Multi-agent Financial Trading System
**Authors:** Xiangyu Li, Yawen Zeng, Xiaofen Xing, Jin Xu, Xiangmin Xu  |  **Venue:** WWW Companion '25  |  **arXiv:** 2502.13165  |  **Year:** 2025

## Core Claim
A multi-agent LLM system using hedging strategies across Bitcoin, Dow Jones stocks, and Forex achieves 71.6% annualized return and 405% total return over 3 years (2021-2023), outperforming all baselines on 9 evaluation metrics by coordinating specialized asset-class agents through three distinct conference types.

## Methodology
- **Architecture:** 3 specialist agents (Bitcoin Analyst Dave, DJ30 Analyst Bob, FX Analyst Emily) + 1 Hedge Fund Manager (Otto), each with 23 tools, 8 action types, and 3 memory stores (Market Information, Investment Reflection, General Experience)
- **Coordination via conferences:** Budget Allocation Conference (30-day cycle, portfolio rebalancing with CVaR optimization), Experience Sharing Conference (peer knowledge exchange), Extreme Market Conference (emergency response when daily amplitude >5% or 3-day cumulative >10%)
- **Decision pipeline:** Memory retrieval (top-K=5 similar cases) -> LLM-based decision making (RL-style reward optimization) -> Reflection update
- **Data:** Daily OHLCV prices + 60 technical indicators + news headlines from Yahoo Finance and Alpaca News API (Jan 2015 - Dec 2023; train: 2015-2020, test: 2021-2023)
- **Backbone:** GPT-4-1106-preview at temperature 0.7; total system cost ~$15 over 3 years (~$0.02/day)

## Key Results
- **Overall:** 71.6% ARR, 405.34% TR, 2.41 Sharpe Ratio, 11.02 Calmar Ratio, 14.21% MDD -- improvements of 33.75%, 54.72%, 24.49%, 44.28%, 16.76% over best baselines respectively
- **Robustness:** Only system with positive TR/SR in all three stress scenarios (rapid rise, rapid decline, frequent fluctuations); 8/9 baselines show negative scores in rapid decline scenarios
- **Ablation:** All three conferences are essential -- removing BAC drops ARR from 71.6% to 43.88%; removing ESC drops ENT from 3.13 to 2.56; removing EMC increases MDD from 14.21% to 24.44%
- **LLM backbone:** Framework is robust across 6 LLMs (ChatGLM-6B to GPT-4), though GPT-4 achieves best returns; larger models adopt more aggressive strategies with higher risk
- **Temporal isolation test (2024 Q1-Q3):** 68.44% TR with 2.1 Sharpe Ratio, confirming out-of-sample robustness

## V4 Relevance
- **Role specialization pattern:** The fund-manager-plus-specialists hierarchy with domain-specific tools and memories maps directly to a V4 multi-agent architecture -- each agent maintains its own memory stores and the manager orchestrates via structured conferences rather than free-form chat
- **Conference-based coordination:** Three conference types (periodic rebalancing, knowledge sharing, emergency response) provide a concrete template for multi-agent communication protocols with different trigger conditions and cadences
- **Hedging as risk management:** The cross-asset hedging approach (optimizing expected return minus portfolio risk minus CVaR) demonstrates that multi-agent systems need explicit risk-control agents and mechanisms, not just return-maximizing signal generators

## Limitations & Caveats
- Backtested only on 3 asset classes (BTC, DJ30 stocks, FX) with daily frequency -- no intraday, options, or broader equity universe; transaction costs and slippage handling unclear
- Relies on GPT-4 API calls for all decisions, creating latency and cost dependencies that may not scale to real-time trading; memory retrieval quality depends heavily on embedding model
- No comparison against simple hedged portfolio baselines (e.g., 60/40, risk-parity) or professional fund benchmarks; the 400% return period (2021-2023) includes a massive BTC bull run that likely dominates results

## Key References
- Sun et al. 2023. PRUDEX-Compass: Towards Systematic Evaluation of RL in Financial Markets. arXiv:2302.00586
- Wang et al. 2021. DeepTrader: A Deep RL Approach for Risk-Return Balanced Portfolio Management. AAAI 2021
- Yang et al. 2023. FinGPT: Open-Source Financial Large Language Models. arXiv:2306.06031
- Zhang et al. 2024. FinMem: A Performance-Enhanced LLM Trading Agent with Layered Memory. arXiv:2311.13743
- Wang et al. 2024. FinAgent: A Multimodal Foundation Agent for Financial Trading. arXiv:2402.18485
- Hong et al. 2023. MetaGPT: Meta Programming for a Multi-Agent Collaborative Framework. arXiv:2308.00352
- Li et al. 2024. FinReport: Explainable Stock Earnings Forecasting via News Factor Analyzing Model. WWW '24
- Sun et al. 2023. TradeMaster: A Holistic Quantitative Trading Platform Empowered by RL. NeurIPS Datasets Track
