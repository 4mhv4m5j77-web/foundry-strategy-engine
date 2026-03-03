# FinAgent: A Multimodal Foundation Agent for Financial Trading
**Authors:** Wentao Zhang, Lingxuan Zhao, Haochong Xia, Shuo Sun, et al.  |  **Venue:** KDD 2024  |  **arXiv:** 2402.18485  |  **Year:** 2024

## Core Claim
FinAgent is the first multimodal foundation agent for financial trading that integrates textual, numerical, and visual data (Kline charts, trading charts) with tool-augmented decision-making. Its dual-level reflection mechanism and diversified memory retrieval system enable it to outperform 12 baselines across 6 financial datasets by over 36% average improvement on profit.

## Methodology
- **Architecture:** Five core modules -- market intelligence (multimodal summarization), memory (vector store with typed retrieval), low-level reflection (price-pattern correlation), high-level reflection (decision quality review), and tool-augmented decision-making.
- **Multimodal inputs:** Daily asset prices (OHLC), Kline/trading charts as images processed by GPT-4V, news from Bloomberg/Seeking Alpha/CNBC, and expert guidance from financial analysts.
- **Diversified retrieval:** M retrieval types x K top items, with separate query text fields for trading vs. retrieval tasks, reducing noise from mixed-objective summaries.
- **Formulation:** Financial trading as MDP with LLM modules embedded in the RL pipeline; actions are buy/sell/hold per asset per day. Trained on a single NVIDIA RTX A6000.
- **Evaluation:** 6 datasets (AAPL, AMZN, GOOGL, MSFT, TSLA, ETHUSD), test period 2023-06-01 to 2024-01-01, measured on ARR, Sharpe, Sortino, Calmar, MDD, and Volatility.

## Key Results
- Best-in-class ARR across all 6 datasets; 92.27% return on TSLA (84.39% relative improvement over next-best baseline), 56.15% on GOOGL, 44.74% on MSFT.
- Outperforms FinGPT and FinMem (LLM baselines) by large margins; FinMem underperforms Buy&Hold on AMZN while FinAgent achieves 42.33% ARR.
- Ablation: low-level reflection adds 45-101% ARR improvement on TSLA; augmented tools boost stock performance but hurt crypto (auxiliary agents are stock-specialized).
- Diversified retrieval yields clear ARR and Sharpe gains on AAPL; t-SNE confirms distinct retrieval-type clusters in embedding space.
- Rule-based methods control risk better (lower MDD) but sacrifice returns; FinAgent trades slightly higher risk for substantially higher profit.

## V4 Relevance
- **Dual-level reflection as a design pattern:** Separating low-level (market-signal-to-price correlation) and high-level (decision quality review) reflection maps directly to a V4 architecture where LLMs generate alpha factors (low-level) and a meta-layer evaluates strategy performance (high-level).
- **Diversified retrieval with typed queries:** Generating multiple retrieval query types per time step (short-term impact, long-term trend, sentiment) prevents information collapse and is directly applicable to a RAG-based alpha research system.
- **Tool augmentation boundaries:** Augmented expert tools (MACD, KDJ/RSI, Mean Reversion) helped stocks but degraded crypto performance, indicating that tool selection must be asset-class-aware in any generalist trading agent.

## Limitations & Caveats
- Single-asset trading only (no portfolio optimization or cross-asset allocation); each asset is traded independently with no position sizing beyond buy/sell/hold.
- Relies heavily on GPT-4/GPT-4V API calls at every time step across 5 modules, making inference cost and latency prohibitive for high-frequency or large-universe deployment.
- Test period (7 months, mid-2023 to early 2024) is relatively short and coincides with a broad equity rally, limiting evidence of performance in bear markets or regime changes.

## Key References
- BloombergGPT (Wu et al., 2023) -- finance-domain LLM
- FinGPT (Yang et al., 2023) -- open-source financial LLM
- FinMem (Yu et al., 2023) -- LLM agent with layered memory for trading
- Toolformer (Schick et al., 2023) -- teaching LLMs to use tools
- Generative Agents (Park et al., 2023) -- interactive simulacra of human behavior
- TradeMaster (Shuo Sun et al., 2023) -- quantitative trading platform
- PRUDEX-Compass (Shuo Sun et al., 2023) -- RL evaluation for financial markets
- FinRL (Liu et al., 2020) -- deep RL library for automated stock trading
- AutoGPT (Yang et al., 2023) -- Auto-GPT for online decision making
- Voyager (Wang et al., 2023) -- open-ended embodied agent with LLMs
