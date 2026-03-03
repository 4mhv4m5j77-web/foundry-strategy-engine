# The New Quant: A Survey of Large Language Models in Financial Prediction and Trading
**Authors:** Weilong Fu (Columbia University)  |  **Venue:** arXiv preprint  |  **arXiv:** 2510.05533  |  **Year:** 2025

## Core Claim
LLMs are shifting quantitative investing from feature-centric text mining to end-to-end decision systems where models read heterogeneous disclosures, generate auditable hypotheses, and translate textual understanding into risk-controlled positions. This survey synthesizes 50+ studies (2023-2025) into a task-centered taxonomy spanning sentiment, information extraction, numerical QA, summarization, multimodal analysis, agentic workflows, and governance.

## Methodology
- Proposes a seven-task taxonomy mapping LLM capabilities to trading relevance: sentiment/opinion, information extraction and knowledge graphs, numerical QA, summarization, multimodal cues, agentic workflows, and governance functions
- Organizes return prediction literature by evidence channel (news, filings, earnings calls, policy) and modeling pattern (zero-shot scoring, domain-tuned FinLLMs, RAG-grounded signals, LLM-guided structured models, time series prompting)
- Surveys trading system architectures along the trade lifecycle: research agents, prompt-to-strategy conversion, retrieval-verified analysis loops, signal-to-order execution, and portfolio construction
- Consolidates benchmarks (FinQA, FinanceBench, AlphaFin, R-Judge) and proposes evaluation desiderata requiring time-safe splits, economic metrics (Sharpe, turnover, capacity), and agent audit traces

## Key Results
- Zero/few-shot LLM sentiment on news and social media shows out-of-sample return predictability, but evaluation standards remain inconsistent across studies
- RAG and retrieval-aware chunking reduce hallucination and stabilize factor construction; layout-aware encoders improve numerical reasoning over filings and tables
- Agentic frameworks (TradingGPT, FinAgent, FinMem, QuantAgent, Alpha GPT) demonstrate that layered memory, role specialization, multi-agent debate, and self-improvement loops can produce executable strategies with human oversight
- Temporal leakage from web pretraining is a critical unsolved problem; even without explicit future documents, latent knowledge can leak into predictions at inference time
- Most studies report accuracy/correlation rather than trading-grade metrics (walk-forward Sharpe, drawdown, turnover, capacity, transaction costs)

## V4 Relevance
- Validates the separation of concerns architecture: LLM-based signal generation (research/alpha) must be kept distinct from portfolio optimization/execution, with retrieval-first prompting, tool-verified numerics, and confidence gating between layers
- Supports using RAG-grounded evidence loops (propose -> retrieve -> verify -> simulate) as the core pattern for LLM alpha factor generation, with audit logs binding language to timestamped evidence
- Highlights hybrid query routing (lightweight models for easy queries, high-capacity for hard cases) as essential for cost/latency management in production trading systems

## Limitations & Caveats
- No standardized time-safe trading benchmark exists; most benchmarks measure reasoning fidelity rather than simulated P&L under realistic market microstructure
- LLM signals can overfit to disclosure style, sector, or macro regime; cross-regime and multilingual generalization remains largely untested
- Hallucination and brittle numerical reasoning persist; the survey emphasizes that trading systems should never change risk based on unverifiable rationales

## Key References
- Lopez-Lira and Tang (2023) - Zero/few-shot GPT scores predict cross-sectional returns
- Steinert and Altmann (2023) - LLM microblog sentiment correlates with next-day stock moves
- Li, Yu, et al. (2023) - TradingGPT: layered memory and multi-agent debate for trading
- Wang, Yuan, et al. (2024) - Self-improving agent loop with critique and simulator retesting
- Yuan, Wang, and Guo (2024) - Alpha GPT 2.0: human-in-the-loop alpha mining with review gates
- Yepes, You, et al. (2024) - Retrieval-aware chunking for long document QA in filings
- Sarkar and Vafa (2024) - Temporal leakage and time machine effects in LLM evaluation
- Jiang, Kelly, and Xiu (2023) - Trend model baselines for measuring incremental value of language signals
- Hua et al. (2024) - Constitution-guided safety for agent tool use
- Zhang, Zhao, et al. (2024) - FinAgent: tool-augmented multimodal trading agent
