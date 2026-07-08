# Can Blindfolded LLMs Still Trade? An Anonymization-First Framework for Portfolio Optimization (BlindTrade)
**Authors:** Joohyoung Jeon (Korea University / Mirae Asset Securities), Hongchul Lee (Korea University)  |  **Venue:** arXiv preprint (ICLR 2026 FinAI workshop-class; ICLR ethics statement)  |  **arXiv:** 2603.17692  |  **Year:** 2026

## Core Claim
To trust an LLM trading signal you must first prove it isn't memorized ticker association. BlindTrade "blindfolds" the agents — anonymizing all tickers/company/product names — and shows that meaningful cross-sectional signal persists: four anonymized LLM agents → reasoning-embedding GNN → cost-aware PPO achieves Sharpe 1.40±0.22 (2025 YTD OOS, 20 seeds), with signal legitimacy checked by IC screening and negative-control shuffling rather than asserted.

## Methodology
- **Anonymization protocol:** point-in-time S&P 500 constituents daily (EODHD API, 2020-01→2025-08, 1,403 days — delisted/added members correctly handled = survivorship addressed); tickers → synthetic IDs ("AAPL"→"STOCK_0026"); proper nouns in news ("Apple", "iPhone", "Tim Cook") replaced via Google Knowledge Graph API. Explicitly claimed as a minimum safeguard, not leak-proof.
- **4 specialized agents** (Gemini 2.5 Flash), strict t−60..t−1 information set, deterministic JSON out + MANDATORY free-text reasoning: Momentum, News-Event, Mean-Reversion, Risk-Regime. Reasoning text → 384-d SBERT embedding, part of a 394-d per-stock feature vector.
- **IC validation as a deployment GATE:** Spearman rank IC vs 21-day-forward returns, LLM features vs RAW features; only features informative on holdout are retained ("if the IC fails validation, we do not use that signal").
- **SemGAT:** 2-layer GATv2; sector edges + semantic edges (reasoning-embedding cosine >0.75, top-10 neighbors); HL-Gauss distributional head + ranking loss.
- **PPO-DSR policy:** Differential Sharpe reward minus 10bps/turnover costs; interpretable intent head (defensive/neutral/aggressive); top-20 masking; Optuna-tuned on a held-out validation window, then frozen for OOS; 20 seeds.
- **Negative control:** cross-sectionally shuffle GNN scores with universe/prices/costs unchanged — a real signal must die under shuffling.

## Key Results
- 2025 YTD OOS (145 days): Sharpe 1.40±0.22, CumRet 32.2%±5.2% vs SPY 0.64/8.5%; beats SPY in 20/20 seeds. But MDD −31.7% vs SPY −19.0% (always 100% invested, no cash action).
- **IC honesty:** absolute LLM ICs are tiny (avg +0.005; Risk-Regime +0.011, p<1e-4; 0.0515 on the 2025 holdout); for Momentum/Mean-Reversion the LLM's contribution is REMOVING misleading RAW correlation (ΔIC toward zero), not adding signal — the paper distinguishes these two mechanisms explicitly.
- Negative control: shuffling collapses |RankIC| 0.015→0.0004 (≈random) — performance derives from cross-sectional signal structure, not artifacts.
- Ablations: graph structure is the biggest contributor (ΔSharpe −0.78 without GNN), LLM features add −0.26; naive top-K without the RL cost-control layer → 139%/day turnover → Sharpe −1.17 (cost-aware execution, not signal, makes it tradable).
- Simpler-is-better: fancier GNN losses (vol-scaled targets, confidence weighting) INCREASE seed variance and DROP win rate (20/20 → 12/20, 10/20).
- **Regime dependency (extended OOS 2024–25, 397 days):** underperforms in the 2024 trending bull (Sharpe 0.34 vs SPY 1.70), outperforms in volatile 2025 (1.02 vs 0.54).

## V4 Relevance
- **The anonymization probe is directly adaptable as generator validation:** run Refinery seed generation with anonymized symbols/universe context and measure how much seed quality (validator pass rates, downstream IC of derived features) survives — complements the blueprint's contamination-controlled rediscovery probe (§4.3) and Look-Ahead-Bench-style leakage measurement. Cheap to add as an eval-set variant; blocks the "it's Apple, so momentum" shortcut our regime/indicator abstraction discipline already targets.
- **Negative-control shuffling is a one-day, always-available honesty check** for ANY ranked-signal claim in the pipeline (interim scores, feature effectiveness): shuffle the scores, keep everything else; if performance doesn't collapse, the claimed signal wasn't doing the work. Worth adopting as a standing eval-discipline tool (E-stage).
- **Validate-before-deploy IC gating mirrors our label_basis/quarantine doctrine:** features that fail holdout IC are simply not used — same fail-closed spirit as archive admission gates.
- **Cautionary findings that confirm decided bets:** cost-aware execution dominates raw signal (supports deterministic execution layer + turnover accounting in the ranker's cost model); added model complexity hurt stability (supports right-sizing/simplicity discipline); tiny absolute ICs (0.005–0.05) from LLM features land exactly in the FINSABER-calibrated realistic range — a useful expectations anchor for Refinery seed-derived features.

## Limitations & Caveats
- **The anonymization effect itself is NOT ablated** (authors' own limitation #1): no anonymized-vs-raw-ticker comparison was run, so the paper demonstrates the protocol and validates signal structure — it does not measure how much memorization the blindfold actually removed. The probe design is the takeaway, not an effect size.
- Single 145-day headline OOS window; extended eval reveals strong regime dependency (bull-market underperformance); training period short (2020–2023); static policy, no walk-forward retraining (named as future work).
- High volatility/MDD by construction (no cash allocation); 10bps flat cost model (no impact/latency); Gemini-2.5-Flash-only (no cross-backbone check); semantic-vs-sector edge ablation deferred.
- Two-author preprint; results hinge on one market (US large-cap) and the EODHD constituent feed.

## Key References
- Lee et al. 2025 — "Your AI, not your view" (arXiv 2507.20957; the LLM sector/size-bias evidence motivating the blindfold)
- Bailey, Borwein, López de Prado, Zhu 2014 — Pseudo-mathematics and financial charlatanism (backtest overfitting)
- Elton, Gruber, Blake 1996 — Survivorship bias and mutual fund performance
- Yu et al. 2025 — LiveTradeBench (arXiv 2511.03628; static-benchmark winners fail in real-time trading)
- Fan et al. 2025 — AI-Trader (arXiv 2512.10971; agents without risk management fail in practice)
- Bellemare et al. 2017 — Distributional RL (HL-Gauss head)
- Xiao et al. 2024 — TradingAgents (arXiv 2412.20138)
- Fu 2025 — The New Quant survey (arXiv 2510.05533)
