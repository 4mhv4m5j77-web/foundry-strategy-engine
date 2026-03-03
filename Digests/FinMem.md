# FinMem: A Performance-Enhanced LLM Trading Agent with Layered Memory and Character Design
**Authors:** Yangyang Yu, Haohang Li, Zhi Chen, Yuechen Jiang, Yang Li, Denghui Zhang, Rong Liu, Jordan W. Suchow, Khaldoun Khashanah  |  **Venue:** arXiv (q-fin.CP)  |  **arXiv:** 2311.13743  |  **Year:** 2023

## Core Claim
FinMem introduces a three-module LLM trading agent (Profiling, Memory, Decision-making) whose layered long-term memory -- stratified into shallow (daily news), intermediate (10-Q filings), and deep (10-K filings) tiers with distinct decay rates -- outperforms both DRL agents and other LLM trading agents on cumulative return and Sharpe ratio across five US equities.

## Methodology
- Three-module architecture: **Profiling** (dynamic character with risk-seeking/averse/adaptive settings), **Memory** (working memory + three-layer long-term memory with FAISS vector store), **Decision-making** (Buy/Sell/Hold per share via Guardrails AI validation).
- Memory retrieval scores combine recency (exponential decay, Q_shallow=14d, Q_intermediate=90d, Q_deep=365d), relevancy (cosine similarity via OpenAI text-embedding-ada-002), and importance (piecewise scoring with layer-specific decay bases alpha_shallow=0.9, alpha_intermediate=0.967, alpha_deep=0.988).
- Backbone LLM: GPT-4-Turbo (temperature=0.7). Training: Aug 2021 -- Oct 2022; Testing: Oct 2022 -- Apr 2023. Data from Yahoo Finance (OHLCV) and Alpaca News API (Benzinga).
- Compared against Buy-and-Hold, PPO, DQN, A2C, FinGPT, and Generative Agents (Park et al.) across TSLA, NFLX, AMZN, MSFT, COIN.
- Access counter mechanism promotes high-impact memories from shallow to deeper layers over time, resetting recency scores upon promotion.

## Key Results
- TSLA: FinMem achieves 61.76% cumulative return (Sharpe 2.68) vs. next-best DQN at 33.34% (Sharpe 0.97) and B&H at -18.63%.
- NFLX: 36.45% cumulative return (Sharpe 2.02) vs. B&H 35.51% and Generative Agents 32.06%.
- COIN: 34.98% cumulative return (Sharpe 0.72) where B&H lost -30.01% and FinGPT lost -88.78%.
- FinMem requires only 6--12 months of training data vs. ~10 years for DRL agents, with statistically significant outperformance (Wilcoxon signed-rank test).
- Consistently achieves lowest volatility and max drawdown among top performers (e.g., TSLA annualized volatility 46.86% vs. 69.98% for B&H).

## V4 Relevance
- **Layered memory is directly applicable:** Stratifying market data by timeliness (daily news vs. quarterly filings vs. annual reports) with distinct decay rates materially improves signal quality -- a V4 agent should implement analogous shallow/intermediate/deep memory tiers with tunable retention.
- **Self-adaptive risk character improves robustness:** Dynamically switching between risk-seeking and risk-averse profiles based on recent cumulative returns acts as an implicit drawdown control mechanism worth replicating.
- **Minimal training data requirement:** The framework's effectiveness with only 6-12 months of daily data validates that LLM-based agents can bootstrap quickly on new instruments, relevant for expanding V4 to new tickers or asset classes.

## Limitations & Caveats
- Single-share trading only (Buy/Sell/Hold for 1 share per day) -- no portfolio-level allocation, position sizing, or multi-asset strategies tested.
- GPT-4-Turbo API costs and latency are not reported; real-time deployment feasibility is unaddressed.
- Evaluated on only 5 high-liquidity US large-cap stocks during a specific market regime (Oct 2022 -- Apr 2023); generalization to other asset classes, market caps, or regimes is unvalidated.

## Key References
- Park et al. (2023) -- Generative Agents: Interactive Simulacra of Human Behavior [35]
- Yang et al. (2023) -- FinGPT: Open-Source Financial LLMs [55]
- Wang et al. (2023) -- Survey on LLM-based Autonomous Agents [50]
- Liu et al. (2021, 2022) -- FinRL: DRL framework for financial trading [28, 26]
- Mnih et al. (2013) -- DQN [32]
- Schulman et al. (2017) -- PPO [42]
- Ebbinghaus (1885/2013) -- Memory/Forgetting Curve [9]
- Johnson et al. (2021) -- FAISS vector database [23]
