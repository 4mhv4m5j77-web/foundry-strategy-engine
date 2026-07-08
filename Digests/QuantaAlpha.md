# QuantaAlpha: An Evolutionary Framework for LLM-Driven Alpha Mining
**Authors:** Jun Han, Shuo Zhang, Wei Li, Zhi Yang, et al. (SUFE AIFin Lab, QuantaAlpha, Stanford, PKU, SYSU, SEU)  |  **Venue:** arXiv preprint (q-fin.ST)  |  **arXiv:** 2602.07085v2  |  **Year:** 2026

## Core Claim
Treating each end-to-end alpha-mining RUN as a trajectory and evolving trajectories — targeted mutation of the failure-causing step (prefix frozen, localized rewrite) plus crossover of high-reward segments from parent trajectories — beats both unconstrained regeerative agentic mining (RD-Agent) and generation-regularized mining (AlphaAgent), while symbolic-AST factor construction with semantic-consistency verification and complexity/redundancy gates prevents drift and crowding.

## Methodology
- **Roles:** initialization agent (diversified planning — complementary hypotheses across signal sources/time scales/mechanism types), idea agent (hypotheses from market context + priors + parametric specs), factor agent (hypothesis → structured semantic description → symbolic expression over a standardized operator library → AST → compiled code, with LLM repair on compile failure), LLM verifier (consistency across hypothesis/description/expression/code), evaluation agent (Qlib backtest + evaluation history of success/failure patterns).
- **Trajectory evolution:** mining run = ordered (state, action) sequence with terminal reward R(τ) = predictive score − λ·regularizer. Mutation: self-reflection localizes the worst decision node k; rewrite only that action, freeze the prefix, regenerate the suffix. Crossover: recombine validated segments (hypothesis templates, construction patterns, repair behaviors) from k high-reward parents. Demonstrations bias subsequent generation (imitation prior).
- **Generation gates (AlphaAgent lineage):** complexity C(f) = weighted symbolic length + parameter count + log feature count; redundancy = largest-common-isomorphic-subtree AST similarity vs an alpha zoo, reject-and-rewrite over limits.
- **Eval:** CSI 300; train 2016–2020, valid 2021, **test 2022-01→2025-12 (4y OOS)**; Qlib TopkDropout portfolio (rank by score, drop n lowest, equal weight) with buy/sell transaction fees; metrics on excess returns AFTER costs; features = OHLCV+VWAP only, next-day return target, CSRankNorm. 5 backbone LLMs × 3 agentic frameworks compared + ML/DL/factor-library baselines.
- Factor pool maintenance (case study): greedy RankIC-descending admission, |corr| < 0.7 vs pool, pool capped at 50% of mined factors.

## Key Results
- **Best-in-table on CSI 300:** with GPT-5.2, IC 0.1501 / RankIC 0.1465 / ARR 27.75% / MDD 7.98% vs AlphaAgent-best IC 0.1092/ARR 16.48% and RD-Agent-best IC 0.0531/ARR 9.91%. Gains consistent across all 5 backbones (not model-specific).
- **Ablations (the transferable finding):** removing MUTATION is the largest predictive-power loss (IC −0.0292, ARR −9.81%); removing diversified INITIALIZATION barely moves IC but costs ARR −7.78% and MDD +2.73% (diversity pays at strategy level, not factor level); removing CROSSOVER is smaller but consistent (ARR −2.82%). Generation gates: complexity control removal is the worst at strategy level (excess return −8.44%).
- **OOD transfer:** factors mined on CSI 300 deployed zero-shot on CSI 500 and S&P 500 → ~160% and ~137% cumulative excess return over 4y; baselines stagnate at the Dec-2023 regime shift, QuantaAlpha holds through it.
- **2023 regime-shift diagnosis:** its factor population survived the A-share large-cap→small-cap rotation because semantic DIVERSITY across information channels (overnight/auction gaps, volatility structure, liquidity-conditioned trend quality) kept a predictive subset alive — not because any factor was regime-proof.
- **Iteration convergence:** factor-pool quality peaks around iterations 11–12 (~350 factors); further iterations ADD redundancy and degrade drawdown — evolutionary mining has a measurable over-search point.

## V4 Relevance
- **Trajectory-level operators are the genuinely new idea for the Refinery idea tier (S5):** our receipts already make every mining run a first-class, auditable trajectory (seed logs, repair rounds, escalations, lineage stamps). QuantaAlpha shows mutation should target the FAILING STEP of a run (hypothesis vs expression vs code), not just the output seed — i.e. the S5 mutation operator can consume the receipt trail, not just the archive entry. Crossover of validated segments maps to recombining (thesis template × indicator construction × repair pattern) across successful lineages.
- **Confirms three decided bets with fresh ablation evidence:** evolutionary archive loop over one-shot generation (D2), acceptance regularizers between generation and evaluation (§4.5 — complexity control is the single most valuable gate at strategy level), diversity-first initialization (heterogeneous conditioning §4.4.4 — pays at portfolio level even when per-factor IC is flat).
- **The over-search finding is an N-deflation cousin:** past ~iteration 12 more search adds redundancy and hurts — empirical support for the trial-count ledger + DPP-selection discipline, and for cadence-pegged (not maximal) generation in the operating harness.
- **Consistency verification chain (hypothesis↔description↔expression↔code) is our thesis↔logic alignment constraint** extended one level down; the reject-and-rewrite loop mirrors S1 repair-with-feedback.

## Limitations & Caveats
- **No survivorship-bias treatment** (index constituents via Qlib; delisted handling unstated), **no deflated Sharpe / multiple-testing correction, no walk-forward** (single chronological split — though the 4y OOS window and zero-shot cross-market transfer are meaningfully harder tests than the 1y windows FINSABER indicts).
- Headline numbers (IC 0.15, ARR 27.75% after costs) are far above the FINSABER-calibrated realistic range (IC 0.02–0.05) — treat mechanisms as validated, magnitudes as optimistic; industrial co-authorship (QuantaAlpha company) is a conflict to weigh.
- LLM-based verifier is a soft gate (semantic consistency judged by an LLM — the class of check our blueprint allows only as flag-for-review, never authority); daily-frequency China-centric; six OHLCV-derived base features only (no fundamentals/news).

## Key References
- Novikov et al. 2025 — AlphaEvolve (arXiv 2506.13131; the evolutionary-resampling ancestor)
- Lin et al. 2025 — SE-Agent: self-evolution trajectory optimization (arXiv 2508.02085; the trajectory-operator source)
- Tang et al. 2025 — AlphaAgent (KDD; the generation-regularization baseline it extends)
- Li et al. 2025 — RD-Agent(Q) (arXiv 2505.15155; the full-stack agentic baseline)
- Shi et al. 2025 — AlphaForge (AAAI; dynamic factor combination) + MCTS factor mining (arXiv 2505.11122)
- Ding et al. 2025 — AlphaEval (arXiv 2508.13174; evaluation protocol)
- Hu et al. 2026 — Controlled self-evolution for code optimization (arXiv 2601.07348)
- Duan et al. 2025 — FactorMAD: multi-agent debate factor mining (ICAIF)
