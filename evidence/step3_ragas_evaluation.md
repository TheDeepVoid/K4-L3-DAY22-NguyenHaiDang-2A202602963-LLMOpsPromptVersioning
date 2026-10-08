# Step 3 Evidence: RAGAS Evaluation

## RAGAS Report File
- **Location**: `data/ragas_report.json`
- **Content**:
```json
{
  "prompt_v1_scores": {
    "faithfulness": 0.9722619047619048,
    "answer_relevancy": 0.5929104453899547,
    "context_recall": 0.98,
    "context_precision": 0.9099999999176666
  },
  "prompt_v2_scores": {
    "faithfulness": 0.7191504606504605,
    "answer_relevancy": 0.418574499690935,
    "context_recall": 0.98,
    "context_precision": 0.9099999999196666
  },
  "target_met": true
}
```

## Comparison Table

| Metric | V1 (Concise) | V2 (Structured) | Winner | Target |
|--------|-------------|----------------|--------|--------|
| **faithfulness** | **0.9723** | 0.7192 | ← **V1** | ✅ ≥ 0.8 |
| answer_relevancy | 0.5929 | 0.4186 | ← V1 | - |
| context_recall | 0.9800 | 0.9800 | Tie | - |
| context_precision | 0.9100 | 0.9100 | Tie | - |

## Key Finding
**TARGET MET**: V1 faithfulness = **0.9723 ≥ 0.8** ✅

V1 (concise prompt) significantly outperforms V2 (structured expert prompt) on faithfulness because:
- V1's 2-4 sentence constraint forces model to only use retrieved context
- V2's 4-section structure encourages hallucination to fill all sections

## Terminal Output (Step 3 - Full Evaluation)
```
============================================================
  Bước 3: RAGAS Evaluation
============================================================
✅ Config OK  |  Provider: OPENROUTER  |  Project: day22-lab
⚠️  Using local sentence-transformers embeddings (no API key required)
🔨 Đang tạo FAISS index từ 107 chunks ...
✅ FAISS vectorstore đã sẵn sàng.

🚀 Đang chạy 50 câu hỏi với prompt v1 ...
  [01/50] What are the three main types of machine learning?
  ...
  [50/50] What are common AI safety concerns with LLMs?

🚀 Đang chạy 50 câu hỏi với prompt v2 ...
  [01/50] What are the three main types of machine learning?
  ...
  [50/50] What are common AI safety concerns with LLMs?

📐 Đang đánh giá RAGAS cho prompt v1 ... (vui lòng chờ ~5-10 phút)
...
Evaluating: 100%|██████████| 200/200 [09:34<00:00,  2.87s/it]
   📊 v1 scores:
      faithfulness: 0.9723
      answer_relevancy: 0.5929
      context_recall: 0.9800
      context_precision: 0.9100

📐 Đang đánh giá RAGAS cho prompt v2 ... (vui lòng chờ ~5-10 phút)
...
Evaluating: 100%|██████████| 200/200 [09:23<00:00,  2.82s/it]
   📊 v2 scores:
      faithfulness: 0.7192
      answer_relevancy: 0.4186
      context_recall: 0.9800
      context_precision: 0.9100

=================================================================
  Metric                                V1        V2  Winner
=================================================================
  faithfulness                      0.9723    0.7192  ← V1
  answer_relevancy                  0.5929    0.4186  ← V1
  context_recall                    0.9800    0.9800  ← V2
  context_precision                 0.9100    0.9100  ← V2

✅ Đạt mục tiêu: faithfulness = 0.9723 ≥ 0.8
💾 Đã lưu báo cáo vào /home/aminix/Projects/K4-L3-Track2-Day22-LLMOps-Prompt-Versioning/data/ragas_report.json
```

## Evaluation Details
- **Total LLM calls**: 400 (200 per version × 4 metrics)
- **Evaluator LLM**: Same provider (OpenRouter/Omniroute) with temperature=0
- **Embeddings**: Local sentence-transformers/all-MiniLM-L6-v2
- **Time**: ~20 minutes total