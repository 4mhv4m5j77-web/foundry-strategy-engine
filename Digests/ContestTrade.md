# ContestTrade: A Multi-Agent Trading System Based on Internal Contest Mechanism
**Authors:** Li Zhao, Rui Sun, Zuoyou Jiang, Bo Yang, Yuxiao Bai, Mengting Chen, Xinyang Wang, Jing Li, Zuo Bai  |  **Venue:** arXiv preprint  |  **arXiv:** 2508.00554v3  |  **Year:** 2025

## Core Claim
A two-tiered multi-agent LLM trading system with internal contest mechanisms (Quantify-Predict-Allocate) for both data processing and strategy selection significantly outperforms single-agent, ML, DRL, and other multi-agent baselines by continuously filtering for the highest-quality agents through real-time competitive evaluation driven by authentic market feedback.

## Methodology
- Two specialized teams: Data Team (parallel agents condense raw market data into textual factors, capped at 4k tokens each) and Research Team (agents use Plan+ReAct with financial tools to generate trading signals with symbol, action, evidence, and limitations)
- Internal contest at both layers: Data Analyst Contest selects optimal factor portfolio via 0/1 Knapsack optimization under context-length constraints; Researcher Contest allocates capital proportionally to predicted Sharpe Ratios
- Factor quality quantified via Zero-Intelligence (ZI) Trader: each observation rated in {-2,-1,0,1,2} and scored by price change, making value measurement independent of agent strategy
- Short-term momentum in factor/agent scores predicted with LightGBM; Researcher Contest adds LLM Judge panel for qualitative signal assessment alongside realized Sharpe
- Tested on Chinese A-share market (Jan-Jun 2025, post-LLM knowledge cutoff) with DeepSeek-V3/R1, daily frequency, 0.001 transaction cost, T+1 settlement

## Key Results
- ContestTrade achieves 52.80% cumulative return, 3.12 Sharpe ratio, 12.41% max drawdown -- best across all baselines
- Next best: PPO (15.07% CR, 1.33 SR), RSI&KDJ (8.19% CR, 0.47 SR); multi-agent MASS was catastrophic (-19.12% CR, -1.76 SR)
- Contest mechanisms validated: Data Analyst Contest Rank IC = 0.054, ICIR = 0.13; Researcher Contest Rank IC = 0.079, ICIR = 0.18
- Ablation: removing Researcher Contest drops SR from 3.12 to 1.78; removing Data Analyst Contest drops SR to 2.01; removing all contest+research yields SR of 0.07
- LLM Judge contributes meaningfully: removal drops SR from 3.12 to 2.57

## V4 Relevance
- Directly validates tournament/contest architecture for strategy selection: the Quantify-Predict-Allocate pipeline (score agents historically, predict future utility, allocate proportionally) is a production-ready template for dynamic strategy weighting in a V4 agent layer
- Two-stage contest (data quality contest + strategy contest) shows both upstream signal filtering and downstream strategy selection benefit from competitive mechanisms -- V4 should apply contest pressure at multiple pipeline stages, not just final allocation
- ZI Trader scoring provides a strategy-independent measure of information value that could be adapted to evaluate any alpha factor or signal source without circular reasoning

## Limitations & Caveats
- Tested only on Chinese A-share market with short 6-month window (Jan-Jun 2025); no evidence of generalization to U.S. equities, crypto, or longer horizons
- Relies heavily on DeepSeek-V3/R1 as backbone LLM; cost, latency, and transferability to other models not explored
- Short-term momentum assumption (optimal windows m=5, n=3 for factors; n=5 for strategies) may break in regime changes or low-volatility environments

## Key References
- Gode & Sunder 1993 -- Zero-Intelligence traders for market efficiency (basis for ZI scoring)
- Chroma 2024 -- Context Rot: how LLMs degrade with context length (motivates knapsack portfolio optimization)
- Yao et al. 2023 -- ReAct framework for agent reasoning and acting
- Guo et al. 2025 -- MASS: multi-agent simulation scaling for portfolio construction (primary multi-agent baseline)
- Yu et al. 2025 -- FINCON: synthesized LLM multi-agent with verbal reinforcement (NeurIPS 2024)
- Wang et al. 2024 -- LLMFactor: extracting factors via prompts for stock prediction
- Li et al. 2024 -- FinMem: memory module for LLM trading agent adaptation
- Modarressi et al. 2025 -- NoLiMa: long-context evaluation (supports DC sigmoid model)
- Zhou et al. 2025 -- GSM-Infinite: LLM behavior under increasing context/reasoning complexity
- Wang & Ni 2024 -- QuantAgent: self-improving LLM trading agent
