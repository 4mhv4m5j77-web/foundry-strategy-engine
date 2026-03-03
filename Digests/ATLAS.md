# ATLAS: Adaptive Trading with LLM AgentS Through Dynamic Prompt Optimization and Multi-Agent Coordination
**Authors:** Charidimos Papadakis, Angeliki Dimitriou, Giorgos Filandrianos, Maria Lymperaiou, Konstantinos Thomas, Giorgos Stamou  |  **Venue:** Preprint (NTUA / AILS Lab)  |  **arXiv:** 2510.15949  |  **Year:** 2026

## Core Claim
Adaptive-OPRO, a sequential prompt optimization method that dynamically rewrites a trading agent's instruction prompt using delayed, noisy market feedback, consistently outperforms static prompts and reflection-based adaptation across seven LLM backbones and three market regimes (bearish-volatile, sideways, bullish).

## Methodology
- **Three-layer architecture:** (1) Market Intelligence Pipeline with three specialized analyst agents (Market, News, Fundamental); (2) Decision & Execution Layer centered on a Central Trading Agent that emits order-level actions (type, side, size, timing, price); (3) Feedback Mechanism using Adaptive-OPRO for prompt evolution
- **Adaptive-OPRO:** Extends OPRO to sequential settings via template separation (static instruction block vs. dynamic run-time content), windowed evaluation over K=5 trading days, and a bounded OPRO-style score mapping cumulative ROI to s in [0,100]. Only the static instruction block is editable; placeholders and output schemas stay fixed
- **Evaluation:** Three regime-specific 2-month windows (Apr 28-Jun 28, 2025) on LLY (bearish), XOM (sideways), NVDA (bullish); daily decision cadence; $100K starting capital; executed via StockSim order-level simulator
- **Seven LLM backbones:** GPT-o3, GPT-o4-mini, Claude Sonnet 4 (with/without thinking), LLaMA 3.3-70B, Qwen3-235B, Qwen3-32B; each run 3x to capture stochastic variance
- **Baselines:** Static expert-tuned prompt (Baseline), weekly Reflection, plus five non-LLM strategies (Buy & Hold, MACD, SMA, SLMA, Bollinger Bands)

## Key Results
- **Adaptive-OPRO wins on ROI across all models in the bearish regime:** GPT-o3 achieves +9.02% ROI (vs. -6.11% baseline), GPT-o4-mini +9.06% (vs. -1.30% baseline), Qwen3-235B +1.33% (vs. -1.78% baseline)
- **Reflection paradox:** Reflection shows strong negative correlation with baseline quality (r=-0.78, p<0.05) -- models with competent baselines deteriorate more under reflection, suggesting it amplifies noise rather than stabilizing behavior
- **Adaptive-OPRO improvements are independent of baseline strength** (r=0.05 between baseline ROI and improvement), indicating it alters decision behavior rather than just scaling existing ability
- **Ablation (GPT-o4-mini):** Market Analyst is the most critical component; removing it causes the largest performance drops. News + Market combined removal degrades all regimes substantially, confirming the signals are complementary, not redundant
- **More information is not always better:** Additional modalities help regime-dependently; news analysis is critical in sideways markets but can reduce returns in bullish ones

## V4 Relevance
- **Adaptive-OPRO as a prompt evolution pattern for V4's Idea Agent:** The template separation principle (static instructions vs. dynamic runtime content) directly applies to V4's architecture -- LLM-generated strategies can be iteratively refined via performance feedback while keeping the generation interface stable
- **Multi-agent analyst pipeline validates V4's separation of concerns:** Dedicated Market/News/Fundamental analysts feeding a central decision agent mirrors V4's approach of specialized information preparation agents upstream of an execution layer, with each agent contributing non-redundant signals
- **Reflection is harmful for strong baselines -- prefer windowed scoring:** V4 should avoid naive reflection/self-critique loops and instead use quantitative windowed evaluation (like Adaptive-OPRO's K=5 rolling windows) to drive prompt updates, especially when the base strategy is already competent

## Limitations & Caveats
- **Narrow asset/regime scope:** Only three liquid U.S. equities over two-month windows; no cross-asset, cross-sector, or longer-horizon generalization tested. Results are behavioral evidence, not market-wide performance claims
- **Simulated execution only:** StockSim abstracts away slippage, partial fills, latency, and intraday dynamics; real-world alpha would be lower. Each config run only 3x, limiting statistical power
- **No directional-only ablation:** The paper does not isolate whether gains come from better entry timing, position sizing, or risk management, making it hard to attribute Adaptive-OPRO's edge to specific decision dimensions

## Key References
- Yang et al., 2024 -- OPRO: Large language models as optimizers (foundational prompt optimization method)
- Li et al., 2024 -- CryptoTrade: Reflective LLM agent for zero-shot cryptocurrency trading (EMNLP; reflection baseline)
- Papadakis et al., 2025 -- StockSim: Dual-mode order-level simulator for evaluating multi-agent LLMs in financial markets
- Xiao et al., 2025 -- TradingAgents: Multi-agent LLM financial trading framework
- Yu et al., 2024 -- FINCON: Synthesized LLM multi-agent system with conceptual verbal reinforcement
- Yu et al., 2023 -- FinMem: Performance-enhanced LLM trading agent with layered memory
- Zhou et al., 2025 -- Multi-agent design: Optimizing agents with better prompts and topologies
- Ding et al., 2025 -- TradeExpert: Revolutionizing trading with mixture of expert LLMs
- Xiong et al., 2025 -- FlagTrader: Fusion LLM-agent with gradient-based reinforcement for financial trading
- Fatouros et al., 2025 -- MarketSenseAI 2.0: Enhancing stock analysis through LLM agents
