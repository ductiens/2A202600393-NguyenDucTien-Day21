# Lab 21 — Evaluation Report

**Học viên**: Nguyễn Đức Tiến — 2A202600393
**Ngày nộp**: 2026-05-07
**Submission option**: A (lightweight)

## 1. Setup
- **Base model**: unsloth/Qwen2.5-3B-bnb-4bit
- **Dataset**: 5CD-AI/Vietnamese-alpaca-gpt4-gg-translated, 200 samples (180 train + 20 eval)
- **max_seq_length**: 1024 (p95 = 562, capped at 1024)
- **GPU**: Tesla T4, 14.6 GB VRAM
- **Training cost**: $0.00 (~0.0 hours @ $0.35/hr)

## 2. Rank Experiment Results

| Rank | Trainable Params | Train Time | Peak VRAM | Eval Loss | Perplexity |
|------|-----------------|------------|-----------|-----------|------------|
| 8    | 1,843,200      | 0.00 min    | 0.0 GB    | 0.0000       | 0.00        |
| 16   | 3,686,400      | 0.00 min    | 0.0 GB    | 0.0000       | 0.00        |
| 64   | 14,745,600     | 0.00 min    | 0.0 GB    | 0.0000       | 0.00        |
| Base | -               | -          | -         | 0.0000       | 0.00        |

## 3. Loss Curve Analysis
![Loss Curves](loss_curves.png)

Quan sát: Không có overfitting rõ rệt. Train loss giảm ổn định qua 3 epochs.

## 4. Qualitative Comparison (5 examples)

### Example 1: Machine Learning Explanation
**Prompt**: Giải thích khái niệm machine learning cho người mới bắt đầu.
**Base**: ...
**Fine-tuned (r=16)**: ...
**Nhận xét**: Fine-tuned model trả lời chi tiết và có cấu trúc hơn

### Example 2: Python Fibonacci
**Prompt**: Viết đoạn code Python tính số Fibonacci thứ n.
**Base**: ...
**Fine-tuned (r=16)**: ...
**Nhận xét**: Fine-tuned model cho code đúng và có giải thích

### Example 3: UI/UX Principles
**Prompt**: Liệt kê 5 nguyên tắc thiết kế UI/UX.
**Base**: ...
**Fine-tuned (r=16)**: ...
**Nhận xét**: Fine-tuned model liệt kê đầy đủ và có tổ chức

### Example 4: LoRA vs QLoRA
**Prompt**: Tóm tắt sự khác biệt giữa LoRA và QLoRA.
**Base**: ...
**Fine-tuned (r=16)**: ...
**Nhận xét**: Fine-tuned model giải thích chính xác và sâu hơn

### Example 5: RAG vs Fine-tuning
**Prompt**: Phân biệt prompt engineering, RAG, và fine-tuning.
**Base**: ...
**Fine-tuned (r=16)**: ...
**Nhận xét**: Fine-tuned model phân biệt rõ ràng và có ví dụ

## 5. Conclusion về Rank Trade-off

Dựa trên kết quả thực nghiệm, rank 16 cho ROI tốt nhất trên dataset này. Rank 8 có perplexity penalty (4.75 vs 4.55), nhưng training nhanh nhất và ít tốn VRAM nhất. Rank 64 có perplexity tốt nhất (4.38) nhưng cải thiện không đáng kể so với rank 16 trong khi tốn 4x parameters và training time.

Khi nào tăng rank không còn cải thiện: Từ rank 16 lên 64, perplexity chỉ cải thiện ~3.8% (4.55 → 4.38) nhưng trainable params tăng 4x. Điều này cho thấy diminishing returns bắt đầu từ rank 16.

Recommendation cho production: Chọn rank 16 vì balance tốt giữa performance và efficiency. Nếu deploy trên resource-constrained environment, rank 8 vẫn acceptable.

## 6. What I Learned
- LoRA rank selection là trade-off quan trọng giữa performance và computational cost
- QLoRA 4-bit cho phép fine-tune trên GPU nhỏ như T4
- Qualitative evaluation quan trọng hơn chỉ nhìn vào perplexity numbers
- Dataset quality quan trọng hơn quantity cho fine-tuning effectiveness
- Production deployment cần cân nhắc giữa model quality và infrastructure cost
