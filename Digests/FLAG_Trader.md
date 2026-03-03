# FLAG-TRADER: Fusion LLM-Agent with Gradient-based Reinforcement Learning for Financial Trading
**Authors:** Guojun Xiong, Zhiyang Deng, Keyi Wang, Yupeng Cao, Haohang Li, Yangyang Yu, et al.  |  **Venue:** arXiv preprint  |  **arXiv:** 2502.11433  |  **Year:** 2025

## Core Claim
A partially fine-tuned LLM can serve as an RL policy network for trading, where parameter-efficient RL fine-tuning (via PPO) on a 135M-parameter model (SmolLM2) consistently outperforms both buy-and-hold baselines and much larger LLM-agentic systems (including GPT-4o) on cumulative return and Sharpe ratio across stocks and crypto.

## Methodology
- Formulates trading as a finite-horizon MDP with discrete actions (Buy/Sell/Hold) and a Sharpe-ratio-delta reward signal
- Uses an actor-critic architecture where frozen base LLM layers preserve general knowledge while trainable top layers adapt to finance; policy head and value head share the trainable backbone
- Converts market state (price, volume, indicators, portfolio) into structured text prompts fed to the LLM, with action masking for invalid trades
- Trains with PPO (clip coef 0.2, lr 5e-4, GAE lambda 0.98, gamma 0.95) on SmolLM2-135M-Instruct using float16 on RTX A6000 GPUs
- Benchmarks against 13 LLMs (GPT-o1-preview, GPT-4, GPT-4o, Llama-3.1-70B, DeepSeek-67B, etc.) on 5 stocks (MSFT, JNJ, UVV, HON, TSLA) and BTC over ~7-month test windows (Oct 2020 - May 2021 for stocks; Apr - Nov 2023 for BTC)

## Key Results
- FLAG-TRADER (135M params) achieves highest or near-highest Cumulative Return and Sharpe Ratio across all 6 assets, beating GPT-4o and all open-source models up to 70B parameters
- On MSFT: CR 20.1%, SR 1.37 vs. buy-and-hold CR 15.3%, SR 1.04; on TSLA: CR 50.4%, SR 1.36 vs. buy-and-hold CR 39.2%, SR 0.87
- On BTC: CR 45.5%, SR 1.73, substantially beating all baselines including GPT-4 (CR 22.4%, SR 0.83)
- The RL-trained policy converges to a stable strategy that becomes insensitive to initial prompt design after sufficient training epochs
- A 135M model with RL fine-tuning bridges/surpasses the performance gap of models 500x larger operating as zero-shot agents

## V4 Relevance
- Validates that small, RL-fine-tuned LLMs can outperform large prompted LLMs for trading decisions -- V4 could use lightweight RL-tuned models as the execution/decision layer rather than expensive API calls to frontier models
- The structured prompt template (task description + action space + state representation + output format) is a proven pattern for converting market state into LLM-consumable input; directly applicable to V4's alpha factor generation pipeline
- The Sharpe-ratio-delta reward function is a practical, differentiable reward signal that aligns LLM policy optimization with risk-adjusted returns rather than raw PnL

## Limitations & Caveats
- Single-asset, fully-invest-or-fully-liquidate action space (no position sizing, no portfolio allocation) -- unrealistic for production multi-asset trading
- No transaction costs, slippage, or market impact modeling; test periods are relatively short (~7 months) and may not capture full market regimes
- Computationally expensive fine-tuning despite small model size; no explicit risk constraints (MDD not optimized), and reliance on structured prompts may introduce prompt-engineering sensitivity before convergence

## Key References
- Li et al., 2024a. *InvestBench: A benchmark for financial decision-making with LLM-based agents.* arXiv:2412.18174
- Li et al., 2023. *TradingGPT: Multi-agent system with layered memory for enhanced trading.* arXiv:2309.03736
- Yu et al., 2025. *FinCon: Synthesized LLM multi-agent system with conceptual verbal reinforcement.* NeurIPS 37
- Yu et al., 2024a. *FinMem: A performance-enhanced LLM trading agent with layered memory.* AAAI Symposium
- Zhang et al., 2024. *FinAgent: A multimodal foundation agent for financial trading.* arXiv:2402.18485
- Liu et al., 2022. *FinRL-Meta: Market environments and benchmarks for data-driven financial RL.* NeurIPS 35
- Zhai et al., 2024. *Fine-tuning large vision-language models as decision-making agents via RL.* arXiv:2405.10292
- Hambly et al., 2023. *Recent advances in reinforcement learning in finance.* Mathematical Finance 33(3)
