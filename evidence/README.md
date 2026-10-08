# Phân tích Prompt V1 vs V2

## Tóm tắt
**V1 (Prompt Ngắn Gọn) vượt trội hơn V2 (Prompt Chuyên Gia Có Cấu Trúc)** trên metric RAGAS quan trọng nhất (faithfulness), đạt **0.9723 ≥ 0.8** — đạt mục tiêu.

## So sánh chi tiết

| Metric | V1 (Ngắn Gọn) | V2 (Có Cấu Trúc) | Thắng | Chênh lệch |
|--------|-------------|----------------|--------|------------|
| **faithfulness** | **0.9723** | 0.7192 | **V1** | +0.2531 |
| answer_relevancy | 0.5929 | 0.4186 | V1 | +0.1743 |
| context_recall | 0.9800 | 0.9800 | Hòa | 0 |
| context_precision | 0.9100 | 0.9100 | Hòa | 0 |

## Tại sao V1 thắng

### 1. Faithfulness (Mục tiêu chính: ≥ 0.8)
- **V1**: 0.9723 ✅ **VƯỢT MỤC TIÊU 21%**
- **V2**: 0.7192 ❌ **DƯỚI MỤC TIÊU 10%**

**Nguyên nhân gốc rễ**: V2 ép buộc cấu trúc 4 phần (tóm tắt, cơ chế, ví dụ, kết luận) khiến mô hình **hallucinate (giả tạo) nội dung hợp lý** cho các phần không được hỗ trợ đầy đủ bởi context truy xuất. V1 giới hạn 2-4 câu buộc mô hình chỉ nêu những gì có rõ ràng trong context.

### 2. Answer Relevancy (Độ liên quan câu trả lời)
- **V1**: 0.5929 - Trả lời trực tiếp, tập trung
- **V2**: 0.4186 - Bị loãng bởi nội dung định dạng cấu trúc

**Nguyên nhân gốc rễ**: V2 tiêu ngân sách token cho việc định dạng cấu trúc thay vì trả lời trực tiếp câu hỏi.

### 3. Các metric Context (Hòa nhau)
Cả hai prompt dùng retriever giống hệt nhau (k=3 từ cùng FAISS index), nên context recall/precision giống nhau.

## Thiết kế Prompt

### V1 - Ngắn Gọn (Thắng)
```text
Bạn là trợ lý AI hữu ích. Chỉ dùng context sau để trả lời.
Giữ câu trả lời ngắn gọn (2-4 câu). Trả lời trực tiếp và chính xác.

Context:
{context}
```

### V2 - Chuyên Gia Có Cấu Trúc (Thua)
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

## Insight Quan Trọng

> **Trong hệ thống RAG, prompt đơn giản có ràng buộc thường vượt trội hơn prompt phức tạp có cấu trúc** vì chúng giảm xu hướng hallucinate của mô hình vượt khỏi context được cung cấp.

Cấu trúc "chuyên gia" của V2 nghe hay cho người đọc nhưng **làm giảm metric faithfulness** vì mô hình lấp đầy khoảng trống cấu trúc bằng nội dung nghe hợp lý nhưng không có bằng chứng.

## Khuyến nghị
**Dùng V1 (Ngắn Gọn) cho RAG production** khi độ chính xác sự thật (faithfulness) là quan trọng. Chỉ cân nhắc V2 cho tóm tắt dành cho người đọc khi dễ đọc quan trọng hơn sự tuân thủ sự thật chặt chẽ.

## Các file bằng chứng
- `evidence/03_ragas_report.json` - Kết quả số đầy đủ
- `evidence/step3_ragas_evaluation.md` - Output terminal chi tiết
- `evidence/step1_langsmith_traces.md` - Bằng chứng trace Bước 1
- `evidence/step2_prompt_hub_ab_routing.md` - Bằng chứng Prompt Hub & A/B routing Bước 2
- `evidence/step4_pii_demo_log.txt` - Test cases phát hiện PII
- `evidence/step4_json_demo_log.txt` - Test cases định dạng JSON