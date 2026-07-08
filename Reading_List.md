# The Foundry - Research Reading List

## Status Key
- [x] Read (digest available)
- [ ] Unread

> **Digests:** All papers have been converted to structured markdown digests in [`Digests/`](Digests/). Each digest is ~500-800 tokens covering Core Claim, Methodology, Key Results, V4 Relevance, Limitations, and Key References.

---

## Core Papers (Papers/)

| # | Paper | arXiv | Venue | Digest |
|---|-------|-------|-------|--------|
| 1 | **FINSABER** — Can LLM-based Financial Investing Strategies Outperform the Market in Long Run? | [2505.07078](https://arxiv.org/abs/2505.07078) | KDD 2026 | [Digest](Digests/FINSABER.md) |
| 2 | **TradingAgents** — Multi-Agents LLM Financial Trading Framework | [2412.20138](https://arxiv.org/abs/2412.20138) | Tauric Research | [Digest](Digests/TradingAgents.md) |
| 3 | **ATLAS** — Adaptive Trading with LLM AgentS | [2510.15949](https://arxiv.org/abs/2510.15949) | Preprint | [Digest](Digests/ATLAS.md) |
| 4 | **ContestTrade** — Multi-Agent Trading via Internal Contest Mechanism | [2508.00554](https://arxiv.org/abs/2508.00554) | FinStep-AI | [Digest](Digests/ContestTrade.md) |
| 5 | **HedgeAgents** — Balanced-aware Multi-agent Financial Trading | [2502.13165](https://arxiv.org/abs/2502.13165) | WWW 2025 | [Digest](Digests/HedgeAgents.md) |
| 6 | **Trading-R1** — Financial Trading with LLM Reasoning via RL | [2509.11420](https://arxiv.org/abs/2509.11420) | Tauric Research | [Digest](Digests/Trading_R1.md) |
| 7 | **FinMem** — LLM Trading Agent with Layered Memory | [2311.13743](https://arxiv.org/abs/2311.13743) | ICLR Workshop | [Digest](Digests/FinMem.md) |
| 8 | **FinAgent** — Multimodal Foundation Agent for Financial Trading | [2402.18485](https://arxiv.org/abs/2402.18485) | KDD 2024 | [Digest](Digests/FinAgent.md) |

## Additional Papers (Additional Papers/)

| # | Paper | arXiv | Venue | Digest |
|---|-------|-------|-------|--------|
| 9 | **LLM Agent Trading Survey** — Large Language Model Agent in Financial Trading | [2408.06361](https://arxiv.org/abs/2408.06361) | Columbia/NYU 2024 | [Digest](Digests/LLM_Agent_Trading_Survey.md) |
| 10 | **FinCon** — Multi-Agent System with Conceptual Verbal Reinforcement | [2407.06567](https://arxiv.org/abs/2407.06567) | NeurIPS 2024 | [Digest](Digests/FinCon.md) |
| 11 | **Orchestration Framework** — From Algorithmic to Agentic Trading | [2512.02227](https://arxiv.org/abs/2512.02227) | NeurIPS 2025 Workshop | [Digest](Digests/Orchestration_Framework.md) |
| 12 | **FLAG-Trader** — Fusion LLM-Agent with Gradient-based RL | [2502.11433](https://arxiv.org/abs/2502.11433) | ACL 2025 Findings | [Digest](Digests/FLAG_Trader.md) |
| 13 | **Automate Strategy Finding** — LLM in Quant Investment | [2409.06289](https://arxiv.org/abs/2409.06289) | EMNLP 2025 | [Digest](Digests/Automate_Strategy_Finding.md) |
| 14 | **RiskLabs** — Financial Risk Using LLM on Multimodal Data | [2404.07452](https://arxiv.org/abs/2404.07452) | ICAIF '24 Workshop | [Digest](Digests/RiskLabs.md) |
| 15 | **TradeTrap** — Adversarial Vulnerability Testing for LLM Trading Agents | [2512.02261](https://arxiv.org/abs/2512.02261) | Preprint 2025 | [Digest](Digests/TradeTrap.md) |
| 16 | **Walk-Forward Validation** — Rigorous WFO Framework for Market Signals | [2512.12924](https://arxiv.org/abs/2512.12924) | Preprint 2025 | [Digest](Digests/Walk_Forward_Validation.md) |
| 17 | **The New Quant** — LLMs in Financial Prediction and Trading (Survey) | [2510.05533](https://arxiv.org/abs/2510.05533) | Columbia 2025 | [Digest](Digests/The_New_Quant.md) |
| 18 | **LLM-FE** — Automated Feature Engineering with LLMs | [2503.14434](https://arxiv.org/abs/2503.14434) | Preprint 2025 | [Digest](Digests/LLM_Feature_Engineering.md) |
| 19 | **AlphaAgent** — LLM-Driven Alpha Mining with Regularized Exploration | [2502.16789](https://arxiv.org/abs/2502.16789) | KDD 2025 | [Digest](Digests/AlphaAgent.md) |

---

## V4 Architecture Mapping

| Paper | Strategy Gen | Execution | Risk/Validation | Feedback Loop |
|-------|:---:|:---:|:---:|:---:|
| FINSABER | - | - | *** | *** (cautionary) |
| TradingAgents | *** | * | * | * |
| ATLAS | *** | * | * | ** |
| ContestTrade | ** | * | ** | ** |
| HedgeAgents | * | *** | *** | * |
| Trading-R1 | * | * | * | *** |
| FinMem | * | - | * | *** |
| FinAgent | ** | * | * | ** |
| Survey (2408) | ** | ** | ** | ** (overview) |
| FinCon | ** | ** | *** | *** |
| Orchestration | *** | *** | ** | ** |
| FLAG-Trader | * | * | * | *** |
| Automate Strategy | *** | - | * | ** |
| RiskLabs | * | - | *** | * |
| TradeTrap | - | * | *** | * (adversarial) |
| Walk-Forward | - | - | *** | ** |
| The New Quant | ** | ** | ** | ** (survey) |
| LLM-FE | *** | - | * | ** |
| AlphaAgent | *** | - | ** | *** (decay defense) |

`***` = primary focus, `**` = secondary, `*` = touched on, `-` = not addressed

---

## 2026 Additions — post-blueprint research scan (2026-07-04)

Pulled + digested during the Refinery multi-agent-skill design review (foundry-core
`plans/refinery-multiagent-skill/`). PDFs in `Additional Papers/` where arXiv serves one;
Coordination Primacy has no full PDF on arXiv (1-page cover only) — digest is from the HTML rendition.

| # | Paper | arXiv | Venue | Digest | Why it's here |
|---|-------|-------|-------|--------|---------------|
| 20 | **The Alpha Illusion** — Reported Alpha from LLM Trading Agents ≠ Deployment Evidence | [2605.16895](https://arxiv.org/abs/2605.16895) | Preprint 2026 (position) | [Digest](Digests/Alpha_Illusion.md) | P1–P6 minimum-evidence protocols; parametric-prior-lock-in; independent convergence on "LLM as auditable upstream interface" |
| 21 | **QuantaAlpha** — Evolutionary Framework for LLM-Driven Alpha Mining | [2602.07085](https://arxiv.org/abs/2602.07085) | Preprint 2026 | [Digest](Digests/QuantaAlpha.md) | Trajectory-level mutation/crossover (idea-tier input); confirms evolutionary-archive + generation-gate bets with ablations |
| 22 | **BlindTrade** — Anonymization-First Framework for Portfolio Optimization | [2603.17692](https://arxiv.org/abs/2603.17692) | ICLR 2026 FinAI wkshp | [Digest](Digests/BlindTrade.md) | Anonymization probe + negative-control shuffle test as generator-validation tools; realistic tiny-IC anchor |
| 23 | **Profit Mirage** — Revisiting Information Leakage in LLM Financial Agents | [2510.07920](https://arxiv.org/abs/2510.07920) | Preprint 2025 | [Digest](Digests/Profit_Mirage.md) | PC/CI/IDS leakage-measurement triad; causal fine-tune leakage demo; FactFin ablation supports "LLM generates strategy, never predicts" |
| 24 | **Coordination Primacy** — Reliable Evaluation of LLM Financial MAS | [2603.27539](https://arxiv.org/abs/2603.27539) | PAKDD 2026 wkshp | [Digest](Digests/Coordination_Primacy.md) | 5-failure evaluation rubric; CPH (coordination > model scale, tiered evidence); CBS cost-of-coordination frame. Analytical survey — framing, not evidence |

---

## Open Source Implementations

| Paper | Repo | License |
|-------|------|---------|
| TradingAgents | [TauricResearch/TradingAgents](https://github.com/TauricResearch/TradingAgents) | - |
| ContestTrade | [FinStep-AI/ContestTrade](https://github.com/FinStep-AI/ContestTrade) | Apache 2.0 |
| FINSABER | [waylonli/FINSABER](https://github.com/waylonli/FINSABER) | - |
| FinMem | [pipiku915/FinMem-LLM-StockTrading](https://github.com/pipiku915/FinMem-LLM-StockTrading) | - |
| Trading-R1 | [TauricResearch/Trading-R1](https://github.com/TauricResearch/Trading-R1) | - |
| Orchestration | [Open-Finance-Lab/AgenticTrading](https://github.com/Open-Finance-Lab/AgenticTrading) | - |
| AlphaAgent | [RndmVariableQ/AlphaAgent](https://github.com/RndmVariableQ/AlphaAgent) | - |
