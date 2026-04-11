---
title: Attention Mechanism
tags: [concept, llm, transformers, attention]
sources: [sources/foundation-models.md]
last-updated: 2026-04-10
---

# Attention Mechanism

The core innovation of [[concepts/transformers|transformers]]. For each token, attention answers: *"while processing this word, which other words should I focus on?"*

---

## The Three Vectors (Q, K, V)

Each token creates three vectors by multiplying its embedding by learned weight matrices:

| Vector | Name | Role |
|--------|------|------|
| **Q** | Query | What this token is looking for |
| **K** | Key | What information this token advertises |
| **V** | Value | The actual content this token provides |

---

## Step-by-Step Example

**Sentence:** `"The cat sat on the mat because it was tired"`

Goal: figure out what "it" refers to.

**Step 1 — Every word creates Q, K, V**
- "it" creates its Q (its question)
- Every word creates its K and V

**Step 2 — "it" asks its question via Q**
"I am a pronoun — who or what am I referring to?"

**Step 3 — Match Q against every word's K**
```
"it" Q  vs  "The"     K  →  score: 0.1
"it" Q  vs  "cat"     K  →  score: 0.8  ← high match
"it" Q  vs  "sat"     K  →  score: 0.1
"it" Q  vs  "on"      K  →  score: 0.0
"it" Q  vs  "mat"     K  →  score: 0.2
"it" Q  vs  "because" K  →  score: 0.0
"it" Q  vs  "tired"   K  →  score: 0.1
```

"cat" scores highest because its key advertises: *"I am a living creature, subject of the sentence"* — exactly what "it" is looking for.

**Step 4 — Convert scores to probabilities (softmax)**
All scores sum to 1.

**Step 5 — Collect information using weighted Values**
```
Final vector =
  0.6 × cat(V)    ← 60% of cat's information
+ 0.2 × mat(V)    ← 20% of mat's information
+ 0.1 × tired(V)  ← 10% of tired's information
+ 0.1 × others(V)
```

**Step 6 — "it" vector is updated**
```
Before attention: "it" = [0.5, 0.5, 0.5]  (generic pronoun)
After attention:  "it" = [0.8, 0.3, 0.6]  (heavily influenced by "cat")
```

Then the feed-forward layer processes the updated vector:
> "This is a pronoun referring to a living creature that is tired. Encode that meaning properly."

---

## Multi-Head Attention

In practice, transformers run attention **multiple times in parallel** (multiple "heads"), each learning different types of relationships (syntactic, semantic, coreference, etc.). Results are concatenated.

---

## Computational Cost

Attention is O(n²) in sequence length — every token attends to every other token. This is why context length is expensive and why techniques like sparse attention, flash attention, and sliding window attention exist for long contexts.

---

## Related
- [[concepts/transformers]]
- [[concepts/tokenization]]
