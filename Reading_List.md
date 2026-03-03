# The Foundry - Research Reading List

## Status Key
- [x] Read
- [ ] Unread

---

## Recommended Reading Order

### Already Read

- [x] **FINSABER** - *Can LLM-based Financial Investing Strategies Outperform the Market in Long Run?*
  - Authors: Li, Kim, Cucuringu, Ma (KDD 2026)
  - arXiv: [2505.07078](https://arxiv.org/abs/2505.07078) | [PDF](Papers/FINSABER_2505.07078.pdf)
  - Key takeaway: LLM trading advantages deteriorate over longer horizons. Regime-aware risk controls matter more than framework complexity.

- [x] **TradingAgents** - *Multi-Agents LLM Financial Trading Framework*
  - Authors: Xiao, Sun, Luo, Wang (Tauric Research)
  - arXiv: [2412.20138](https://arxiv.org/abs/2412.20138) | [PDF](Papers/TradingAgents_2412.20138.pdf)
  - Key takeaway: Trading firm-inspired multi-agent architecture (analysts, researchers, traders, risk managers). Direct inspiration for V3 agent roles.

---

### Phase 1: Architecture & Coordination (Read First)

1. [ ] **ATLAS** - *Adaptive Trading with LLM AgentS Through Dynamic Prompt Optimization and Multi-Agent Coordination*
   - Authors: Papadakis, Filandrianos, Dimitriou, Lymperaiou, Thomas, Stamou
   - arXiv: [2510.15949](https://arxiv.org/abs/2510.15949) | [PDF](Papers/ATLAS_2510.15949.pdf)
   - Relevance: **Agent Research Layer** - Adaptive-OPRO prompt optimization adapts agent behavior based on market regime. Multi-agent coordination patterns more sophisticated than TradingAgents.

2. [ ] **ContestTrade** - *A Multi-Agent Trading System Based on Internal Contest Mechanism*
   - Authors: Li Zhao et al. (FinStep-AI)
   - arXiv: [2508.00554](https://arxiv.org/abs/2508.00554) | [PDF](Papers/ContestTrade_2508.00554.pdf)
   - Relevance: **Parallel Strategy Comparison** - Internal contest mechanism for agent competition. Directly applicable to running multiple strategies in tandem and selecting winners. Open source (Apache 2.0).

### Phase 2: Risk & Execution

3. [ ] **HedgeAgents** - *A Balanced-aware Multi-agent Financial Trading System*
   - Authors: Li, Zeng, Xing, Xu, Xu (WWW 2025)
   - arXiv: [2502.13165](https://arxiv.org/abs/2502.13165) | [PDF](Papers/HedgeAgents_2502.13165.pdf)
   - Relevance: **Edge Execution Layer** - Risk management and portfolio hedging across asset classes. 3 hedging agents + 1 manager with 23 tools and 3 memory types.

4. [ ] **Trading-R1** - *Financial Trading with LLM Reasoning via Reinforcement Learning*
   - Authors: Xiao, Sun, Chen, Wu, Luo, Wang (Tauric Research)
   - arXiv: [2509.11420](https://arxiv.org/abs/2509.11420) | [PDF](Papers/Trading-R1_2509.11420.pdf)
   - Relevance: **Feedback Loop Layer** - RL-trained reasoning for trading decisions. Easy-to-hard curriculum (SFT + RFT). Structured investment thesis generation.

### Phase 3: Skeptic's Lens (Read Last)

5. [ ] *Re-read FINSABER with context from papers 1-4*
   - Focus on: Which failure modes from FINSABER do the other papers actually address? Which do they ignore?

### Supplementary: Design Pattern Deep-Dives

6. [ ] **FinMem** - *A Performance-Enhanced LLM Trading Agent with Layered Memory and Character Design*
   - Authors: Yu, Li, Chen, Jiang, Li, Zhang, Liu, Suchow, Khashanah (IEEE TBD, ICLR Workshop)
   - arXiv: [2311.13743](https://arxiv.org/abs/2311.13743) | [PDF](Papers/FinMem_2311.13743.pdf)
   - Relevance: **Feedback Loop - Memory Architecture** - Three-tier memory (working/episodic/semantic) for learning from past trades. Adjustable cognitive span. Open source.

7. [ ] **FinAgent** - *A Multimodal Foundation Agent for Financial Trading: Tool-Augmented, Diversified, and Generalist*
   - Authors: Zhang, Zhao, Xia, Sun et al. (KDD 2024)
   - arXiv: [2402.18485](https://arxiv.org/abs/2402.18485) | [PDF](Papers/FinAgent_2402.18485.pdf)
   - Relevance: **Multimodal Data Integration** - Handles numeric, textual, and visual data. Tool-augmented design aligns with MCP tool interface. Dual-level reflection module.

---

## Additional Papers (Highly Cited 2025-2026)

These are landmark papers frequently cited across the LLM trading literature. Located in `Additional Papers/`.

8. [ ] **LLM Agent in Financial Trading: A Survey** - *Large Language Model Agent in Financial Trading*
   - Authors: Ding et al. (2024)
   - arXiv: [2408.06361](https://arxiv.org/abs/2408.06361) | [PDF](Additional%20Papers/LLM_Agent_Trading_Survey_2408.06361.pdf)
   - Why: **The foundational survey.** Reviewed 27 papers, defines the taxonomy (LLM-as-Trader vs LLM-as-Alpha-Miner) that every subsequent paper uses. Read this to understand how the field is organized.

9. [ ] **FinCon** - *A Synthesized LLM Multi-Agent System with Conceptual Verbal Reinforcement for Enhanced Financial Decision Making*
   - Authors: Yu, Yao, Li, Deng, Cao, Chen, Suchow et al. (NeurIPS 2024)
   - arXiv: [2407.06567](https://arxiv.org/abs/2407.06567) | [PDF](Additional%20Papers/FinCon_2407.06567.pdf)
   - Why: **Most-cited multi-agent trading paper of 2024-2025.** Manager-analyst hierarchy with Conceptual Verbal Reinforcement (CVRF). Uses CVaR risk control and text-based gradient descent for belief updates. Referenced in nearly every subsequent paper.

10. [ ] **Orchestration Framework** - *From Algorithmic Trading to Agentic Trading*
    - Authors: Li, Grover, Alpuerto, Cao, Liu (NeurIPS 2025 Workshop)
    - arXiv: [2512.02227](https://arxiv.org/abs/2512.02227) | [PDF](Additional%20Papers/Orchestration_Framework_2512.02227.pdf)
    - Why: **Uses MCP and A2A protocols** to orchestrate financial agents — the same stack you're building on. Maps every traditional algo trading component (alpha, risk, portfolio, execution, backtest) to autonomous agents. Open source: [Open-Finance-Lab/AgenticTrading](https://github.com/Open-Finance-Lab/AgenticTrading).

11. [ ] **FLAG-Trader** - *Fusion LLM-Agent with Gradient-based Reinforcement Learning for Financial Trading*
    - Authors: Xiong, Deng, Wang, Cao, Li et al. (ACL 2025 Findings)
    - arXiv: [2502.11433](https://arxiv.org/abs/2502.11433) | [PDF](Additional%20Papers/FLAG-Trader_2502.11433.pdf)
    - Why: **Small local LLMs beating large proprietary models** via RL policy gradient optimization. Directly relevant to your local inference setup — shows you don't need GPT-4 scale to compete. Parameter-efficient fine-tuning + PPO with trading rewards.

---

## V3 Architecture Mapping

| Paper | Agent Research | Edge Execution | Feedback Loop |
|-------|:---:|:---:|:---:|
| TradingAgents | *** | * | * |
| ATLAS | *** | * | ** |
| ContestTrade | ** | * | ** |
| HedgeAgents | * | *** | * |
| Trading-R1 | * | * | *** |
| FINSABER | - | - | *** (cautionary) |
| FinMem | * | - | *** |
| FinAgent | ** | * | ** |
| Survey | ** | ** | ** (overview) |
| FinCon | ** | ** | *** |
| Orchestration | *** | *** | ** |
| FLAG-Trader | * | * | *** |

`***` = primary focus, `**` = secondary, `*` = touched on, `-` = not addressed

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
