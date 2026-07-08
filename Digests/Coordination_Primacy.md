# Toward Reliable Evaluation of LLM-Based Financial Multi-Agent Systems: Taxonomy, Coordination Primacy, and Cost Awareness
**Authors:** Phat Nguyen (Georgia Tech), Thang Pham (Adobe)  |  **Venue:** DMO-FinTech Workshop, PAKDD 2026  |  **arXiv:** 2603.27539  |  **Year:** 2026

> **Evidentiary weight note:** analytical survey — no new empirical results; its central hypothesis is explicitly presented as falsifiable-but-unvalidated. Use for framing and checklists, not as evidence.

## Core Claim
Cross-system comparisons in the LLM-trading-MAS literature are unreliable because of five systematic evaluation failures; the structural pattern that survives them motivates the **Coordination Primacy Hypothesis (CPH)** — the inter-agent coordination protocol drives trading decision quality more than model scaling — and the **Coordination Breakeven Spread (CBS)** metric, which asks whether coordination's signal gain clears its cost (spread + latency) per instrument.

## Methodology
- Four-dimensional taxonomy (architecture pattern / coordination mechanism / memory architecture / tool integration) applied to 12 multi-agent systems + 2 single-agent baselines (FinCon, TradingAgents, HedgeAgents, ContestTrade, FinVision, TradingGPT, QuantAgents, AlphaAgents, ElliottAgents, FinRobot, Agentic RAG, REITs system; FinMem, FinAgent).
- Five evaluation-failure criteria scored per system: contamination control, point-in-time universe, rolling-window reporting, net-of-cost returns, regime coverage.
- CPH supporting evidence explicitly TIERED: Tier 1 = AMA live multi-market benchmark (weaker models in sophisticated coordination beat frontier models in linear pipelines — "strongly suggestive"); Tier 2 = author-reported ablations (FinCon/TradingAgents: removing coordination costs 15–30% Sharpe vs 5–8% for model downgrade); Tier 3 = theoretical scaling arguments. Paper states definitive validation is BLOCKED by the evaluation failures (the needed controlled experiment can't be run on current infrastructure).
- CBS: coordination optimal only if bid-ask spread < Δprice-improvement/2; debate latency priced at 5–20bps adverse movement per 2-round debate, 1–3s/round.

## Key Results
- **No surveyed system satisfies more than 2 of 5 evaluation criteria.** FinCon/HedgeAgents/FinAgent/QuantAgents 2/5; TradingAgents/ContestTrade/FinVision 1/5; FinMem 0/5 — and FinMem's reported +23% on MSFT REVERSES to −22% under FINSABER controlled re-evaluation (the sign-reversal exhibit for backtest overfitting).
- Only FINSABER and StockBench model transaction costs at all; HedgeAgents' 405%, FinCon's 114%, ContestTrade's 52.8% are all GROSS of costs; 10–20bps round-trip compounds to 25–50 pts of annual drag for daily-trading systems.
- Cost envelope: 3–7 agents × 2–3 interaction rounds is the practical budget consensus; ~$0.50–2.00 per daily decision at API prices; coordination economically viable mainly in liquid, narrow-spread instruments.
- Practical minimums it converges on: a 3-stage pipeline (signal → risk gate → execution) suffices for most production settings; hybrid small-model/frontier escalation is the named cost lever; memory needs regime-change-triggered belief revision, not just decay.

## V4 Relevance
- **CPH aligns with our independently-derived priors** (Harness-Bench same-model spreads, AdaptOrch topology-routing, notes §2.2/§2.6): where you spend design effort is coordination/harness, not backbone shopping — consistent with the Foundry's benchmark-then-freeze model policy and the Refinery's emphasis on spine/topology over model scale. Tier-2 numbers (coordination ablation ≫ model swap) are the citable version, with the author-reported caveat.
- **The 5-failure checklist is a compact review rubric** for any paper entering our evidence base — it overlaps FINSABER + Alpha Illusion P1/P2/P5 but adds rolling-window reporting and regime coverage as named criteria; useful as the standing "should we believe this paper" screen in future research scans.
- **CBS is a useful cost-accounting frame for OUR seed economics**, adapted: coordination cost here = generation/eval token spend + iteration latency, and the breakeven question becomes "does another proposer/critic/debate round clear its cost in deflated-score improvement" — the same discipline as the trial-count ledger applied to orchestration depth. Also directly relevant if seeds' derived strategies ever trade less-liquid names (spread-dependent viability of any coordination-heavy signal).
- Reinforces two decided bets: sequential pass-through pipelines are the weakest pattern (our fixed-DAG-with-deterministic-gates is not that), and frontier-escalation hybrid tiering (our two-tier local/frontier design) is the cost-efficient shape.

## Limitations & Caveats
- Analytical only; CPH evidence is suggestive tiers, not controlled experiment — the paper itself says the validation infrastructure doesn't exist yet.
- Workshop venue, two authors; Table-1 metrics are transcribed from papers with inconsistent protocols (which is the paper's own point).
- CBS is a conceptual threshold — the paper concedes empirical CBS values "cannot be computed from currently published results"; asset-class regions in its figure are illustrative.

## Key References
- Qian et al. 2025 — AMA live multi-market trading benchmark (arXiv 2510.11695; the Tier-1 evidence)
- Li et al. 2025 — FINSABER (the controlled re-evaluation source for the FinMem sign reversal)
- Chen et al. 2025 — StockBench (arXiv 2510.02209; post-cutoff real-market evaluation)
- Liang et al. 2024 — Degeneration-of-Thought (EMNLP; debate convergence failure)
- Xiong et al. 2025 — experience-following behavior in agent memory (arXiv 2505.16067; RAG anchoring bias)
- Belcak et al. 2025 — Small language models are the future of agentic AI (arXiv 2506.02153; the SLM-specialist direction)
