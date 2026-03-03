# Orchestration Framework for Financial Agents: From Algorithmic Trading to Agentic Trading
**Authors:** Jifeng Li, Arnav Grover, Abraham Alpuerto, Yupeng Cao, Xiao-Yang Liu  |  **Venue:** Workshop on Generative AI in Finance, NeurIPS 2025  |  **arXiv:** 2512.02227  |  **Year:** 2025

## Core Claim
Maps each component of a traditional algorithmic trading pipeline (data, alpha, risk, portfolio, execution, backtest, audit) to specialized LLM-powered agents coordinated by a central orchestrator using MCP for control messages and A2A for inter-agent communication, with a shared memory agent for state persistence.

## Methodology
- Structured around agent pools per pipeline stage: Data Agents, Alpha Agents, Risk Agents, Portfolio Agents, Execution Agents, Backtest Agents, and Audit Agents, all managed by an Orchestrator + Planner layer
- Orchestrator sends control messages via MCP (task id, schemas, policy flags, timeout, retry budget); agents communicate peer-to-peer via A2A protocol; Memory Agent stores structural summaries (never evaluation-window data)
- Alpha Agents propose factor structures from literature only; all numerical signal computation and backtesting handled by tool-based modules, keeping realized returns hidden from LLMs to prevent data leakage
- Stock backtest: 7 equities (AAPL, MSFT, GOOGL, JPM, TSLA, NVDA, META), hourly bars, 09/2022-01/2025 via Polygon + yfinance; BTC backtest: minute bars, 07/27-08/13/2025 via Polygon
- Models used: GPT-4o for factor survey and feature list drafting; XGBoost for BTC return prediction; walk-forward evaluation with rolling training windows

## Key Results
- Stocks: 20.42% total return, Sharpe 2.63, volatility 11.83%, max drawdown -3.59% vs. S&P 500 at 15.97% return (Sharpe 1.86) over the same period
- BTC: 8.39% cumulative return over 17 days vs. Buy&Hold +3.80%, Sharpe 0.378 vs. 0.170, max drawdown -2.80% vs. -5.26%
- Equal-weighted benchmark beat the agentic approach on raw return (47.46%) but with far higher volatility (22.54%) and max drawdown (-16.21%); the agentic system trades off return for tighter risk control
- BTC strategy: 64.7% win rate, ~1.04 trades/day, average holding time 16.07 hours, Calmar ratio 166.06 vs. 23.30 for Buy&Hold

## V4 Relevance
- Provides a concrete blueprint for two-protocol orchestration: MCP for top-down control (task dispatch, schemas, heartbeats, timeouts) and A2A for lateral agent-to-agent communication. V4 could adopt this split to separate command flow from information sharing between strategy, risk, and execution agents.
- The strict information barrier -- LLM agents never see realized returns or evaluation-window labels, with all numerical computation delegated to tool modules -- is a critical anti-leakage pattern directly applicable to any LLM-orchestrated trading architecture.
- Memory Agent architecture (time-stamped structural summaries, no performance data, peer failover) offers a practical pattern for cross-cycle state persistence without contaminating agent prompts with outcome data.

## Limitations & Caveats
- Stock universe is only 7 mega-cap names with hourly data; BTC test is only 17 days of minute data. Neither test covers a drawdown regime, multiple asset classes, or longer horizons.
- Equal-weighted benchmark substantially outperforms on raw return (47.46% vs 20.42%), suggesting the risk-control gates may be overly conservative or the alpha signal generation is weak relative to simple diversification.
- No ablation studies on individual agent contributions, protocol overhead, or comparison against simpler non-agentic pipelines doing the same walk-forward backtest.

## Key References
- [15] Model Context Protocol (MCP) -- modelcontextprotocol.io, 2025
- [26] Xiao et al. TradingAgents: Multi-agent LLM financial trading framework, arXiv:2412.20138, 2025
- [11] Li et al. R&D-agent-quant: Multi-agent framework for data-centric factors, arXiv:2505.15155, 2025
- [31] Yu et al. Finmem: Performance-enhanced LLM trading agent with layered memory, IEEE Trans. Big Data, 2025
- [30] Yu et al. Fincon: Synthesized LLM multi-agent system with verbal reinforcement, NeurIPS 2024
- [10] Li et al. CAMEL: Communicative agents for LLM society, NeurIPS 2023
- [25] Wu et al. AutoGen: Next-gen LLM applications via multi-agent conversations, COLM 2024
- [9] Kissell. The Science of Algorithmic Trading and Portfolio Management, 2013
- [12] Liu et al. FinRL-Meta: Near-real market environments for data-driven deep RL, NeurIPS 2022
