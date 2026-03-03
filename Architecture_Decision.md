# Architecture Decision Record: V4 — "LLM Reasons, Algorithms Execute"

> **Date:** 2026-03-02 (Updated: 2026-03-03)
> **Status:** APPROVED
> **Supersedes:** V3 Architecture Plan (2026-02-26)
> **Evidence base:** 18 research papers (see Reading_List.md, all digests in Digests/)

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
| **Automate Strategy Finding** | Direct validation: LLM generates alpha factors → ML optimizes weights (nearly identical to V4) |
| **RiskLabs** | Proof that direct LLM risk prediction fails catastrophically; LLMs must be information processors |
| **TradeTrap** | Adversarial attack taxonomy: 6 attack modules across 4 threat surfaces; defense requirements for V4 |
| **Walk-Forward Validation** | Concrete walk-forward protocol (252d train, 63d test, 34 folds) as minimum validation standard |
| **The New Quant (Survey)** | Comprehensive 2025 survey validating hybrid LLM+classical pattern; identifies temporal leakage risk |
| **LLM-FE** | Evolutionary feature engineering with memory buffer; template for V4's Thesis Library |

---

## Architecture Gap Analysis

How well does the expanded 18-paper evidence base cover V4's core assumptions?

### Fully Covered by Research

| V4 Component | Gap / Assumption | Paper(s) Addressing It | Status |
|--------------|-----------------|----------------------|--------|
| LLM → feature engineering → XGBoost | Core hybrid pattern needs validation | Automate Strategy Finding, The New Quant, LLM-FE | ✅ **Covered** |
| LLM generates alpha factors | LLM-as-Alpha-Miner viability | Automate Strategy Finding, LLM Agent Survey, The New Quant | ✅ **Covered** |
| Adversarial robustness | Failure modes & attack surfaces | TradeTrap | ✅ **Covered** |
| Backtesting rigor | Walk-forward validation protocol | Walk-Forward Validation, FINSABER | ✅ **Covered** |
| Risk prediction limitations | LLMs bad at direct numerical prediction | RiskLabs | ✅ **Covered** |
| Contest/tournament mechanism | Strategy selection via competition | ContestTrade | ✅ **Covered** |
| Multi-agent coordination | Agent role design & communication | TradingAgents, HedgeAgents, ATLAS, Orchestration | ✅ **Covered** |
| Memory architecture | Tiered memory for learning from trades | FinMem, FinCon | ✅ **Covered** |
| Small local LLMs viable | Don't need GPT-4 scale | FLAG-Trader, Trading-R1 | ✅ **Covered** |
| MCP/A2A tool interface | Agent orchestration protocols | Orchestration Framework | ✅ **Covered** |

### Previously Partial/Open — Now Resolved in V4 Architecture

| V4 Component | Original Gap | Resolution | Architecture Section |
|--------------|-------------|------------|---------------------|
| Contest promotion threshold (5%) | Statistical significance unclear | Replaced fixed 5% with Deflated Sharpe + Wilcoxon signed-rank test (p < 0.05) on fold-by-fold comparison | Section 4.4 (Pipeline Flow, Step 7) |
| Regime conditioning value | No direct ablation in literature | Built-in three-way ablation: regime-aware vs. regime-blind vs. regime-oracle, run in parallel during paper trading | Section 16 (Regime Conditioning Validation) |
| CVRF effectiveness for V4 | Untested in Alpha-Miner context | CVRF targets seed generation quality (not trade decisions). Empirical validation plan: 30 cycles without → 30 with → compare Forge pass rates. Kill switch: simplify if <10% improvement | Section 15 (CVRF in Alpha-Miner Context) |
| Memory tier contribution | No ablation of specific tiers | Instrumented memory access logging + per-tier influence metrics. Remove any tier with <5% influence rate after 90 days | Section 17 (Memory Tier Contribution Tracking) |
| Cross-asset generalization | Mostly equity-focused | Deferred to V4.4. V4.0-V4.3 focus on US equities only. Architecture is asset-class agnostic (regime → model → execution) but validation is equity-only | Section 11 (Maturity Path, V4.4) |
| Transaction costs & slippage | No paper models this | Full transaction cost model: commission + spread + square-root market impact. Applied at every Forge stage. Impact model calibrated monthly from live fills | Section 4.6 (Transaction Cost Model) |
| Live deployment validation | Paper-to-production gap | Four-gate deployment: Backtest → Paper (60d) → Small Live (90d) → Production Scaling. Degradation budget with kill switches per metric | Section 14 (Paper-to-Production Transition Protocol) |

### Newly Addressed Threats (Not in Original Gap Analysis)

| Threat | Source | V4 Response | Architecture Section |
|--------|--------|-------------|---------------------|
| Data fabrication / MCP hijacking | TradeTrap | Cross-validation of data sources, anomaly detection, checksummed tool responses | Section 13.2-13.3 |
| Prompt injection via Idea Agent | TradeTrap | Isolated context processing, schema validation, deterministic embeddings | Section 13.3 |
| Memory poisoning | TradeTrap | Append-only hash chains, signed CVRF messages, deterministic embeddings | Section 13.3 |
| State tampering | TradeTrap | Every-cycle broker reconciliation, data-hash verification on regime assessments | Section 13.3 |
| Temporal leakage | The New Quant | Strict information-set discipline in Walk-Forward Validation, no lookahead in Seed Compiler | Section 4.5 |
| ATLAS Reflection Paradox | ATLAS | Avoid naive self-critique; use quantitative windowed evaluation for prompt updates | Section 18 (Open Questions) |

### Key Takeaways from Gap Analysis (Updated)

1. **All 17 original gaps now have architectural resolutions.** 10 are covered by research, 7 are resolved by new V4 architecture sections with empirical validation plans.
2. **Core V4 design is well-supported**: The hybrid LLM-reasons/algorithm-executes pattern is validated by multiple independent papers (Automate Strategy Finding, RiskLabs, The New Quant survey).
3. **Adversarial robustness is architecturally addressed**: TradeTrap's 6 attack vectors map to specific defenses with zero-trust boundaries between Agent Research and Edge Execution.
4. **Backtesting rigor exceeds literature standards**: Walk-Forward Validation + FINSABER + transaction cost modeling + Deflated Sharpe + statistical significance testing.
5. **Empirical validation is built into the maturity path**: Regime ablation, CVRF validation, and memory tier contribution are measured during paper trading — not assumed to work.
6. **Remaining risk is execution-layer**: Live deployment introduces unknowns (broker reliability, latency, market impact) that no paper can address. The four-gate protocol with degradation budgets manages this.

---

## References

All papers are in this repository with structured digests in `Digests/`:

**Core (Papers/):**
1. ATLAS — 2510.15949 | [Digest](Digests/ATLAS.md)
2. Trading-R1 — 2509.11420 | [Digest](Digests/Trading_R1.md)
3. ContestTrade — 2508.00554 | [Digest](Digests/ContestTrade.md)
4. HedgeAgents — 2502.13165 | [Digest](Digests/HedgeAgents.md)
5. FINSABER — 2505.07078 | [Digest](Digests/FINSABER.md)
6. TradingAgents — 2412.20138 | [Digest](Digests/TradingAgents.md)
7. FinMem — 2311.13743 | [Digest](Digests/FinMem.md)
8. FinAgent — 2402.18485 | [Digest](Digests/FinAgent.md)

**Additional (Additional Papers/):**
9. LLM Agent in Financial Trading: A Survey — 2408.06361 | [Digest](Digests/LLM_Agent_Trading_Survey.md)
10. FinCon — 2407.06567 | [Digest](Digests/FinCon.md)
11. Orchestration Framework — 2512.02227 | [Digest](Digests/Orchestration_Framework.md)
12. FLAG-Trader — 2502.11433 | [Digest](Digests/FLAG_Trader.md)
13. Automate Strategy Finding — 2409.06289 | [Digest](Digests/Automate_Strategy_Finding.md)
14. RiskLabs — 2404.07452 | [Digest](Digests/RiskLabs.md)
15. TradeTrap — 2512.02261 | [Digest](Digests/TradeTrap.md)
16. Walk-Forward Validation — 2512.12924 | [Digest](Digests/Walk_Forward_Validation.md)
17. The New Quant (Survey) — 2510.05533 | [Digest](Digests/The_New_Quant.md)
18. LLM-FE — 2503.14434 | [Digest](Digests/LLM_Feature_Engineering.md)
