# Large Language Model Agent in Financial Trading: A Survey
**Authors:** Han Ding, Yinheng Li, Junhao Wang, Hang Chen, Doudou Guo, Yunbai Zhang  |  **Venue:** Preprint (Columbia University / NYU)  |  **arXiv:** 2408.06361  |  **Year:** 2024

## Core Claim
This is the first systematic survey of LLM-based financial trading agents, reviewing 27 papers and organizing them into a two-branch taxonomy -- "LLM as a Trader" (direct signal generation) vs. "LLM as an Alpha Miner" (alpha factor production) -- while cataloguing their architectures, data inputs, evaluation methods, and open challenges.

## Methodology
- **Taxonomy:** Divides agent architectures into (1) LLM as a Trader with four sub-types: news-driven, reflection-driven, debate-driven, and RL-driven; and (2) LLM as an Alpha Miner (e.g., QuantAgent, AlphaGPT) where LLMs generate alpha factors fed into downstream trading systems
- **Data categorization:** Identifies four data groups used across agents -- numerical (prices, volumes), textual (news, filings, analyst reports, social media), visual (charts), and simulated (synthetic market environments)
- **Evaluation framework:** Catalogues metrics (cumulative return, Sharpe ratio, max drawdown, signal accuracy, Information Coefficient) and backtesting practices across surveyed papers
- **Model census:** Maps LLM backbone usage across papers; GPT-3.5 (9 papers) and GPT-4 (8 papers) dominate, with a long tail of open-source models (Qwen, Baichuan, FinGPT, LLaMA)

## Key Results
- **LLM trader agents achieve 15-30% annualized return above the strongest baseline** during backtesting with real market data, though backtesting periods are short (median 1.3 years) and typically cover only a single regime
- **Reflection and memory mechanisms are critical:** Layered memory (FinMem) and reflection (FinAgent) consistently improve trading performance over stateless prompting by providing temporal context and reducing hallucination
- **Alpha mining is underexplored but high-potential:** Only two papers (QuantAgent, AlphaGPT) use the LLM-as-Alpha-Miner pattern, yet both demonstrate that LLMs can generate and iteratively refine alpha factors through inner-loop/outer-loop architectures
- **Major gaps identified:** Almost no studies consider trading costs, visual data is barely explored, social media data is underutilized, few studies fine-tune LLMs for trading, and integration with existing production trading systems is unaddressed
- **Ethical risk:** Simulated environments reveal LLMs can exploit insider information and craft deceptive explanations under pressure, raising regulatory concerns

## V4 Relevance
- **Validates the Alpha Miner architecture as V4's core pattern:** The survey's distinction between "LLM as Trader" vs. "LLM as Alpha Miner" directly maps to V4's design; the inner-loop/outer-loop pattern from QuantAgent (LLM generates factors, market feedback refines them) is the closest published precedent to V4's alpha generation pipeline
- **Memory and reflection are necessary, not optional:** Across the surveyed papers, stateless LLM agents consistently underperform those with layered memory and reflection; V4 should incorporate structured memory for strategy performance history and market regime context
- **Identified gap is V4's opportunity:** The survey explicitly notes that integration with downstream ML execution systems is absent from the literature -- this is precisely V4's differentiator (LLM generates alpha, ML models execute)

## Limitations & Caveats
- **Backtesting is narrow and likely overstated:** Median test period is 1.3 years, confined to US and Chinese stock markets, with no derivatives/bonds/commodities coverage; few studies account for transaction costs or slippage
- **Closed-source model dependency:** Most agents rely on GPT-3.5/GPT-4, raising concerns about reproducibility, data privacy, and inference latency for high-frequency applications
- **No live trading validation:** All reported results are backtested only; no paper in the survey demonstrates live/paper-trading performance, leaving real-world viability unproven

## Key References
- Yu et al., 2023 -- FinMem: Performance-enhanced LLM trading agent with layered memory and character design (arXiv:2311.13743)
- Zhang et al., 2024 -- FinAgent: Multimodal foundation model for financial trading (arXiv:2402.18485)
- Li et al., 2024 -- LLMFactor: Extracting profitable factors through prompts for explainable stock movement prediction (arXiv:2406.10811)
- Wang et al., 2024 -- QuantAgent: Seeking Holy Grail in trading by self-improving large language model (arXiv:2402.03755)
- Sun et al., 2024 -- AlphaGPT: Human-AI interactive alpha mining for quantitative investment (arXiv:2308.00016)
- Yang et al., 2023 -- TradingGPT: Multi-agent system with layered memory and distinct characters for enhanced financial trading (arXiv:2309.03736)
- Xing, 2024 -- Designing heterogeneous LLM agents for financial sentiment analysis (arXiv:2401.05079)
- Fatouros et al., 2024 -- Can large language models beat Wall Street? Unveiling the potential of AI in stock selection (arXiv:2401.03737)
- Koa et al., 2024 -- Learning to generate explainable stock predictions using self-reflective large language models (WWW '24)
- Chong et al., 2024 -- StockAgent: Large language model-based stock trading in simulated real-world environments
