# FinCon: A Synthesized LLM Multi-Agent System with Conceptual Verbal Reinforcement for Enhanced Financial Decision Making
**Authors:** Yangyang Yu, Zhiyuan Yao, Haohang Li, Zhiyang Deng, Yuechen Jiang, Yupeng Cao, Zhi Chen, Jordan W. Suchow, Zhenyu Cui, Rong Liu, Zhaozhuo Xu, Denghui Zhang, Koduvayur Subbalakshmi, Guojun Xiong, Yueru He, Jimin Huang, Dong Li, Qianqian Xie  |  **Venue:** NeurIPS 2024  |  **arXiv:** 2407.06567  |  **Year:** 2024

## Core Claim
FinCon introduces a manager-analyst hierarchical multi-agent framework with dual-level risk control and conceptual verbal reinforcement (CVRF) that outperforms both DRL-based and LLM-based agents on single-stock trading and portfolio management tasks, achieving superior cumulative returns and Sharpe ratios while maintaining lower max drawdown.

## Methodology
- **Manager-Analyst hierarchy** inspired by real investment firms: 7 specialized analyst agents (news, data, 10-Q/10-K filing, analyst reports, ECC audio, stock selection, data analysis) feed distilled uni-modal insights to a single manager agent that makes final buy/sell/hold decisions
- **Dual-level risk control**: (1) Within-episode CVaR monitoring triggers risk-averse stance on sudden CVaR drops; (2) Over-episode CVRF compares winning vs. losing episodes, distills conceptual beliefs, and back-propagates them to manager and analyst prompts via textual gradient descent
- **POMDP formulation** with daily PnL as reward; portfolio weights determined by mean-variance optimization constrained by directional agent decisions
- **Data**: Real-world multi-modal financial data (prices, news, filings, ECC audio via Whisper) from Jan 2022 to Jun 2023; backbone LLM is GPT-4-Turbo at temperature 0.3
- **Learning rate analog**: Overlapping percentage of trading decisions between consecutive episodes controls prompt update magnitude (replacing edit-distance-based methods)

## Key Results
- **Single-stock trading** (8 stocks): FinCon achieves highest CR and SR on most tickers (e.g., TSLA: 82.9% CR, 1.97 SR; GOOG: 27.4% CR, 1.60 SR) while maintaining among the lowest MDD values, outperforming FinGPT, FinMem, FinAgent, GA, and DRL baselines (A2C, PPO, DQN)
- **Portfolio management**: Portfolio 1 (TSLA, MSFT, PFE) achieves 113.8% CR, 3.27 SR, 16.2% MDD vs. next-best FinRL-A2C at 19.5% CR, 0.83 SR; Portfolio 2 similarly dominant at 32.9% CR, 1.37 SR
- **CVaR ablation**: Removing within-episode CVaR drops GOOG CR from 25.1% to -1.5% (SR: 1.05 to -0.01); portfolio CR from 113.8% to 14.7%
- **Belief ablation**: Removing CVRF belief updates drops GOOG CR from 25.1% to -11.9%; portfolio CR from 113.8% to 28.4%
- Achieves strong results after only ~4 training episodes, far fewer than traditional RL agents require

## V4 Relevance
- **Manager-analyst hierarchy is directly applicable**: Specialized analyst agents processing uni-modal data (news, filings, prices) with a synthesizing manager agent maps cleanly onto a V4 three-layer architecture; the key insight is that uni-modal specialization reduces cognitive load and improves reasoning quality over monolithic multi-source prompts
- **Conceptual verbal reinforcement as belief updating**: CVRF's mechanism of distilling winning/losing episode patterns into conceptual beliefs and selectively propagating them back to agent prompts is a concrete, implementable form of inter-episode learning without weight updates -- directly relevant to building adaptive trading agents that refine their strategies over time
- **Dual-level risk control as a template**: Within-episode CVaR monitoring for real-time risk alerts combined with over-episode belief refinement provides a practical two-timescale risk management pattern; the CVaR trigger mechanism (risk-averse on CVaR drop) is simple and effective

## Limitations & Caveats
- Tested only on small portfolios (3 stocks); authors acknowledge scaling to tens of assets remains an open challenge due to LLM context window constraints and information distillation bottlenecks
- Hallucination risk increases with portfolio complexity; the system occasionally generates non-existent memory indices in multi-asset settings
- All results use GPT-4-Turbo with real-time API costs; no analysis of cost-efficiency, latency, or performance with smaller/open-source models; training period is only ~9 months (Jan-Oct 2022) with ~8 month test window

## Key References
- [30] Shinn et al. "Reflexion: Language agents with verbal reinforcement learning." NeurIPS 2024.
- [36] Zhang et al. "StockAgent: Large language model-based stock trading in simulated real-world environments." 2024.
- [33] Zhang et al. "FinAgent: A multimodal foundation agent for financial trading." 2024.
- [32] Yu et al. "FinMem: A performance-enhanced LLM trading agent with layered memory." 2023.
- [28] Tang et al. "Unleashing the potential of LLMs as prompt optimizers." 2024.
- [27] Pryzant et al. "Automatic prompt optimization with gradient descent and beam search." 2023.
- [48] Liu et al. "FinRL: A deep reinforcement learning library for automated stock trading." 2020.
- [37] Kuester et al. "Value-at-risk prediction: A comparison of alternative strategies." J. Financial Econometrics, 2006.
- [44] Sumers et al. "Cognitive architectures for language agents." 2023.
- [29] Yao et al. "Retroformer: Retrospective large language agents with policy gradient optimization." 2023.
