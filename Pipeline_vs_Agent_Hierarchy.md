# Pipeline vs. Agent Hierarchy — Research Report

> **Date:** 2026-03-14
> **Status:** Reference Document
> **Purpose:** Inform architectural decision between pipeline (sequential) and agent hierarchy (multi-agent debate) topologies for The Foundry V4's Agent Research System.
> **Conclusion:** V4's current hybrid DAG (fixed-round pipeline with structured debate) is well-supported by the literature. No architectural changes recommended.

---

## 1. Overview

The Foundry V4's Agent Research System uses 9 LLM agents to generate strategy seeds — structured hypotheses that the Forge validates through ML training. This document evaluates two candidate topologies for organizing those agents:

1. **Pipeline** — agents execute sequentially, each receiving the previous agent's output
2. **Agent Hierarchy** — agents organized in a structured topology with parallel execution, adversarial debate, and orchestrated synthesis

V4 currently implements a **hybrid DAG** that captures the best of both. This document provides the evidence base for that decision.

---

## 2. Pipeline Architecture (Sequential Assembly Line)

### 2.1 How It Works

Agents execute in a fixed linear chain. Each agent receives the output of the previous agent and produces structured output for the next.

```
Idea → Fundamental → Technical → Sentiment → Macro → Bull → Bear → Risk → Strategist
```

### 2.2 Pros

| Advantage | Detail |
|-----------|--------|
| **Fully deterministic** | Execution order is fixed; every output traces to exactly one stage |
| **One GPU slot at a time** | Zero contention on inference hardware; no batching complexity |
| **Simple error handling** | If stage N fails, stop, log, retry, or abort — trivial to implement |
| **Easy monitoring** | Per-stage SLA budgets; clear timing metrics; Prometheus scraping per stage |
| **Clear data lineage** | Every piece of a seed traces back to exactly one stage |

### 2.3 Cons

| Disadvantage | Evidence | Source |
|-------------|----------|--------|
| **39% performance degradation** in multi-turn sequential LLM interactions | Agents lose context as information compresses through the chain | Du et al., ICML 2024 |
| **8.5–10.5% accuracy loss** from text-based information bottleneck between agents | Information lost at each handoff stage | Cache-to-Cache research |
| **No adversarial debate** | Bull can't respond to Bear's objections; emergent insights impossible | — |
| **Rigid topology** | Adding a new agent means deciding where in the sequence it goes | — |
| **Slower for parallel-eligible work** | Round 1 analysts that currently run in parallel (~5 min) would take ~20 min sequentially | — |
| **Zero papers** in the 19-paper evidence base use pure pipeline for multi-agent trading | No academic support for this topology in financial multi-agent systems | Architecture_Decision.md |

---

## 3. Agent Hierarchy (Orchestrated Debate)

### 3.1 How It Works

Agents organized in a multi-round structure with an orchestrator. Includes parallel execution, adversarial debate pairs, and hierarchical synthesis.

### 3.2 Pros

| Advantage | Evidence | Source |
|-----------|----------|--------|
| **Emergent cross-domain insights** | Sibyl's multi-agent debate scored 7x over single-agent on GAIA benchmark | Sibyl framework |
| **Adversarial debate breaks "Degeneration of Thought"** | Once an LLM commits to an answer, self-reflection can't fix it; a Bear agent can | Liang et al., EMNLP 2024 |
| **Parallel execution** | Round 1 analysts run concurrently, cutting latency from ~20 min to ~5 min | — |
| **Flexible topology** | Adding an agent = adding a node, no reordering required | LangGraph hierarchical teams |
| **Attribution via SHAP** | Features trace back to the agent that hypothesized them, closing the feedback loop | AlphaAgent |
| **Andrew Ng validation** | "A team of GPT-3.5 agents can collectively outperform a single GPT-4" — validates Minsky's Society of Mind thesis | Andrew Ng (public remarks) |

### 3.3 Cons

| Disadvantage | Evidence | Source |
|-------------|----------|--------|
| **Harder to debug** | 9 agents across 4 rounds = forensic exercise to trace a bad seed | Smit et al., ICML 2024 |
| **~2.46x compute cost** vs. single-agent baseline | 1.83x for self-reflection + 0.63x for debate overhead | Liang et al., EMNLP 2024 |
| **Groupthink risk from homogeneous backbone** | All agents on the same local model share identical biases and training data artifacts | EMNLP 2025 Findings |
| **Non-deterministic** | Same inputs can produce different seeds on different runs | Smit et al., ICML 2024 |
| **Orchestration complexity** | CrewAI's hierarchical process documented as not always functioning as designed | CrewAI docs, Towards Data Science |
| **"Single strong agent" counterargument** | A single agent with strong prompting can match multi-agent debate in some settings | Wang et al., ACL 2024 |

---

## 4. Academic Evidence Base

### 4.1 Multi-Agent Debate Papers

| Paper | Venue | Key Finding |
|-------|-------|-------------|
| Du et al., "Improving Factuality and Reasoning through Multiagent Debate" | ICML 2024 | 3 agents debating 2 rounds significantly improves math reasoning and reduces hallucination. Performance scales with both agent count and round count. |
| Liang et al., "Encouraging Divergent Thinking through Multi-Agent Debate" | EMNLP 2024 | Identifies DoT problem. MAD with judge + adaptive break mechanism outperforms self-reflection. Same-backbone agents can outperform heterogeneous agents if debate structure is right. |
| Smit et al., "Should we be going MAD?" | ICML 2024 | MAD does not reliably outperform simpler strategies (self-consistency, ensembling). MAD is more sensitive to hyperparameters. Modulating agreement level is key. |
| Wang et al., "Rethinking the Bounds of LLM Reasoning" | ACL 2024 | Single agent with strong prompts can match best multi-agent performance when demonstrations are available. Multi-agent adds value primarily in zero-shot settings. |

### 4.2 Finance-Specific Multi-Agent Papers

| Paper | Venue | Architecture | Relevance to Foundry |
|-------|-------|-------------|---------------------|
| FinCon (Yu et al.) | NeurIPS 2024 | Manager-analyst hierarchy with CVRF | Direct inspiration for V4's debate structure and belief updates |
| TradingAgents (Xiao et al.) | 2024 | Bull/Bear researchers + analysts + risk team | Direct inspiration for V4's 9-agent roles |
| FinMem | 2023 | Layered memory (short/medium/long-term) | V4's three-tier memory (Redis/TimescaleDB/pgvector) |
| TradingGPT | 2023 | FinMem + inter-agent debate on reflections | Validates debate-on-reflections improves robustness |
| HedgeAgents | 2025 | Specialized risk agents + extreme market conferences | V4's Risk Manager role + tail-risk handling |

### 4.3 Framework Patterns

| Framework | Pattern | Relevance |
|-----------|---------|-----------|
| LangGraph Supervisor | Central supervisor routes to specialized workers. Supports multi-level hierarchies. | V4's Strategist acts as top-level supervisor. |
| AutoGen Group Chat | All agents share a conversation thread. Speaker selection via LLM or FSM. | V4 uses more structured pipeline approach — more deterministic. |
| CrewAI Hierarchical | Manager delegates to workers, validates outputs. Known issues with sequential execution instead of true delegation. | Cautionary tale: over-relying on LLM-driven orchestration leads to unpredictable behavior. V4's fixed round structure avoids this. |

### 4.4 Society of Mind

Minsky's 1986 theory — that intelligence emerges from managed interaction of simpler, non-intelligent agents — is experiencing a renaissance. The Society of HiveMind (SOHM) framework represents agent collectives as optimizable graphs. The key insight: the value is not in any individual agent's intelligence but in the *structure* of their interaction.

---

## 5. Key Warnings from the Literature

### 5.1 The FINSABER Warning

Even well-structured multi-agent debate produces impressive-looking theses. V4's architecture correctly routes these through the Forge's rigorous validation rather than trusting them directly. This is the single most important architectural decision.

### 5.2 Long-Horizon Performance Degrades

"Can LLM-based Financial Investing Strategies Outperform the Market in Long Run?" (2025) found that extending evaluation horizons significantly diminishes LLM advantages. Over 20 years, Buy-and-Hold consistently ranks among top performers. This reinforces V4's design: agents generate hypotheses, ML models validate them statistically.

### 5.3 Crowding Risk from Homogeneous Agents

If all agents share the same model backbone and similar training data, they may systematically overweight or underweight certain factors. TradeTrap's adversarial attack taxonomy identifies "uniform decision-making due to similar prompts" as a source of crowding and instability under market stress. Using a different model for the Strategist provides some heterogeneity, but agents sharing the same backbone have correlated biases.

**Mitigation:** Benchmark multiple models for agent roles (see `Documentation/Model_Registry.md` and `Documentation/Benchmark_Plan.md`). Consider assigning different models to different agent roles once benchmarking identifies the best model for each task.

### 5.4 The "Single Strong Agent" Counterargument

Wang et al. (ACL 2024) found that a single agent with strong demonstrations can match multi-agent debate performance. This does not invalidate the hierarchy for The Foundry (where the task is open-ended hypothesis generation, not benchmark QA), but it means the hierarchy must earn its complexity through measurable improvement in seed quality — which the CVRF validation plan (30 cycles with vs. 30 without, measuring Forge pass rates) is designed to test.

---

## 6. V4's Current Design: Hybrid DAG

V4 implements neither a pure pipeline nor a pure hierarchy. It is a **fixed-round DAG with structured debate**:

```
Round 1: Fan-out (parallel)     → 4 analysts independently query data
Round 2: Adversarial debate     → Bull/Bear receive ALL analyst reports
Round 3: Fan-in (risk)          → Risk Manager reviews everything
Round 4: Synthesis              → Strategist produces seeds
Round 5: CVRF (weekly)          → Belief updates from trade results
```

This captures the best of both topologies:

| Property | Pipeline | Hierarchy | **V4 Hybrid** |
|----------|----------|-----------|---------------|
| Deterministic orchestration | Yes | No | **Yes** (fixed rounds) |
| Parallel execution | No | Yes | **Yes** (Round 1) |
| Adversarial debate | No | Yes | **Yes** (Round 2) |
| Fan-in synthesis | No | Yes | **Yes** (Round 4) |
| Easy to debug | Yes | No | **Moderate** (fixed topology helps) |
| Flexible topology | No | Yes | **Moderate** (add agents within rounds) |

---

## 7. Recommendations

Based on the literature:

1. **The fixed 4-round pipeline is the right call.** It avoids CrewAI's orchestration pitfalls while capturing FinCon's efficiency gains from structured hierarchy. Do not make it dynamic unless there is evidence of improvement.

2. **Log everything per debate session.** Each agent's full input and output, with `debate_session_id` linking to the resulting seeds. This is the only way to debug and attribute.

3. **Consider modulating agreement level** (per Smit et al.). The Bull/Bear prompts should explicitly control how adversarial the debate is — too much agreement causes groupthink, too much disagreement causes noise.

4. **Model homogeneity risk is real.** Monitor for systematic blind spots. Using a different model for the Strategist helps. Consider assigning different models to different agent roles based on benchmark results (see `Documentation/Model_Registry.md`). Rotating one analyst agent to a different backbone periodically could serve as a canary.

5. **CVRF validation is essential.** The planned 30-with vs. 30-without comparison will determine whether the debate structure actually improves Forge pass rates. If it does not show >10% improvement, the architecture document already has a kill switch to simplify.

6. **Attribution via SHAP.** Since XGBoost models are trained on features hypothesized by specific agents, SHAP values on those features provide a natural attribution path back to the originating agent. This closes the feedback loop.

---

## 8. Sources

- [Du et al., "Improving Factuality and Reasoning through Multiagent Debate" (ICML 2024)](https://arxiv.org/abs/2305.14325)
- [Liang et al., "Encouraging Divergent Thinking through Multi-Agent Debate" (EMNLP 2024)](https://arxiv.org/abs/2305.19118)
- [Smit et al., "Should we be going MAD?" (ICML 2024)](https://arxiv.org/abs/2311.17371)
- [Wang et al., "Rethinking the Bounds of LLM Reasoning" (ACL 2024)](https://aclanthology.org/2024.acl-long.331/)
- [FinCon (NeurIPS 2024)](https://arxiv.org/abs/2407.06567)
- [TradingAgents](https://arxiv.org/abs/2412.20138)
- [LangGraph Supervisor](https://github.com/langchain-ai/langgraph-supervisor-py)
- [LangGraph Hierarchical Agent Teams](https://langchain-ai.github.io/langgraph/tutorials/multi_agent/hierarchical_agent_teams/)
- [CrewAI Hierarchical Process](https://docs.crewai.com/en/learn/hierarchical-process)
- [AutoGen Conversation Patterns](https://microsoft.github.io/autogen/0.2/docs/tutorial/conversation-patterns/)
- [Society of HiveMind (SOHM)](https://arxiv.org/html/2503.05473v1)
- [Multi-Agent LLM Groupthink Research (EMNLP 2025 Findings)](https://aclanthology.org/2025.findings-emnlp.333.pdf)
- [FINSABER (long-horizon LLM trading evaluation)](https://arxiv.org/abs/2505.07078)
