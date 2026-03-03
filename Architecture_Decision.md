# Architecture Decision Record: V4 — "LLM Reasons, Algorithms Execute"

> **Date:** 2026-03-02
> **Status:** APPROVED
> **Supersedes:** V3 Architecture Plan (2026-02-26)
> **Evidence base:** 12 research papers (see Reading_List.md)

---

## The Question

Should the LLM do the trading (V3 approach) or develop algorithms that trade (V2 approach)?

## The Answer

**Neither alone. Both together.**

LLMs are best at understanding **WHY** markets move (regime assessment, thesis generation, sentiment analysis, factor discovery). They are demonstrably bad at precise **WHEN** to trade (timing signals). Trained ML models (XGBoost) are the opposite: great at pattern-matching timing signals, terrible at understanding context.

V4 gives each system the job it's good at.

---

## Evidence From Papers

### The FINSABER Warning (2505.07078)

This is the most important paper in the collection. Every other LLM-as-Trader paper shows impressive results — then FINSABER stress-tests the same approaches:

- **20-year evaluation** (2004-2024) instead of 1-2 years
- **500+ symbols** instead of 3-8 cherry-picked tickers
- **Survivorship bias addressed** — includes delisted stocks
- **Result: LLM timing strategies underperform or match Buy-and-Hold**

The short-period wins in other papers likely reflect cherry-picked evaluation windows, small symbol universes, and LLMs being accidentally long-biased (most training data is bullish).

### What LLMs Are Actually Good At

| Task | LLM Advantage | Evidence |
|------|--------------|----------|
| Regime assessment | **Strong** | ATLAS adaptive-OPRO, HedgeAgents extreme market conferences |
| Thesis generation | **Strong** | Trading-R1 structured theses, TradingAgents debate |
| Sentiment analysis | **Strong** | FinAgent multimodal, ContestTrade news processing |
| Factor discovery | **Strong** | ContestTrade contest mechanism, Orchestration alpha agents |
| Risk narrative | **Strong** | HedgeAgents hedging expertise, FinCon CVaR alerts |
| Precise timing | **Weak** | FINSABER shows timing signals degrade over time |
| Order execution | **Weak** | No paper shows LLM advantage over traditional execution |
| Position sizing | **Mixed** | FinCon portfolio management works, but simple rules often suffice |

### Paper Results by Approach

| Paper | Approach | Best Result | Eval Period | Symbols |
|-------|----------|-------------|-------------|---------|
| ContestTrade | LLM-as-Trader (contest) | 52.8% CR, 3.12 SR | ~1 year | Small set |
| FinAgent | LLM-as-Trader (multimodal) | 92.3% return | Short | 6 datasets |
| HedgeAgents | LLM-as-Trader (hedging) | 71.6% ARR, 2.41 SR | 3 years | Multi-asset |
| FinCon | LLM-as-Trader (hierarchical) | 113.8% CR portfolio | ~2 years | 8 stocks |
| ATLAS | LLM-as-Trader (adaptive) | 10-30% ROI improvement | 2 months | 3 stocks |
| FLAG-Trader | LLM-as-Trader (RL-tuned) | 14-33% CR | ~1 year | 3 stocks |
| Orchestration | Alpha-Miner | 20.4% CR, 2.63 SR | 9 months | Stocks + BTC |
| Trading-R1 | Alpha-Miner (RL) | Improved SR vs baselines | Multi-period | Broad |
| **FINSABER** | **Evaluation** | **LLMs underperform B&H** | **20 years** | **500+ symbols** |

Note the pattern: impressive results in short/narrow evaluations, failure at scale.

---

## What V2 Gets Right and Wrong

### Right
- Rigorous backtesting (tri-regime, anti-gaming, full-universe)
- Proven execution (XGBoost inference is fast, deterministic)
- Systematic search (100 strategies/hour, Optuna tuning)
- Telemetry capture (every trade logged for future RL)

### Wrong
- **Discards LLM intelligence**: Reasoning about WHY markets move gets thrown away, reduced to `max_depth=6, learning_rate=0.01`
- Heavy ETL overhead (47-column Parquet is fragile)
- No adaptation (frozen models don't learn from live execution)
- LLM at lowest-value task (generating XGBoost configs)

## What V3 Gets Right and Wrong

### Right
- Intelligence preservation (agent reasoning embedded in deployed code)
- Interpretability (humans can read Python playbooks)
- Continuous adaptation (feedback loop improves regime detection)
- Zero marginal cost (local inference on Beast)

### Wrong
- **Trusts LLM timing**: Rules like `if rsi_14 < 35 and sector_trend == "up"` are exactly what FINSABER shows fails
- No independent validation (agents backtest their own playbooks)
- Rule writing is fragile (generating correct Python is harder than generating hypotheses)
- Unproven at scale

---

## V4 Resolution

| Capability | V2 Approach | V3 Approach | V4 Approach |
|-----------|-------------|-------------|-------------|
| Regime awareness | Backtest filter | First-class concept | First-class + formalized state machine |
| Strategy generation | LLM → XGBoost config | LLM → Python trading rules | LLM → Strategy seeds → XGBoost training |
| Timing decisions | XGBoost model | LLM-written rules | XGBoost model (trained on agent features) |
| Validation | Tri-regime + anti-gaming | Agent self-backtest | Tri-regime + anti-gaming + contest mechanism |
| Adaptation | Frozen after promotion | Agent rewrites playbooks | Agent re-seeds → retrain → contest re-rank |
| Intelligence preservation | Discarded | Embedded in code | Embedded in seeds + feature hypotheses + memory |
| Execution | XGBoost inference | Python rule execution | XGBoost inference |
| Cost | $300-900/mo API | ~$100-150/mo local | ~$100-150/mo local |

### The Core Design Principle

**The LLM decides WHAT to look for. XGBoost learns WHEN the pattern predicts profitable trades.**

Strategy seeds contain hypotheses ("quality names with strong balance sheets outperform in risk-off"), feature specifications ("compute ROE, debt/equity, earnings revisions, RSI"), and risk parameters ("max 3% position, 50% exposure in this regime"). The Forge trains XGBoost on these features and validates the model across all regimes.

The LLM's reasoning survives in the seed's thesis, feature choices, and risk parameters. But the timing decision — when exactly to enter and exit — is learned from data by XGBoost, which is better at this than any LLM.

---

## Paper Contributions to V4

| Paper | What V4 Takes |
|-------|--------------|
| **ContestTrade** | Contest mechanism: models compete per regime for deployment slots |
| **FinMem** | Three-tier memory: working (Redis, 24h) → episodic (TimescaleDB, 2yr) → semantic (pgvector, forever) |
| **FinCon** | Manager-analyst hierarchy + CVRF: weekly natural-language belief updates |
| **Trading-R1** | Structured investment thesis format in strategy seeds |
| **Orchestration Framework** | MCP tool interface pattern for agent data access |
| **FLAG-Trader** | Validates local small LLMs can compete with large API models |
| **FINSABER** | Core principle: LLMs must NOT make timing decisions |
| **ATLAS** | Adaptive prompt optimization for regime-aware agent behavior |
| **HedgeAgents** | Specialized risk agents + extreme market conferences |
| **TradingAgents** | Multi-agent debate structure with specialized roles |
| **FinAgent** | Multimodal data integration + tool augmentation pattern |
| **Survey (2408.06361)** | Taxonomy: LLM-as-Trader vs LLM-as-Alpha-Miner → V4 is the hybrid |

---

## References

All papers are in this repository:

**Core (Papers/):**
1. ATLAS — 2510.15949
2. Trading-R1 — 2509.11420
3. ContestTrade — 2508.00554
4. HedgeAgents — 2502.13165
5. FINSABER — 2505.07078
6. TradingAgents — 2412.20138
7. FinMem — 2311.13743
8. FinAgent — 2402.18485

**Additional (Additional Papers/):**
9. LLM Agent in Financial Trading: A Survey — 2408.06361
10. FinCon — 2407.06567
11. Orchestration Framework — 2512.02227
12. FLAG-Trader — 2502.11433
