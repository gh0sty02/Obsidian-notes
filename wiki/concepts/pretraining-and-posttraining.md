---
title: Pretraining & Post-Training
tags: [concept, llm, training, rlhf, fine-tuning]
sources: [sources/foundation-models.md]
last-updated: 2026-04-10
---

# Pretraining & Post-Training

Two distinct phases in building a useful LLM.

---

## Pretraining

Training a model from scratch on massive text data.

- Weights are initialized randomly, then adjusted through many iterations
- Objective: **next token prediction** (self-supervised — no human labels needed)
- The model reads text, predicts the next word, corrects itself, repeats billions of times
- Builds world knowledge, language understanding, reasoning patterns
- Most resource and compute intensive phase (requires FLOPs at scale)

**Why self-supervision works:** Text is self-labeling. Given `"My favorite color is ___"`, the correct answer is already in the corpus.

**Result:** A model that's very good at text completion but may produce racist, sexist, or incorrect outputs — it learned from the internet indiscriminately.

---

## Post-Training (Fine-Tuning)

Training on top of a pretrained model to make it useful and safe.

**Two steps:**

### 1. Supervised Fine-Tuning (SFT)
- Training data: high-quality (prompt, ideal response) pairs
- Optimizes the model for **conversation**, not just completion
- Requires fewer resources than pretraining

### 2. Preference Fine-Tuning
- Optimizes for responses humans actually prefer
- Techniques:
  - **RLHF** (Reinforcement Learning from Human Feedback) — human raters score responses, a reward model is trained on those scores, the LLM is optimized to maximize reward model scores
  - **DPO** (Direct Preference Optimization) — simpler alternative to RLHF, trains directly on preference pairs without a separate reward model
  - **RLAIF** (Reinforcement Learning from AI Feedback) — AI rates responses instead of humans (cheaper, scalable)

---

## Pretraining vs Post-Training: The Analogy

| Phase | Analogy |
|-------|---------|
| Pretraining | Reading books to acquire knowledge |
| Post-training | Learning how to apply and communicate that knowledge |

**Key difference in objective:**
- Pretraining optimizes token-level prediction accuracy
- Post-training optimizes for overall response quality that users prefer

---

## Relation to Fine-Tuning

"Fine-tuning" colloquially refers to post-training a model for a specific domain or task. Efficient fine-tuning methods like **LoRA** (Low-Rank Adaptation) allow fine-tuning with far fewer trainable parameters.

---

## Related
- [[concepts/model-sizing]]
- [[concepts/transformers]]
