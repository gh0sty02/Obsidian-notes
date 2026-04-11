---
title: Tokenization & Embeddings
tags: [concept, llm, tokenization, embeddings]
sources: [sources/foundation-models.md]
last-updated: 2026-04-10
---

# Tokenization & Embeddings

The first stage of any LLM forward pass. Text must be converted to numbers before the model can process it.

---

## Tokens

A token can be a character, a word, or part of a word.

**Example:** `"can't"` → `["can", "'t"]` — two tokens.

Typical LLMs use **subword tokenization** (BPE or similar). Common words are single tokens; rare words split into subwords.

**Why it matters:**
- Cost is measured in tokens, not words
- Context length = max tokens the model can process at once
- Token boundaries affect what the model "sees"

---

## Embeddings

After tokenization, each token is converted to a dense vector (list of numbers) that encodes its meaning.

**Token embedding** — captures semantic meaning. Words with similar meanings have vectors that are close in high-dimensional space.

**Positional embedding** — added to the token embedding to encode where in the sequence the token appears. Without this, the model has no sense of word order.

```
Final embedding = token_embedding + positional_embedding
```

The number of dimensions in the positional vector defines the **context length** of the model.

---

## Vocabulary

The full set of tokens a model knows. Modern LLMs have vocabularies of 32K–200K tokens. Each token is represented as a row in an embedding matrix (size: vocab_size × embedding_dim).

---

## Unembedding (Output)

After all transformer layers, the output vector for the last token is passed through an **unembedding layer** (reverse of embedding):
- Computes similarity between the output vector and every token in the vocabulary
- Outputs a probability distribution over all tokens
- A token is then [[concepts/sampling-techniques|sampled]] from this distribution

---

## Related
- [[concepts/transformers]]
- [[concepts/attention-mechanism]]
- [[concepts/sampling-techniques]]
