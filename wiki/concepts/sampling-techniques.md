---
title: Sampling Techniques
tags: [concept, llm, sampling, inference, temperature]
sources: [sources/foundation-models.md]
last-updated: 2026-04-10
---

# Sampling Techniques

How an LLM picks the next token from a probability distribution.

The model outputs a **logit vector** (one value per vocabulary token) → converted to probabilities via **softmax** → one token is selected.

---

## Greedy Sampling

Always pick the highest-probability token. Simple, deterministic, but often produces repetitive or bland output. Works well for classification tasks.

---

## Temperature

Reshapes the probability distribution before sampling.

| Temperature | Effect |
|-------------|--------|
| `< 1.0` (e.g., 0.2) | Sharpens distribution — high-prob tokens dominate, model is focused and predictable |
| `= 1.0` | Probabilities unchanged — model samples as-is |
| `> 1.0` (e.g., 1.5) | Flattens distribution — gaps shrink, model is more creative and surprising |

**Use low temp for:** factual Q&A, code generation, structured output  
**Use high temp for:** creative writing, brainstorming, exploration

---

## Top-K Sampling

Instead of considering all 50K+ vocabulary tokens, keep only the **top K** by probability and sample from those.

**Example (K=3):** Only the three most likely tokens are candidates. Ignores the long tail entirely.

**Risk:** If K is too small, the model may be forced into bad choices in unusual contexts.

---

## Top-P (Nucleus) Sampling

Instead of a fixed count, keep tokens until their **cumulative probability exceeds P**.

**Example (P=0.9):**
```
mat   = 40%  → running total: 40%
floor = 25%  → running total: 65%
chair = 20%  → running total: 85%
roof  = 10%  → running total: 95% ← crosses 90%, stop
```
Sample from: mat, floor, chair, roof. Discard everything below.

**Advantage over Top-K:** Adapts to the situation. When the model is confident (steep distribution), fewer tokens are included. When uncertain (flat distribution), more tokens are included.

---

## In Practice

These are typically combined:
1. Temperature reshapes the distribution
2. Top-P cuts the long tail
3. Top-K adds a hard cap on candidates
4. One token is sampled from what remains

---

## Related
- [[concepts/transformers]]
- [[concepts/tokenization]]
