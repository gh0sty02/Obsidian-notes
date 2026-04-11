---

---
# 

> **Your Background**: Senior SWE (3+ yrs) | React, TypeScript, Node.js | Targeting Staff/Principal roles
**Goal**: Pivot to AI Engineering with production-ready skills

---

## 🎯 Learning Philosophy

1. **Build > Read**: Code every day, reading alone won't stick
2. **Public learning**: Share projects on GitHub, write about what you build
3. **Depth over breadth**: Master RAG before touching fine-tuning
4. **Interview prep mindset**: Connect each topic to "how would I build X at scale?"

---

## Phase 1: Foundations (Weeks 1-3)

### Week 1: LLM Fundamentals & API Basics

**Core Concepts**

- Transformers, tokens, context windows, temperature
- OpenAI vs Anthropic vs open-source models
- Cost calculations, rate limits

**Resources**

- 📖 [Andrej Karpathy - Intro to LLMs](https://www.youtube.com/watch?v=zjkBMFhNj_g) (1hr video)
- 📖 [OpenAI Cookbook](https://github.com/openai/openai-cookbook)
- 📖 [Anthropic Claude API Docs](https://docs.anthropic.com/en/docs/intro-to-claude)
- 🛠️ [Replicate](https://replicate.com/) - Test multiple models quickly

**Practice**

```python
# Day 1-2: Basic completions
# Day 3-4: Streaming responses
# Day 5-7: Build a CLI chatbot with conversation memory
```

**Deliverable**: CLI chatbot that maintains conversation context (use JSON file for memory)

---

### Week 2: Prompt Engineering Deep Dive

**Core Concepts**

- Zero-shot, few-shot, chain-of-thought
- System prompts, temperature tuning
- Structured outputs (JSON mode, function calling)
- Prompt caching, batching

**Resources**

- 📖 [Anthropic Prompt Engineering Guide](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- 📖 [OpenAI Prompt Engineering](https://platform.openai.com/docs/guides/prompt-engineering)
- 📖 [Prompt Engineering Guide](https://www.promptingguide.ai/)
- 🎓 [DeepLearning.AI - ChatGPT Prompt Engineering](https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/)

**Practice**

- Write prompts for: code review, data extraction, summarization
- Experiment with temperature (0.0 → 1.0)
- Build a prompt testing harness (compare outputs across prompts)

**Deliverable**: GitHub repo with 10+ production-quality prompts + comparison tool

---

### Week 3: Embeddings & Semantic Search

**Core Concepts**

- Text embeddings (OpenAI ada-002, Cohere, open-source)
- Cosine similarity, dot product
- Chunking strategies (fixed-size, semantic, recursive)
- When to use embeddings vs keyword search

**Resources**

- 📖 [OpenAI Embeddings Guide](https://platform.openai.com/docs/guides/embeddings)
- 📖 [Chunking Strategies Deep Dive](https://www.pinecone.io/learn/chunking-strategies/)
- 🛠️ [LangChain Text Splitters](https://python.langchain.com/docs/modules/data_connection/document_transformers/)
- 📖 [Vicki Boykis - What are embeddings?](https://vickiboykis.com/what_are_embeddings/)

**Practice**

```python
# Day 1-2: Generate embeddings, compute similarity
# Day 3-4: Experiment with chunking strategies
# Day 5-7: Build semantic search over your bookmarks/notes
```

**Deliverable**: Personal knowledge search engine (search your Notion/Obsidian/bookmarks)

---

## Phase 2: RAG Mastery (Weeks 4-7)

### Week 4: Vector Databases

**Core Concepts**

- FAISS (local), Pinecone, Weaviate, Chroma, Qdrant
- Indexing, querying, filtering
- Hybrid search (sparse + dense)
- Metadata filtering

**Resources**

- 📖 [Pinecone Learning Center](https://www.pinecone.io/learn/)
- 📖 [Weaviate Academy](https://weaviate.io/developers/academy)
- 🛠️ [Chroma](https://www.trychroma.com/) - Start here (easiest)
- 📖 [Vector DB Comparison](https://superlinked.com/vector-db-comparison/)

**Practice**

- Set up Chroma locally
- Index 1000+ documents
- Implement metadata filtering
- Compare retrieval quality across DBs

**Deliverable**: Vector DB comparison repo (Chroma vs FAISS vs Pinecone benchmarks)

---

### Week 5-6: RAG Implementation (Core Skill)

**Core Concepts**

- Naive RAG → Advanced RAG
- Query transformation (HyDE, query expansion, multi-query)
- Reranking (Cohere, Cross-Encoder)
- Context window optimization
- Citation & source tracking

**Resources**

- 📖 [LangChain RAG Tutorial](https://python.langchain.com/docs/tutorials/rag/)
- 📖 [Advanced RAG Techniques](https://www.anthropic.com/research/contextual-retrieval) (Anthropic)
- 🎓 [DeepLearning.AI - Building RAG Apps](https://www.deeplearning.ai/short-courses/building-applications-vector-databases/)
- 📖 [RAG Evaluation - RAGAS](https://docs.ragas.io/en/latest/)
- 📺 [Full Stack RAG Tutorial - Greg Kamradt](https://www.youtube.com/watch?v=sVcwVQRHIc8)

**Practice Progression**

5. **Week 5**: Naive RAG (retrieve → stuff context → generate)
6. **Week 6**: Advanced RAG (reranking, HyDE, query expansion)

**Deliverable**: Production RAG system for a real use case

- **Idea 1**: "Ask my codebase" (index your GitHub repos)
- **Idea 2**: "Company docs chatbot" (scrape docs, make searchable)
- **Idea 3**: "Research assistant" (ingest papers, answer questions)

**Must-haves**:

- Streaming responses
- Source citations
- Cost tracking
- Evaluation metrics

---

### Week 7: Evaluation & Iteration

**Core Concepts**

- Retrieval metrics: Precision@K, Recall@K, MRR, NDCG
- Generation metrics: Faithfulness, answer relevancy
- RAGAS framework
- Human-in-the-loop evaluation

**Resources**

- 📖 [RAGAS Documentation](https://docs.ragas.io/)
- 📖 [LangSmith Evaluation](https://docs.smith.langchain.com/evaluation)
- 📖 [Eugene Yan - Evaluating RAG](https://eugeneyan.com/writing/evaluating-rag/)

**Practice**

- Create test dataset for your Week 5-6 project
- Run RAGAS metrics
- A/B test chunking strategies
- Build an evaluation dashboard

**Deliverable**: Evaluation report showing RAG improvements (v1 → v2 metrics)

---

## Phase 3: Production Skills (Weeks 8-10)

### Week 8: LangChain & LangGraph

**Core Concepts**

- Chains, agents, memory types
- Tool/function calling
- LangGraph for complex workflows
- **When NOT to use frameworks** (important!)

**Resources**

- 📖 [LangChain Docs](https://python.langchain.com/docs/get_started/introduction)
- 📖 [LangGraph Docs](https://langchain-ai.github.io/langgraph/)
- 🎓 [DeepLearning.AI - LangChain](https://www.deeplearning.ai/short-courses/langchain-for-llm-application-development/)
- 🎓 [DeepLearning.AI - LangGraph](https://www.deeplearning.ai/short-courses/ai-agents-in-langgraph/)

**Practice**

- Rebuild your Week 6 RAG project using LangChain
- Create a multi-step agent with tools
- Implement conversation memory (buffer, summary, vector)

**Deliverable**: Refactor previous project using LangChain (compare code complexity)

---

### Week 9: API Development & Deployment

**Core Concepts**

- FastAPI for AI endpoints
- Streaming responses (SSE)
- Rate limiting, caching (Redis)
- Error handling, retries
- Background tasks (Celery, RQ)

**Resources**

- 📖 [FastAPI Docs](https://fastapi.tiangolo.com/)
- 📖 [FastAPI + Streaming](https://github.com/tiangolo/fastapi/discussions/8701)
- 🛠️ [Modal](https://modal.com/) - Easiest deployment
- 🛠️ [Railway](https://railway.app/) - Good for full-stack
- 📖 [Vercel AI SDK](https://sdk.vercel.ai/docs) - For Next.js integration

**Practice**

```python
# Day 1-2: FastAPI + OpenAI streaming
# Day 3-4: Add Redis caching
# Day 5-7: Deploy to Modal/Railway
```

**Deliverable**: Deployed RAG API with:

- `/chat` endpoint (streaming)
- `/search` endpoint (vector search)
- Swagger docs
- Public URL

---

### Week 10: Frontend + Full-Stack Integration

**Core Concepts**

- React + AI streaming (Vercel AI SDK)
- Real-time UI updates
- Cost/token tracking on frontend
- Error states, loading states

**Resources**

- 📖 [Vercel AI SDK](https://sdk.vercel.ai/docs)
- 📖 [React Streaming Example](https://github.com/vercel/ai-chatbot)
- 🛠️ [shadcn/ui](https://ui.shadcn.com/) - Use your existing UI skills

**Practice**

- Build Next.js frontend for your RAG API
- Implement streaming chat interface
- Add source citations UI
- Deploy to Vercel

**Deliverable**: Full-stack RAG app (Next.js + FastAPI/Modal)

- **Portfolio piece**: This is your "hire me" project

---

## Phase 4: Advanced & Specialization (Weeks 11-12)

### Week 11: AI Agents (Hot in 2025)

**Core Concepts**

- ReAct pattern (Reasoning + Acting)
- Tool use, code execution
- Multi-agent systems
- Agent evaluation

**Resources**

- 📖 [Anthropic Tool Use Docs](https://docs.anthropic.com/en/docs/build-with-claude/tool-use)
- 📖 [LangGraph Multi-Agent](https://langchain-ai.github.io/langgraph/tutorials/multi_agent/)
- 📖 [Lilian Weng - LLM Agents](https://lilianweng.github.io/posts/2023-06-23-agent/)
- 🛠️ [AutoGPT](https://github.com/Significant-Gravitas/AutoGPT)

**Practice**

- Build a coding agent (writes + executes code)
- Build a research agent (searches web + summarizes)
- Combine agents into a system

**Deliverable**: Multi-agent system (e.g., "AI teammate" that can research + code + summarize)

---

### Week 12: MLOps for LLMs + Optional Fine-tuning

**Core Concepts**

- Prompt versioning (Git-based)
- A/B testing prompts
- Observability (LangSmith, Helicone, Braintrust)
- Cost optimization
- **Fine-tuning** (if time permits)

**Resources**

- 📖 [LangSmith](https://docs.smith.langchain.com/)
- 📖 [Helicone](https://www.helicone.ai/)
- 📖 [Braintrust](https://www.braintrust.dev/)
- 📖 [OpenAI Fine-tuning](https://platform.openai.com/docs/guides/fine-tuning) (optional)

**Practice**

- Set up LangSmith for your projects
- Implement prompt versioning
- Track costs across projects
- (Optional) Fine-tune a small model

**Deliverable**: Observability dashboard for all your projects

---

## 🚀 Portfolio Projects (Build These)

By end of 12 weeks, you should have:

7. ✅ **Personal Knowledge Search** (Week 3)
8. ✅ **Production RAG System** (Week 5-6) - domain-specific Q&A
9. ✅ **Full-Stack RAG App** (Week 10) - **Main portfolio piece**
10. ✅ **AI Agent System** (Week 11) - autonomous task execution
11. ✅ **Evaluation Framework** (Week 7) - reusable testing harness

**GitHub Strategy**:

- Create `ai-engineering-portfolio` repo
- Each project = separate folder with README
- Include metrics, cost analysis, learnings

---

## 📊 Skill Validation Checklist

After 12 weeks, you should be able to:

### RAG & Vector DBs

- [ ] Explain naive vs advanced RAG with examples
- [ ] Choose appropriate chunking strategy for a use case
- [ ] Implement hybrid search (keyword + semantic)
- [ ] Debug retrieval quality issues
- [ ] Compare vector DBs and recommend based on requirements

### Prompt Engineering

- [ ] Write production-quality prompts for any task
- [ ] Implement few-shot learning effectively
- [ ] Use function calling for structured outputs
- [ ] Optimize prompts for cost/quality tradeoff

### Production Skills

- [ ] Deploy streaming AI API (FastAPI)
- [ ] Build real-time AI frontend (Next.js)
- [ ] Implement proper error handling & retries
- [ ] Set up monitoring & cost tracking

### Agents

- [ ] Build ReAct-style agent with tools
- [ ] Implement function calling for tool use
- [ ] Debug agent reasoning loops

### Evaluation

- [ ] Measure retrieval quality (precision/recall)
- [ ] Run RAGAS metrics on RAG systems
- [ ] Create test datasets for AI apps

---

## 🎯 Interview Prep Integration

Map AI skills to your DevKit platform patterns:

**System Design**:

- "Design a scalable RAG system for 1M docs"
- "Design a multi-tenant AI chatbot"
- "Design real-time AI agent infrastructure"

**Coding**:

- LeetCode-style but for AI: "Implement semantic deduplication"
- "Build a prompt testing framework"
- "Optimize vector search query"

**Behavioral**:

- "Tell me about your RAG project" (Week 5-6)
- "How did you improve retrieval accuracy?" (Week 7)
- "What's your approach to prompt engineering?" (Week 2)

---

## 🔥 Hot Takes & Principles

12. **RAG > Fine-tuning**: 90% of companies use RAG, start there
13. **LangChain is optional**: Learn vanilla implementations first
14. **Agents are the future**: Focus here after RAG mastery
15. **Evaluation is underrated**: Most engineers skip this, don't be them
16. **Build in public**: Tweet progress, share learnings, grow network

---

## 📚 Bonus Resources

**Newsletters**:

- [Latent Space](https://www.latent.space/)
- [The AI Engineer](https://www.theaiengineer.com/)
- [AI Jason](https://www.aijason.com/)

**Communities**:

- [LangChain Discord](https://discord.gg/langchain)
- [AI Engineer World's Fair](https://www.ai.engineer/)
- [r/LangChain](https://www.reddit.com/r/LangChain/)

**YouTube**:

- [Sam Witteveen](https://www.youtube.com/@samwitteveenai) - LangChain tutorials
- [Greg Kamradt](https://www.youtube.com/@DataIndependent) - RAG deep dives
- [AI Jason](https://www.youtube.com/@AIJasonZ) - Weekly AI news

**Books** (optional):

- "Designing Machine Learning Systems" - Chip Huyen
- "Building LLMs for Production" - Eugene Yan (blog)

---

## 🎓 Next Steps After 12 Weeks

**If pursuing Staff/Principal roles**:

17. Contribute to LangChain/LlamaIndex/ChromaDB
18. Write technical blog posts on RAG optimizations
19. Speak at local AI meetups
20. Build a widely-used open-source AI tool

**Advanced Topics (Weeks 13+)**:

- Fine-tuning deep dive (LoRA, QLoRA, full fine-tuning)
- Multimodal RAG (images, audio, video)
- Graph RAG (Neo4j + LLMs)
- LLM quantization, optimization
- Custom model hosting (vLLM, TGI)

---

## ⏱️ Time Commitment

- **Minimum**: 10 hrs/week = part-time pivot (18 weeks)
- **Recommended**: 15-20 hrs/week = 12 weeks to production-ready
- **Intensive**: 30+ hrs/week = 6-8 weeks (if between jobs)

**Daily Structure**:

- 1 hr: Reading/watching
- 2 hrs: Coding/building
- 30 min: Writing/documenting

---

## 🎯 Success Metrics

**Week 6**: Ship your first RAG app
**Week 10**: Have a deployed full-stack AI app in your portfolio
**Week 12**: Confidently interview for AI Engineer roles

**You'll know you've succeeded when**:

- You can explain RAG to a PM in 2 minutes
- You've built 3+ production AI apps
- You're debugging vector search queries in your sleep
- Recruiters start reaching out for AI roles

---

**Last Updated**: March 2026
**Author**: Claude (for Pranay)
**License**: Use freely, share widely

---

*Good luck! Build in public, ship fast, and remember: the best way to learn AI engineering is to build things that break, then fix them. 🚀*