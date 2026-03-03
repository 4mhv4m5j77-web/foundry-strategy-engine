# TradeTrap: Are LLM-based Trading Agents Truly Reliable and Faithful?
**Authors:** Lewen Yan, Jilin Mei, Tianyi Zhou, Lige Huang, Jie Zhang, Dongrui Liu, Jing Shao  |  **Venue:** arXiv preprint  |  **arXiv:** 2512.02261  |  **Year:** 2025

## Core Claim
Current LLM-based autonomous trading agents are systematically vulnerable to adversarial perturbations at every stage of their decision pipeline. Small, localized attacks on any single component (data, prompts, memory, or execution interface) propagate through the full loop, inducing extreme concentration, runaway exposure, and large drawdowns without triggering conventional safeguards.

## Methodology
- Built **TradeTrap**, a unified evaluation framework that stress-tests both **Adaptive** (tool-calling, e.g. AI-Trader) and **Procedural** (pipeline, e.g. ValueCell/NoFX) agent architectures under controlled single-variable attacks.
- Defined four threat surfaces with six attack modules: data fabrication & MCP tool hijacking (market intelligence), prompt injection (strategy formulation), memory poisoning & state tampering (portfolio/ledger handling).
- Backtested on ~100 NASDAQ-100 stocks over October, $5,000 initial capital, zero fees, measuring 9 metrics (total return, MDD, Sharpe, Calmar, volatility, position concentration, etc.).
- Attack isolation protocol: only one attack module active per experiment; all other components identical to a clean baseline, ensuring causal attribution.

## Key Results
- **Prompt injection** is devastating to Adaptive agents: total return drops from 7.81% to 0.89%, Sharpe from 5.72 to 0.29, while max position concentration hits 99.98%.
- **State tampering** causes catastrophic failure in Procedural agents: -61.02% total return, 91.97% MDD, 889.61% volatility, 100% max concentration (from a 0.91% / 1.59% MDD baseline).
- **MCP tool hijacking** induces "strategic paralysis" -- the Adaptive agent hallucinates phantom positions after a manufactured volatility trap and stops trading entirely.
- **Memory poisoning** silently erodes performance over time: Adaptive return drops from 7.81% to 1.88%, Sharpe from 5.72 to 1.58, with losses accumulating gradually rather than appearing as abrupt failures.
- Adaptive agents are more vulnerable to information-channel attacks (data fabrication, MCP hijacking); Procedural agents are more vulnerable to internal-state corruption (memory poisoning, state tampering).

## V4 Relevance
- **Data pipeline integrity is critical**: If V4 uses LLM agents to ingest news/sentiment for alpha generation, fabricated or hijacked data sources can propagate into concentrated, narrative-driven bets. Cross-validation of data sources and cryptographic verification of tool/API provenance are essential defenses.
- **State and memory isolation**: Any architecture that persists portfolio state or reasoning traces across sessions is vulnerable to poisoning that compounds silently. V4 should implement independent ground-truth reconciliation between the agent's believed state and actual exchange state at every decision step.
- **Prompt hardening and architecture choice**: Prompt injection alone can collapse Adaptive agent performance by 10x on risk-adjusted metrics. If V4 uses LLM-generated strategies fed to ML execution, the LLM layer needs input guardrails, red-teaming, and zero-trust boundaries between the strategy-generation and execution layers.

## Limitations & Caveats
- Evaluation limited to a single one-month window (October) on NASDAQ-100 with $5,000 capital and no transaction costs or slippage -- unclear how results scale to realistic AUM, multi-asset, or live-trading conditions.
- Only two agent architectures tested (AI-Trader, ValueCell); results may not generalize to more sophisticated multi-agent or RL-augmented systems.
- Attacks are applied in isolation; compound/multi-vector attacks and adaptive adversaries that respond to agent behavior are not studied.

## Key References
- [5] Chen et al., "AgentPoison: Red-teaming LLM agents via poisoning memory or knowledge bases," NeurIPS 2024
- [6] Cheng et al., "Uncovering the vulnerability of LLMs in the financial domain via risk concealment," arXiv 2509.10546, 2025
- [7] Fu et al., "Imprompter: Tricking LLM agents into improper tool use," arXiv 2410.14923, 2024
- [10] HKUDS, "AI-Trader: Autonomous Trading Agent Framework," 2025
- [11] Hu et al., "FinTrust: A comprehensive benchmark of trustworthiness evaluation in finance domain," EMNLP 2025
- [14] Liu et al., "Prompt injection attack against LLM-integrated applications," arXiv 2306.05499, 2023
- [21] ValueCell-ai, "ValueCell: Intelligent Trading Agent Suite," 2025
- [23] Wei et al., "Jailbroken: How does LLM safety training fail?" NeurIPS 2023
- [30] Zou et al., "PoisonedRAG: Knowledge corruption attacks to RAG," USENIX Security 2025
