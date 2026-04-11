---
title: Foundation Models — Study Notes
tags: [source, llm, transformers, training, attention]
sources: [raw/AI/Understanding foundation Models.md, raw/AI/Introduction.md]
last-updated: 2026-04-10
---

# Foundation Models — Study Notes

**Origin:** Personal study notes from the LLM Fundamentals course (Week 1, completed 2026-03-22)
**Files:** `raw/AI/Understanding foundation Models.md`, `raw/AI/Introduction.md`

---

## Key Claims

1. **Transformers are fundamentally about attention** — the ability for every token to look at every other token and decide how much to focus on it. This solves the long-range dependency problem that crippled RNNs.

2. **Model file size = 4 bytes × parameter count** (FP32), or 2 bytes (FP16). A 7B model ≈ 14–28 GB depending on precision.

3. **Post-training is necessary** because pretraining optimizes for token prediction, not human-preferred responses. Raw pretrained models can produce harmful output.

4. **MoE models are more efficient than they appear** — Mistral 8x7B has 46.7B total params but only 12.9B active per token, so inference cost matches a 12.9B dense model.

5. **Three proxies for model quality:** parameter count (capacity), training tokens (knowledge), FLOPs (training cost). None alone is sufficient.

---

## Language Model Types

| Type | Training Objective | Uses |
|------|-------------------|------|
| **Masked LM** (e.g., BERT) | Predict missing tokens using both directions | Classification, understanding, debugging |
| **Autoregressive LM** (e.g., GPT, Claude) | Predict next token from preceding tokens | Text generation, chat |

---

## Concepts Covered
- [[concepts/tokenization]]
- [[concepts/transformers]]
- [[concepts/attention-mechanism]]
- [[concepts/pretraining-and-posttraining]]
- [[concepts/sampling-techniques]]
- [[concepts/model-sizing]]
