# Step 2 Evidence: Prompt Hub & A/B Routing

## Prompt Hub URLs
- **V1 (Concise)**: https://smith.langchain.com/prompts/aminix-rag-v1/e622e095
- **V2 (Structured Expert)**: https://smith.langchain.com/prompts/aminix-rag-v2/509cfa30

## Prompt Versions

### V1 - Concise (aminix-rag-v1)
```text
Bạn là trợ lý AI hữu ích. Chỉ dùng context sau để trả lời.
Giữ câu trả lời ngắn gọn (2-4 câu). Trả lời trực tiếp và chính xác.

Context:
{context}
```

### V2 - Structured Expert (aminix-rag-v2)
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

## A/B Routing Logic
- **Method**: Deterministic MD5 hash of `request_id`
- **Even hash** → V1 (aminix-rag-v1)
- **Odd hash** → V2 (aminix-rag-v2)
- **Same request_id** → Always same version (deterministic)

## Terminal Output (Step 2 - 50 A/B Routed Questions)
```
============================================================
  Bước 2: Prompt Hub & A/B Routing
============================================================
✅ Config OK  |  Provider: OPENROUTER  |  Project: day22-lab
⚠️  V1 lỗi: Conflict for /commits/-/aminix-rag-v1. HTTPError('409 Client Error: Conflict for url: https://api.smith.langchain.com/commits/-/aminix-rag-v1', '{"error":"Nothing to commit: prompt has not changed since latest commit"}\n')
⚠️  V2 lỗi: Conflict for /commits/-/aminix-rag-v2. HTTPError('409 Client Error: Conflict for url: https://api.smith.langchain.com/commits/-/aminix-rag-v2', '{"error":"Nothing to commit: prompt has not changed since latest commit"}\n')
↓ Đã pull 'aminix-rag-v1' từ Hub
↓ Đã pull 'aminix-rag-v2' từ Hub
⚠️  Using local sentence-transformers embeddings (no API key required)
🔨 Đang tạo FAISS index từ 107 chunks ...
✅ FAISS vectorstore đã sẵn sàng.
[01] [prompt-v2] What are the three main types of machine learning?...
[02] [prompt-v2] What is overfitting in machine learning?...
[03] [prompt-v1] Explain the bias-variance tradeoff....
[04] [prompt-v1] How does regularization prevent overfitting?...
[05] [prompt-v2] What is cross-validation?...
[06] [prompt-v2] What is backpropagation?...
[07] [prompt-v2] What are Convolutional Neural Networks primarily used f...
[08] [prompt-v2] How do LSTM networks address the vanishing gradient pro...
[09] [prompt-v1] What activation functions are commonly used in neural n...
[10] [prompt-v2] What is the role of pooling layers in CNNs?...
[11] [prompt-v1] What is the transformer architecture?...
[12] [prompt-v1] What are word embeddings?...
[13] [prompt-v2] What is transfer learning in NLP?...
[14] [prompt-v1] How does BERT handle language understanding?...
...
[50] [prompt-v1] What are common AI safety concerns with LLMs?...

📊 Routing: V1=25 câu | V2=25 câu | Tổng=50
✅ Bước 2 hoàn thành! Kiểm tra Prompt Hub và traces trên LangSmith.
```

## How to Verify in LangSmith Dashboard
1. Go to https://smith.langchain.com
2. Select project: `day22-lab`
3. You should see 50 new traces named `ab-rag-query`
4. Each trace has tags: `ab-test`, `step2`
5. Check metadata for `version`: "v1" or "v2"
6. Verify Prompt Hub: https://smith.langchain.com/prompts → search "aminix-rag"