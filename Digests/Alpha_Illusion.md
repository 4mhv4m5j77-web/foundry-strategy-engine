# The Alpha Illusion: Reported Alpha from LLM Trading Agents Should Not Be Treated as Deployment Evidence
**Authors:** Yuxuan Ye, Jun Han, Ao Hu, Juncheng Bu, Yiyi Chen, Liangjian Wen, Danilo Mandic, Danny Dongning Sun, Xu Yinghui, Zenglin Xu (Fudan, SUFE, SWUFE, Northeastern, Imperial, Peng Cheng Lab)  |  **Venue:** arXiv preprint (position paper)  |  **arXiv:** 2605.16895  |  **Year:** 2026

## Core Claim
Reported alpha from end-to-end LLM trading agents (FinCon, FinMem, TradingAgents, FinAgent, QuantAgent, FLAG-Trader) must not be read as deployment evidence until it survives structural validity tests. Beyond evaluation confounds (temporal contamination, unmodeled friction, short-window Sharpe noise), three STRUCTURAL mismatches remain even under perfect evaluation: language confidence is not tradable probability; narrative reasoning is not numerical execution; parametric priors in model weights are undisclosed implicit factor exposures.

## Methodology
- Position paper + reproduction harness (github.com/hj1650782738/Trading): 1-year (2025-01→2026-01), 5-ticker equal-weight reproduction of TradingAgents and QuantAgent with layered gross-to-net friction cleansing (commission + token cost + spread + market impact per the Almgren-Chriss decomposition, eq. 1).
- Friction coverage audit: 8 standard cost components × 5 published frameworks — 35 of 40 cells unmodeled; only commissions are modeled by all five.
- Sharpe-uncertainty analysis via Lo (2002) SE approximation: CI half-widths overlaid on published headline Sharpes (FinBen's GPT-4 FinTrade Sharpe = 1.51±1.08 — std exceeds half the mean).
- **P1–P6 minimum reporting protocol suite**, tiered by claim strength: P1 temporal integrity (cutoff/post-cutoff windows), P2 dynamic universe (delistings, survivorship), P3 counterfactual robustness (view-flip/confidence/position monotonicity under reverse evidence), P4 epistemic calibration (ECE, reliability curves on any confidence used for sizing), P5 realistic implementation (full gross-to-net incl. token/latency cost), P6 multi-agent disaggregation (single-agent baseline, role similarity, disagreement rate, net delta).

## Key Results
- Reproduction: with frictions charged, TradingAgents portfolio Sharpe 0.43→0.22 and QuantAgent −0.96→−1.15; net buy-and-hold beats both agents on 4 of 5 tickers. On TSLA, TradingAgents CR −2.01% gross → −10.17% net.
- Contamination anchor (citing Profit Mirage, 2510.07920): crossing the pretraining cutoff drops FinMem total return ≈71.85% and QuantAgent Sharpe ≈51.48%.
- Parametric Prior Lock-in (citing Lee et al. 2025): all 6 LLMs tested show significant Tech-vs-Defensive sector tilt (p<0.001); at 60% counter-evidence the strongest-prior model flips its view in ≈8% of cases vs ≈30% for the lowest-bias model; persona prompts, voting, and debate do NOT remove the shared prior — multi-agent agreement ≠ independent-expert agreement.
- Multi-agent debate anchor: across 36 configurations, debate wins <20% of the time; added rounds/agents may degrade (Zhang et al. 2025).
- Knowledge is not point-in-time reliable in EITHER direction: LLMs answer large-cap revenue questions at 54.17% (2017 firms) vs 6.32% (1995 firms) — recency/size bias inside the cutoff (Shah et al. 2025).

## V4 Relevance
- **Independent convergence on the core thesis:** the paper's "modular alternative" (LLM as schema-bound information extractor at Stage 1, independent calibration/risk/execution modules downstream, LLM never final decision authority) IS "LLM reasons, algorithms execute" — arrived at from the evaluation-rigor direction. Strongest external validation of the Refinery division of authority to date.
- **P1–P6 ports directly into the eval discipline (blueprint E-stage + skill review rubric):** P1/P2 are already covered (PIT anchors, survivorship-free FirstRateData universe); P3 (counterfactual robustness / reverse-evidence monotonicity probes) and P4 (ECE on any confidence field before it ever gains authority — ours is metadata-only, keep it that way) are NEW checks worth adopting; P5's token-cost-in-net-return accounting belongs in the ranker contract's cost model; P6 is a caution for any future S7 frontier-panel work — report disaggregation, not just panel wins.
- **Parametric Prior Lock-in names a real generation risk for us:** seed proposers may carry shared sector/style tilts that surface as "autonomous analysis." Countermeasures already in the blueprint (heterogeneous proposers D3, originality/crowding constraints §4.5, archive diversity) get a new probe: measure per-lane sector/style tilt of generated seeds vs universe base rates; the acceptance constraints should treat systematic tilt like crowding.

## Limitations & Caveats
- Position paper: the empirical core is one 1-year, 5-ticker reproduction of two systems; friction parameters (impact κ, β) are assumed-standard, not fitted; token costs amortized portfolio-level.
- P3's monotonicity diagnostic is proposed, not validated at scale; PPL is offered as an "explanatory framework rather than an established unified mechanism."
- Cross-domain extrapolations acknowledged by the authors (Guo et al. 2017 calibration evidence is from image classifiers; finance-side calibration evidence is thinner).
- The paper defends exploratory architecture research — it does not show LLM-seeded, ML-executed hybrids (our class) fail; it explicitly endorses that class (Stage-1 role, Lopez-Lira/Tang semantic-signal evidence).

## Key References
- Li et al. 2025 — Profit Mirage: information leakage in LLM financial agents (arXiv 2510.07920; the contamination quantification)
- Lee et al. 2025 — "Your AI, not your view": LLM bias in investment analysis (ICAIF; the PPL evidence base)
- Lo 2002 — The statistics of Sharpe ratios (short-window SE)
- Harvey, Liu, Zhu 2016 — cross-section of expected returns (t≈3.0 multiple-testing hurdle)
- Merchant & Levy 2025 — Divergence Decoding: look-ahead bias embedded in model parameters (arXiv 2512.06607)
- Zhang et al. 2025 — Stop Overvaluing Multi-Agent Debate (arXiv 2502.08788)
- Chen et al. 2025 — StockBench: contamination-free real-market LLM agent evaluation (arXiv 2510.02209)
- Yuan et al. 2026 — TraderBench: conceptual-vs-computational gap in adversarial capital markets (arXiv 2603.00285)
- Gu, Kelly, Xiu 2020 — Empirical asset pricing via ML (the R² ceiling)
- Novy-Marx & Velikov 2025 — AI-powered (finance) scholarship (industrialized HARKing warning)
