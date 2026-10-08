# V1 vs V2 Prompt Analysis

## Summary
**V1 (Concise Prompt) significantly outperforms V2 (Structured Expert Prompt)** on the key RAGAS metric (faithfulness), achieving **0.9723 ≥ 0.8** target.

## Detailed Comparison

| Metric | V1 (Concise) | V2 (Structured) | Winner | Difference |
|--------|-------------|----------------|--------|------------|
| **faithfulness** | **0.9723** | 0.7192 | **V1** | +0.2531 |
| answer_relevancy | 0.5929 | 0.4186 | V1 | +0.1743 |
| context_recall | 0.9800 | 0.9800 | Tie | 0 |
| context_precision | 0.9100 | 0.9100 | Tie | 0 |

## Why V1 Wins

### 1. Faithfulness (Primary Target: ≥ 0.8)
- **V1**: 0.9723 ✅ **EXCEEDS TARGET BY 21%**
- **V2**: 0.7192 ❌ **BELOW TARGET BY 10%**

**Root Cause**: V2's forced 4-section structure (summary, mechanism, example, conclusion) causes the model to **hallucinate plausible content** for sections not fully supported by retrieved context. V1's 2-4 sentence constraint forces the model to only state what's explicitly in the context.

### 2. Answer Relevancy
- **V1**: 0.5929 - Direct, focused answers
- **V2**: 0.4186 - Diluted by structural filler

**Root Cause**: V2 spends token budget on structural formatting rather than directly answering the question.

### 3. Context Metrics (Tie)
Both prompts use identical retriever (k=3 from same FAISS index), so context recall/precision are identical.

## Prompt Designs

### V1 - Concise (Winner)
```text
Bạn là trợ lý AI hữu ích. Chỉ dùng context sau để trả lời.
Giữ câu trả lời ngắn gọn (2-4 câu). Trả lời trực tiếp và chính xác.

Context:
{context}
```

### V2 - Structured Expert (Loser)
```text
Bạn là chuyên gia AI. Đọc kỹ context, xác định facts liên quan,
viết câu trả lời rõ ràng và có tổ chức (3-5 câu). Cấu trúc câu trả lời:
1. Tóm tắt khái niệm chính
2. Giải thích cơ chế/nguyên lý
3. Ví dụ hoặc ứng dụng thực tế (nếu có)
4. Kết luận ngắn gọn

Context:
{context}
```

## Key Insight

> **In RAG systems, simpler constrained prompts often outperform complex structured ones** because they reduce the model's tendency to hallucinate beyond the provided context.

The "expert structure" in V2 sounds better for human readers but **hurts faithfulness metrics** because the model fills structural gaps with plausible but unsupported content.

## Recommendation
**Use V1 (Concise) for production RAG** where factual accuracy (faithfulness) is critical. Consider V2 only for human-facing summaries where readability trumps strict factual adherence.

## Evidence Files
- `evidence/step3_ragas_report.json` - Full numerical results
- `evidence/step3_ragas_evaluation.md` - Detailed terminal output
- `evidence/step1_langsmith_traces.md` - Step 1 trace evidence
- `evidence/step2_prompt_hub_ab_routing.md` - Prompt Hub & A/B routing evidence
- `evidence/step4_pii_demo_log.txt` - PII detection test cases
- `evidence/step4_json_demo_log.txt` - JSON formatting test cases