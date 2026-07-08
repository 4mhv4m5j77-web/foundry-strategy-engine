# Profit Mirage: Revisiting Information Leakage in LLM-based Financial Agents
**Authors:** Xiangyu Li, Yawen Zeng, Xiaofen Xing, Jin Xu, Xiangmin Xu (SCUT; the HedgeAgents authors)  |  **Venue:** arXiv preprint  |  **arXiv:** 2510.07920  |  **Year:** 2025

> **Digest provenance note:** written from a verified subagent full-read (2026-07-04) with headline numbers spot-checked verbatim against the source; Alpha Illusion (2605.16895 §2.2) independently cites the same decay figures. Treat as digest-grade, one notch below a first-hand full read.

## Core Claim
The "profits" of LLM-based financial agents are largely an information-leakage mirage: agent frameworks that shine inside their backbone's knowledge cutoff decay by half or more beyond it, because backtest performance rides on memorized historical data rather than learned market skill. The paper contributes a formalized leakage-measurement triad (PC/CI/IDS), a memorization benchmark (FinLake-Bench), a causal fine-tuning demonstration, and FactFin — a mitigation architecture whose core move is constraining the LLM to strategy-code generation validated under counterfactual perturbation, never direct prediction.

## Methodology
- **Exp 1 — cutoff-crossing decay:** published agent frameworks (FinMem, FinAgent, QuantAgent, FinCON, TradingAgents, ...) on NASDAQ-100, GPT-4o backbone (cutoff Oct 2023): backtest window Q2–Q3 2021 (in-cutoff) vs generalization window Q3–Q4 2024 (post-cutoff), market conditions matched (+13.79% vs +13.35% index return).
- **Exp 2 — counterfactual perturbation** of price/news/indicator inputs (10 large-caps, 30 event points/stock): three formalized metrics — Prediction Consistency (PC: share of predictions unchanged under meaningfully altered inputs), Confidence Invariance (CI), Input Dependency Score (IDS, KL-based).
- **Exp 3 — FinLake-Bench:** 2,000 historical QA pairs (Jan 2022–Jun 2023) in 4 categories (price inquiry, trend prediction, event impact, market performance) probing memorization across GPT-4o / Claude-Sonnet-3.7 / Grok-3.
- **Exp 4 — causal injection:** LoRA fine-tune Qwen2.5-7B / Llama-3.1-8B on historical financial data (FNSPID, DJIA 30) → measure in-distribution accuracy vs out-of-sample generalization change.
- **FactFin mitigation:** Strategy Code Generator + RAG + Monte-Carlo Tree Search + Counterfactual Simulator, optimized against PC/CI/IDS; evaluated on 6 assets (2020–2025) vs 9 baselines × 6 backbones.

## Key Results
- **Cutoff-crossing decay:** Sharpe decays 51.48% (QuantAgent) to 62.23% (FinCON); Total Return decays 50.18% (TradingAgents) to 71.85% (FinMem).
- **Counterfactual insensitivity:** PC 0.69–0.82 — up to 82% of predictions unchanged despite meaningfully altered inputs (FinMem worst: PC 0.8213 / CI 0.8743 / IDS 0.2766) — decisions driven by priors/memory, not the data shown.
- **Memorization:** 85.37% (price) to 92.94% (event impact) accuracy on historical QA — far above chance for all three frontier backbones.
- **Causal demonstration:** injecting historical data lifts in-distribution accuracy 51.61%→72.16% (Qwen) / 54.73%→76.52% (Llama) while out-of-sample generalization DROPS 21.53% / 18.06% — the leakage-accuracy tradeoff is inducible, not correlational.
- **FactFin:** +31.91% TR / +22.74% SR over best baselines with lowest PC/CI and highest IDS; ablation: the Counterfactual Simulator is the single largest de-leaking component. Nuance: once the LLM is constrained to code/strategy generation, LARGER closed-source models show BETTER leakage control — the "bigger leaks more" scaling holds for direct prediction, and is mediated by how the LLM is used.

## V4 Relevance
- **PC/CI/IDS is an implementable leakage GATE, not just a principle:** perturb the evidence a seed's thesis cites (counterfactual rows/news) and require the generator's output to MOVE — a Refinery seed whose thesis/entry-logic survives contradictory evidence unchanged is prior-driven, not evidence-driven. Natural fit as an eval-task class for the #70 runner and as a CVRF-adjacent probe; complements Alpha Illusion's P3 (same monotonicity idea, here with a formalized metric triad).
- **FactFin's ablation is direct evidence FOR the Foundry's core bet:** constraining the LLM to structured-strategy generation validated under perturbation (instead of direct prediction) is the single most effective leakage mitigation they measure — "LLM reasons, algorithms execute" wins on anti-leakage grounds, not just reliability grounds.
- **The causal fine-tuning result is a hard warning for our LoRA plans:** fine-tuning generation models on historical financial data raises in-window performance while DEGRADING generalization — any Foundry fine-tune must be gated on post-cutoff/leakage-controlled eval deltas, never in-window gains (E-stage statistics discipline; contamination probes §4.3).
- **FinLake-Bench-style QA probes** are a cheap standing audit: before trusting any new backbone/lane model for generation, measure its memorization of the backtest window (complements BlindTrade's anonymization probe — one measures what the model knows, the other removes the trigger).
- Supersedes-nuance vs Look-Ahead-Bench (notes §2.4): qualitative thesis is the same (subsumed there), but the measurement protocol, the causal demonstration, and the usage-mediated scaling nuance are new — cite Profit Mirage for the PROTOCOL, Look-Ahead-Bench for the scaling claim.

## Limitations & Caveats
- Same-team as HedgeAgents (self-critique credibility, but also incentive to sell FactFin); FactFin's headline gains inherit the usual short-window/6-asset caveats — treat its ablation ordering as the finding, not its TR numbers.
- Backtest windows are short and US-large-cap/NASDAQ-heavy; PC/CI/IDS thresholds for "acceptable" sensitivity are not established — calibration needed per system.
- Not yet peer-reviewed (preprint, Oct 2025); FinLake-Bench release status should be checked before building on it.

## Key References
- Li et al. 2025 — Look-Ahead-Bench (arXiv 2601.13770; the leakage-scaling companion claim)
- Ye et al. 2026 — The Alpha Illusion (arXiv 2605.16895; uses this paper as its contamination anchor)
- Merchant & Levy 2025 — Divergence Decoding (arXiv 2512.06607; parameter-level future-memory stripping)
- Yu et al. 2023/2024 — FinMem / FinCON (the frameworks whose decay is measured)
- Sarkar & Vafa 2024 — temporal leakage / time-machine effects in LLM evaluation
