# AlphaAgent: LLM-Driven Alpha Mining with Regularized Exploration to Counteract Alpha Decay
**Authors:** Ziyi Tang, Zechuan Chen, Jiarui Yang, Jiayao Mai, Yongsen Zheng, Keze Wang, Jinrui Chen, Liang Lin  |  **Venue:** KDD 2025  |  **arXiv:** 2502.16789  |  **Year:** 2025

## Core Claim
LLM-driven alpha mining suffers from homogeneous factor generation that accelerates crowding-induced alpha decay. AlphaAgent counters this with three regularization mechanisms — originality enforcement via AST similarity, hypothesis-factor alignment via LLM semantic evaluation, and complexity control via AST structural constraints — enabling decay-resistant factor discovery that maintains stable IC over 4 years while traditional factors decay to near zero.

## Methodology
- Three specialized LLM agents: **Idea Agent** (proposes market hypotheses from financial theories and trends), **Factor Agent** (constructs alpha factors from hypotheses with regularization), **Eval Agent** (backtests, validates, and provides iterative feedback to Idea and Factor agents).
- **Originality enforcement**: Each candidate factor is parsed into an Abstract Syntax Tree (AST). Pairwise subtree isomorphism detects the largest common subtree between the candidate and every factor in the existing alpha zoo (e.g., Alpha101). If the maximum common subtree exceeds a similarity threshold → rejected as a near-clone.
- **Hypothesis-factor alignment**: The LLM evaluates semantic consistency between the Idea Agent's market hypothesis, the Factor Agent's natural-language description, and the generated mathematical expression. Misaligned factors (where the expression doesn't implement the stated hypothesis) are rejected.
- **Complexity control**: AST depth and node count are constrained to prevent over-engineered expressions that overfit. Factors exceeding structural complexity limits are simplified or rejected.
- Evaluated on CSI 500 (Chinese market) and S&P 500 (US market) from January 2021 to January 2025, with transaction costs applied.

## Key Results
- **Alpha decay resistance**: While traditional factors (RSI, GP-generated, Alpha158) saw IC plummet to near zero over 4 years, AlphaAgent's factors maintained stable IC between 0.02–0.025.
- **Returns**: 11.00% annualized excess return (IR=1.49) on CSI 500; 8.74% annualized (IR=1.05) on S&P 500 — significantly ahead of second-best (LSTM 4.96%, DeepSeek-R1 2.75%).
- **Risk control**: MDD below 10% in both markets (-9.36% CSI, -9.10% S&P).
- **Efficiency**: 81% higher effective factor ratio (hit ratio) while consuming 30% fewer tokens than baseline LLM approaches.
- **Ablation**: Removing any of the three regularizations degrades IC by 15-30%; removing all three collapses performance to baseline LLM levels.

## V4 Relevance
- **Direct architectural analog**: AlphaAgent's Idea Agent → Factor Agent → Eval Agent maps to V4's Thesis Library → Strategist → Forge pipeline. The key addition is the regularization layer between seed generation and Forge evaluation.
- **Originality enforcement is critical for V4**: Without it, the Strategist will converge on the same popular feature combinations (momentum + RSI + volume) that every other quant runs, accelerating crowding. V4 should track all promoted model features in a "factor zoo" and penalize seeds that reuse the same feature combinations.
- **SHAP feedback closes the loop**: AlphaAgent's Eval Agent provides iterative feedback on which factors actually predict. V4 should use XGBoost SHAP values as the Eval signal — feeding per-feature importance back to the Strategist so it learns which feature hypotheses are actually captured by the trained model.
- **Decay monitoring**: V4 should track per-feature IC over rolling windows and detect hyperbolic decay patterns (complementary: "Not All Factors Crowd Equally," arXiv 2512.11913, derives α(t) = K/(1+λ·t) for mechanical factors). When a champion model's features show decay, trigger re-seeding with explicit novelty bias.

## Limitations & Caveats
- Evaluated on daily frequency only; no evidence for intraday or weekly horizons.
- AST similarity requires a curated operator library; factors using novel operators outside the library cannot be parsed.
- Complexity control thresholds (AST depth, node count) are hyperparameters that may need market-specific tuning.
- Results are backtested; no live deployment validation. The 4-year test period includes relatively benign markets for quant strategies.

## Key References
- WorldQuant (2015) — Alpha101: formulaic alpha factor library (the "alpha zoo" baseline)
- Kakushadze, Z. (2016) — "101 Formulaic Alphas" (basis for AST similarity comparison)
- McLean, R.D. & Pontiff, J. (2016) — "Does Academic Research Destroy Stock Return Predictability?" (alpha decay from publication)
- Microsoft Research — RD-Agent framework (implementation foundation)
- Lee, C. (2025) — "Not All Factors Crowd Equally" (hyperbolic decay model, arXiv 2512.11913)
- Xu et al. (2024) — RD-Agent: LLM-based autonomous agent for data-driven research and development
