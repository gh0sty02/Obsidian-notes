---
title: Transformers
tags: [concept, llm, architecture, deep-learning]
sources: [sources/foundation-models.md]
last-updated: 2026-04-10
---

# Transformers

The dominant architecture for modern LLMs, introduced by Google. Transformers solve the core problem of understanding relationships between words across an entire sequence — not just adjacent words.

**The core problem they solve:** Given `"The animal did not cross the road because it was too tired"`, how does the model know "it" refers to the animal, not the road? Transformers read the full sentence and establish relationships via [[concepts/attention-mechanism|attention]].

---

## Architecture

A transformer has two components:

### Encoder
- Takes in the input sequence
- Builds a rich contextual understanding via attention
- Outputs a numerical representation (vector) of meaning
- Used in: BERT, masked language models

### Decoder
- Takes in the encoder's output (or previous tokens)
- Generates output word by word
- Also uses attention (both self-attention and cross-attention)
- Used in: GPT, autoregressive models

Most modern chat LLMs (GPT, Claude, Llama) are **decoder-only**.

---

## Forward Pass (What Happens When You Type a Prompt)

1. **[[concepts/tokenization|Tokenization]]** — input text is broken into tokens
2. **Embeddings** — each token is converted to a vector that encodes meaning + position
3. **Transformer Layers** (repeated N times):
   - **[[concepts/attention-mechanism|Attention Layer]]** — each token attends to every other token, updating its vector to absorb context
   - **Feed-Forward Layer** — each token's updated vector passes through a neural network for further processing
4. **Unembedding Layer** — the final vector is mapped back to a probability distribution over the vocabulary
5. **[[concepts/sampling-techniques|Sampling]]** — a token is selected from the distribution

---

## Key Innovation

The **attention mechanism** — instead of processing words sequentially (like RNNs), transformers process all tokens simultaneously and let each token decide which other tokens to focus on. This enables:
- Parallel processing (faster training)
- Long-range dependencies (understanding context across thousands of tokens)
- Scalability to massive parameter counts

---

## Related
- [[concepts/attention-mechanism]]
- [[concepts/tokenization]]
- [[concepts/sampling-techniques]]
- [[concepts/model-sizing]]
- [[concepts/pretraining-and-posttraining]]
