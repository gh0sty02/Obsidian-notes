---
title: Model Sizing & Parameters
tags: [concept, llm, model-size, parameters, moe]
sources: [sources/foundation-models.md]
last-updated: 2026-04-10
---

# Model Sizing & Parameters

How to reason about the scale of LLMs and what "parameter count" actually means.

---

## What is a Parameter?

A parameter (also called a weight) is a numerical value that influences model predictions. During training, parameters are adjusted millions of times to minimize prediction error.

```
Total Parameters = (number of weights) + (number of biases)
```

---

## Memory Calculation

```
Model file size = bytes per parameter × total parameter count
```

| Precision | Bytes per param | 7B model | 70B model |
|-----------|----------------|----------|-----------|
| FP32 (full) | 4 bytes | 28 GB | 280 GB |
| FP16 / BF16 | 2 bytes | 14 GB | 140 GB |
| INT8 (quantized) | 1 byte | 7 GB | 70 GB |

---

## What Parameters Measure

| Proxy | Measures |
|-------|----------|
| Number of parameters | Learning capacity |
| Number of training tokens | How much the model learned |
| Number of FLOPs | Training cost |

A small model trained on massive data can outperform a large model trained on little data. Parameter count alone is not sufficient — training data quantity matters equally.

---

## Dense vs Sparse Models

**Dense model:** All parameters active for every token.

**Sparse model:** Most parameters are zero. More storage-efficient, can require less compute than apparent size suggests.

---

## MoE (Mixture of Experts)

A sparse architecture where the model is divided into "expert" parameter groups. Only a subset of experts activate per token.

**Example — Mistral 8x7B:**
- 8 experts, each ~7B parameters
- Total: 46.7B parameters (some shared, so not 56B)
- Per token: only 2 experts activate → 12.9B active parameters
- **Inference cost = equivalent to a 12.9B dense model**, despite 46.7B total params

**Key insight:** MoE allows large model capacity with smaller inference cost.

---

## Scaling Laws

Larger models generally outperform smaller ones at the same task, and newer models outperform older ones at the same size. But there are diminishing returns — optimal training requires balancing model size with training token count (Chinchilla scaling laws).

---

## Related
- [[concepts/pretraining-and-posttraining]]
- [[concepts/transformers]]
