---
title: Prompt Engineering
tags: [concept, llm, prompting, few-shot, chain-of-thought, in-context-learning]
sources: [sources/ai-engineering-roadmap.md, sources/prompt-engineering-raw.md]
last-updated: 2026-04-17
---

# Prompt Engineering

Designing inputs to LLMs to reliably produce high-quality outputs. A core skill for AI engineers.

---

## Prompt Components

A well-structured prompt typically has:

1. **System prompt** — persona, constraints, output format, tone
2. **Examples** (few-shot) — demonstrate expected behavior
3. **Context** — relevant information the model needs
4. **Task** — the specific thing you want done
5. **Constraints** — format requirements, length, what to avoid

---

## Core Techniques

### Zero-Shot
Ask directly with no examples. Works for simple tasks and capable models.
```
Classify the sentiment: "The product broke after one day."
```

### Few-Shot
Provide 2–5 examples of (input → output) before the real task. Dramatically improves performance on structured tasks.

### Chain-of-Thought (CoT)
Prompt the model to reason step-by-step before answering. Add "Let's think step by step" or provide CoT examples.
- Improves accuracy on math, logic, multi-step reasoning
- Increases token usage and cost

### System Prompts
Set the model's persona and constraints at the start of a conversation. Claude and GPT both support a distinct system role.

---

## Structured Output

For programmatic use, get the model to output JSON:
- **JSON mode** (OpenAI) — guarantees valid JSON
- **Function calling / tool use** — define a schema, model fills it
- **Explicit formatting in prompt** — "respond only with a JSON object with keys: ..."

---

## Temperature & Quality Trade-offs

| Low temperature (0.0–0.3) | High temperature (0.7–1.0) |
|--------------------------|---------------------------|
| Factual Q&A | Creative writing |
| Code generation | Brainstorming |
| Structured extraction | Exploration |
| Classification | Generating diverse options |

---

## Prompt Injection & Security

**Prompt injection:** Malicious input that overrides the system prompt or changes model behavior.

**Defenses:**
- Validate and sanitize user input
- Use XML/delimiter tags to separate system context from user input
- Don't trust model output for security-critical decisions
- Separate untrusted content from instructions (e.g., `<user_input>...</user_input>`)

---

## Prompt Caching

Anthropic and OpenAI support caching long system prompts. If the same prefix is reused, subsequent calls are cheaper and faster. Critical for production cost optimization.

---

## Systematic Experimentation

Treat prompts like code:
- Version control your prompts
- A/B test variants with a test dataset
- Log inputs, outputs, and scores
- Don't tweak prompts based on one example — test across a diverse set

---

## In-Context Learning

In-context learning (ICL) means the model learns from examples or information within the current prompt — no weight updates occur.

**Prompt vs Context distinction:**
- **Prompt** — the entire input to the model
- **Context** — the information provided within the prompt for the model to use

### Zero-Shot & Few-Shot (ICL)
- Each example in the prompt = one "shot"
- More examples → better performance, up to context length limits
- Trade-off: more shots = longer prompt = higher inference cost
- Practical ceiling: 5–10 shots for most tasks

### Needle in a Haystack Test
Used to evaluate **context efficiency** (not just context length):
- Insert a specific fact ("needle") at different positions in a long document ("haystack")
- Ask the model to retrieve the needle
- Reveals positional biases — some models perform better with instructions at start vs end
- GPT-4 and Claude show different retrieval patterns across context positions

### Context Window Considerations
- Longer context ≠ better retrieval — models may ignore middle content ("lost in the middle" phenomenon)
- Critical instructions: place at start OR end of context
- Anthropic's prompt engineering tutorial recommends testing retrieval quality, not just context length

## Anthropic Resources

- **Interactive Tutorial:** `github.com/anthropics/prompt-eng-interactive-tutorial` — comprehensive step-by-step guide to prompting Claude
- **Google Cloud Guide:** Covers prompt engineering fundamentals with vendor-neutral perspective

## Related
- [[concepts/rag-architecture]]
- [[concepts/sampling-techniques]]
- [[concepts/tokenization]] — context length is measured in tokens
