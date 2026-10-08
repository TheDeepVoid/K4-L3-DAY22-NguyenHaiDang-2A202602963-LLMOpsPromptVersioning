# Step 1 Evidence: LangSmith RAG Pipeline Traces

## LangSmith Project
- **Project Name**: `day22-lab`
- **URL**: https://smith.langchain.com/projects/p/day22-lab (or search "day22-lab" in your LangSmith dashboard)

## Traces Summary
- **Total Traces**: 50+ (from Step 1) + 50+ (from Step 2 A/B routing) = **100+ traces**
- **Trace Names**: `rag-query` (Step 1), `ab-rag-query` (Step 2)
- **Tags**: `rag`, `step1` | `ab-test`, `step2`

## Terminal Output (Step 1 - 50 Questions)
```
============================================================
  Bước 1: LangSmith RAG Pipeline
============================================================
✅ Config OK  |  Provider: OPENROUTER  |  Project: day22-lab
⚠️  Using local sentence-transformers embeddings (no API key required)
📚 Đã chia thành 107 chunks
🔨 Đang tạo FAISS index từ 107 chunks ...
✅ FAISS vectorstore đã sẵn sàng.
[01/50] Q: What are the three main types of machine learning?
       A: The three main types of machine learning are: 1. **Supervised learning** — models train on labeled...
[02/50] Q: What is overfitting in machine learning?
       A: Overfitting happens when a model learns the training data too well — including its noise and quirks...
...
[50/50] Q: What are common AI safety concerns with LLMs?
       A: Common AI safety concerns include: alignment problems, bias and fairness issues, privacy violations...

✅ 50 traces đã gửi lên LangSmith project 'day22-lab'
   Mở https://smith.langchain.com để xem traces.
```

## How to Verify in LangSmith Dashboard
1. Go to https://smith.langchain.com
2. Select project: `day22-lab`
3. You should see 50+ traces named `rag-query`
4. Click any trace to see:
   - Retrieval step (k=3 docs)
   - Prompt template with context
   - LLM invocation
   - Output parsing
   - Latency and token usage