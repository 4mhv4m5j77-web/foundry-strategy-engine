# RiskLabs: Predicting Financial Risk Using Large Language Model Based on Multimodal and Multi-Sources Data
**Authors:** Yupeng Cao, Zhi Chen, Prashant Kumar, Qingyun Pei, Yangyang Yu, Haohang Li, Fabrizio Dimino, Lorenzo Ausiello, K.P. Subbalakshmi, Papa Momar Ndiaye  |  **Venue:** MFFM Workshop @ ICAIF '24  |  **arXiv:** 2404.07452  |  **Year:** 2024

## Core Claim
Direct LLM risk prediction is markedly ineffective — but LLMs serve as powerful information processors when their outputs are fused with classical deep learning models. RiskLabs integrates multimodal financial data (earnings calls, news, time series) through LLM-based encoders into a multi-task prediction framework that outperforms baselines on volatility forecasting and Value at Risk (VaR) prediction.

## Methodology
- **Four-module architecture:** (1) Earnings Conference Call Encoder (audio via Wav2Vec2 + MHSA, transcript via SimCSE + MHSA, LLM-based call analyzer with hierarchical summarization), (2) Time-Series Encoder (BiLSTM on VIX, 64 hidden states, 128-dim output), (3) News-Market Reactions Encoder (LLM analyzes news → historical similarity matching → market reaction prediction), (4) Multimodal Fusion + Multi-Task Prediction
- **Additive fusion:** E = w₀ + w₁·Tₐ + w₂·Tₜ + w₃·T_f + T_a + T_n + ε
- **Multi-task loss:** Jointly optimizes volatility prediction (3, 7, 15, 30 days) and 95% VaR using quantile regression
- **Data:** S&P 500 earnings conference calls (audio + transcripts), daily news, VIX time series; 8:2 temporal train/test split
- **LLM:** GPT-4 for Conference Call Analyzer and News Encoder, temperature=0; PyTorch implementation with 8-head attention

## Key Results
- RiskLabs achieves best MSE across 3, 7, and 15-day volatility forecasts (0.324, 0.585, 0.317, 0.233) vs. best classical baseline HTML (0.401, 0.845, 0.349, 0.251)
- VaR prediction: RiskLabs achieves 0.049 (closest to target 0.05) vs. GPT-3.5-Turbo direct prediction at 0.371 (essentially random)
- **Critical finding:** GPT-3.5-Turbo direct risk prediction MSE = 2.198 (worst by far) — LLMs fail catastrophically at direct numerical risk prediction
- Ablation: Audio+Text alone outperforms HTML baseline for 3-day; adding Analysis improves medium-term; adding VIX time-series critical for all horizons
- 30-day forecast lags behind HTML models, indicating LLM-based approaches struggle with longer-term risk forecasting

## V4 Relevance
- **Strongest empirical evidence for V4's core design principle:** LLMs as information processors/reasoners, NOT as direct predictors — reinforces "LLM reasons, algorithm executes" architecture
- **Multimodal fusion approach** validates V4's data pipeline design: LLM extracts signals from unstructured data → classical models make quantitative predictions
- **News-Market Reactions Encoder** with historical similarity matching provides a concrete implementation pattern for V4's regime-awareness module

## Limitations & Caveats
- 30-day volatility forecasting still inferior to HTML model — LLM-based features lose predictive power at longer horizons
- News quality is variable (misinformation introduces noise); scaling news sources introduces filtering challenges
- Position paper with relatively small experimental scope; needs validation across broader asset classes and market conditions

## Key References
- Andersen et al., 2001 — Distribution of realized stock return volatility (foundational for volatility modeling)
- Baevski et al., 2020 — Wav2Vec 2.0 (self-supervised speech representations)
- Gao et al., 2021 — SimCSE (contrastive sentence embeddings)
- Wu et al., 2023 — BloombergGPT (domain-specific financial LLM)
- Zhang et al., 2024b — FinAgent: multimodal financial agent
- Yu et al., 2023 — FinMem: layered memory trading agent
- Song et al., 2024 — OmniPred: language models as universal regressors
- Abbali et al., 2022 — Corporate credit risk sentiments from financial news
