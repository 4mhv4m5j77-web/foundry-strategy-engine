# Automate Strategy Finding with LLM in Quant Investment
**Authors:** Zhizhuo Kou, Holam Yu, Junyu Luo, Jingshu Peng, Xujia Li, Chengzhong Liu, Juntao Dai, Lei Chen, Sirui Han, Yike Guo  |  **Venue:** EMNLP 2025  |  **arXiv:** 2409.06289  |  **Year:** 2025

## Core Claim
A three-stage framework using LLMs to generate executable alpha factor candidates, multi-agent evaluation to filter factors by market conditions, and dynamic weight optimization produces strategies that significantly outperform traditional and ML-based benchmarks across Chinese and US markets.

## Methodology
- **Stage 1 — Seed Alpha Factory (SAF):** GPT-4o mines financial literature to generate alpha factors as executable formulas (cross-section + time-series operators), categorized into Momentum, Mean Reversion, Volatility, Fundamental, Liquidity, Quality, Growth, Technical, and Macro categories
- **Stage 2 — Multi-Agent Evaluation:** Confidence Score Agent (CSA) and Risk Preference Agent (RPA) evaluate factors using multimodal data (text, numerical, visual, audio, video); category-based selection algorithm ensures diversity
- **Stage 3 — Weight Optimization:** 3-layer MLP (10 hidden nodes, ReLU) maps historical alpha values to future yields, dynamically adjusting category weights based on market conditions
- **Data:** SSE50, CSI300 (China) and SP500 (US), OHLCV + VWAP features, Jan 2019–Jan 2024
- **Evaluation:** Information Coefficient (IC), Sharpe Ratio, Sortino Ratio, Calmar Ratio, Maximum Drawdown

## Key Results
- 53.17% cumulative return on SSE50 (Jan 2023–Jan 2024) vs. -13.22% benchmark, best Sharpe (0.287) and lowest volatility (0.762%)
- Outperforms XGBoost (9.53%), LightGBM (7.13%), MLP (3.11%), FinCon (22.47%), SEP (17.89%)
- Cross-market validation: 192.27% annual return on CSI300 H1 2023; 118.24% on SP500 H1 2023
- Maintained positive returns during H1 2022 downturn (12.78% CSI300, 2.77% SP500) while benchmarks crashed (-30.37%, -44.22%)
- Ablation: Removing CSA reduces out-of-sample IC by 31.9% and Sharpe by 22.5%; both agents critical for bear market performance

## V4 Relevance
- **Direct validation of V4's core pattern:** LLM generates alpha factors → ML model (MLP/XGBoost) optimizes weights — nearly identical to V4's Idea Agent → Forge pipeline
- **Seed Alpha Factory concept** maps directly to V4's Thesis Library: LLM-generated formulaic alphas stored and categorized for systematic evaluation
- **Multi-agent evaluation with market-condition awareness** validates V4's regime-conditioning approach; CSA/RPA pattern applicable to V4's Contest mechanism

## Limitations & Caveats
- Alpha generation depends heavily on input document quality; LLM-generated alphas occasionally lack financial intuition of human analysts
- Multi-agent evaluation presupposes persistent historical relationships between market conditions and alpha performance — may break during regime shifts
- Validation primarily on equity markets; cross-asset applicability requires additional investigation

## Key References
- Kakushadze, 2016 — "101 Formulaic Alphas" (foundational alpha repository)
- Cui et al., 2021 — AlphaEvolve: learning framework for novel alphas (SIGMOD)
- Chen & Guestrin, 2016 — XGBoost
- Yu et al., 2024 — FinCon: conceptual verbal reinforcement for financial decisions
- Zhang et al., 2024a — StockAgent: LLM-based stock trading in simulated environments
- Yu et al., 2023b/2024 — FinMem: layered memory trading agent
- Tang et al., 2025 — AlphaAgent: LLM-driven alpha mining with regularized exploration
- Lee et al., 2020 — MAPS: multi-agent RL portfolio management
- Cong et al., 2021 — AlphaPortfolio: direct construction through deep RL
