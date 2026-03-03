# LLM-FE: Automated Feature Engineering for Tabular Data with LLMs as Evolutionary Optimizers
**Authors:** Nikhil Abhyankar, Parshin Shojaee, Chandan K. Reddy  |  **Venue:** Preprint (Under review)  |  **arXiv:** 2503.14434  |  **Year:** 2025

## Core Claim
LLMs can serve as knowledge-guided evolutionary optimizers for automated feature engineering on tabular data. LLM-FE formulates feature engineering as a program search problem where LLMs propose feature transformation programs iteratively, guided by data-driven feedback, consistently outperforming classical and LLM-based baselines across diverse classification and regression benchmarks.

## Methodology
- **Iterative four-step loop:** (a) New Feature Generation — LLM generates Python feature transformation programs as hypotheses; (b) Feature Engineering — programs applied to augment original dataset; (c) Feature Evaluation — prediction model (XGBoost/MLP/TabPFN) trained on augmented data, scored on validation set; (d) Experience Management — multi-population "island" memory buffer stores high-scoring programs for in-context learning
- **Evolutionary search:** Multi-island model (m=3 islands) with Boltzmann sampling for cluster selection; LLM acts as mutation/crossover operator guided by domain knowledge and prior successes
- **Structured prompting:** Task description, feature descriptions, data samples, evaluation function, and in-context demonstrations from memory buffer (k=2 examples per island)
- **Backbone LLMs:** GPT-3.5-Turbo and Llama-3.1-8B-Instruct; temperature=0.8; b=3 programs per iteration; T=20 iterations total
- **Evaluation:** 19 classification + 10 regression datasets from OpenML, UCI, Kaggle; 80/20 train-test split; 5 random seeds; compared against AutoFeat, OpenFE, FeatLLM, CAAFE, OCTree

## Key Results
- **Mean rank 1.47** across 19 classification datasets with XGBoost (vs. next best OCTree at 3.84, OpenFE at 3.26)
- **Mean rank 1.00** across 10 regression datasets (best on every dataset)
- Generalizes across prediction models: improves XGBoost (0.820→0.840 accuracy), MLP (0.745→0.791), TabPFN (0.852→0.863)
- Feature transferability: XGBoost-generated features improve MLP and TabPFN performance, suggesting features capture genuine data characteristics
- **Ablation:** Domain knowledge removal drops accuracy from 0.687→0.626; evolutionary refinement removal causes largest drop to 0.587; data examples removal has smallest impact (0.644)

## V4 Relevance
- **Validates LLM-as-feature-engineer pattern** central to V4's Idea Agent: LLM proposes features (alpha factors) → ML model evaluates → feedback loop refines — directly parallel to V4's Idea Agent → Forge → Contest cycle
- **Evolutionary search with memory buffer** provides concrete implementation template for V4's Thesis Library: store successful strategies, use them as in-context examples for generating new ones
- **Domain knowledge is critical** (ablation shows significant drop without it) — supports V4's approach of feeding financial domain context to the Idea Agent rather than relying on generic prompting

## Limitations & Caveats
- Tested on general tabular ML tasks, not financial data specifically — transfer to alpha factor generation needs validation
- Computational cost: 20 LLM iterations with 3 programs each = 60 LLM calls per dataset; may need optimization for real-time trading
- Currently limited to feature transformation; does not address feature selection or the full ML pipeline

## Key References
- Chen & Guestrin, 2016 — XGBoost (primary evaluation model)
- Hollmann et al., 2022 — TabPFN (transformer for tabular data)
- Hollmann et al., 2024 — CAAFE (context-aware LLM feature engineering, NeurIPS)
- Han et al., 2024 — FeatLLM (LLM-based feature engineering for few-shot learning)
- Zhang et al., 2023 — OpenFE (automated feature generation, ICML)
- Romera-Paredes et al., 2024 — FunSearch: mathematical discoveries with LLMs (Nature)
- Shojaee et al., 2024 — LLM-SR: scientific equation discovery via LLM programming
- Meyerson et al., 2024 — Language model crossover via few-shot prompting (evolutionary learning)
- Nam et al., 2024 — OCTree: feature generation via LLMs with decision tree reasoning
- Lehman et al., 2023 — Evolution through large models (evolutionary ML handbook)
