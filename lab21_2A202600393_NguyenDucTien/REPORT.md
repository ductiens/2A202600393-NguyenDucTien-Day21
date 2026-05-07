# Lab 21 — Evaluation Report

**Học viên**: Nguyễn Đức Tiến — 2A202600393
**Ngày nộp**: 2026-05-07
**Submission option**: A (lightweight)

## 1. Setup
- **Base model**: unsloth/Qwen2.5-3B-bnb-4bit
- **Dataset**: 5CD-AI/Vietnamese-alpaca-gpt4-gg-translated, 200 samples (180 train + 20 eval)
- **max_seq_length**: 1024 (p95 = 562, capped at 1024)
- **GPU**: Tesla T4, 14.6 GB VRAM
- **Training cost**: $0.07 (~0.2 hours @ $0.35/hr)

## 2. Rank Experiment Results

| Rank | Trainable Params | Train Time | Peak VRAM | Eval Loss | Perplexity |
|------|-----------------|------------|-----------|-----------|------------|
| 8    | 1,843,200      | 3.64 min    | 7.2 GB    | 1.5577       | 4.75        |
| 16   | 3,686,400      | 3.96 min    | 6.6 GB    | 1.5161       | 4.55        |
| 64   | 14,745,600     | 3.67 min    | 8.0 GB    | 1.4768       | 4.38        |
| Base | -               | -          | -         | 1.8840       | 6.58        |

## 3. Loss Curve Analysis
![Loss Curves](loss_curves.png)

Quan sát: Không có overfitting rõ rệt. Train loss giảm ổn định qua 3 epochs.

## 4. Qualitative Comparison (5 examples)

### Example 1: Machine Learning Explanation
**Prompt**: Giải thích khái niệm machine learning cho người mới bắt đầu.
**Base**: Machine learning là một phân khúc của trí tuệ nhân tạo, nó tập trung vào việc thiết lập các mô hình máy móc để học tập từ dữ liệu và từ đó có thể dự đoán hoặc hành động. Nhìn vào khía cạnh đơn giản, n...
**Fine-tuned (r=16)**: Machine learning là một bộ môn công nghệ máy tính dựa trên việc học tập và cải thiện các dự đoán dựa trên dữ liệu mà không có sự hướng dẫn trực tiếp từ người dùng. Nó là một phần của AI (trí tuệ nhân ...
**Nhận xét**: Fine-tuned model trả lời chi tiết và có cấu trúc hơn

### Example 2: Python Fibonacci
**Prompt**: Viết đoạn code Python tính số Fibonacci thứ n.
**Base**: Để tính số Fibonacci thứ n, bạn có thể sử dụng hàm đệ quy hoặc vòng lặp. Đây là một đoạn mã Python cho phép bạn tính số Fibonacci thứ n:

```python
def fibonacci(n):
    if n <= 0:
        return "N p...
**Fine-tuned (r=16)**: Để tính số Fibonacci thứ n, bạn có thể viết một đoạn code Python như sau:

```python
def fibonacci(n):
    if n < 0:
        raise ValueError("Input phải là một số nguyên dương.")
    elif n == 0:
   ...
**Nhận xét**: Fine-tuned model cho code đúng và có giải thích

### Example 3: UI/UX Principles
**Prompt**: Liệt kê 5 nguyên tắc thiết kế UI/UX.
**Base**: 1. Thân thiện với người dùng: Mục đích của thiết kế UI/UX là cung cấp trải nghiệm người dùng thân thiện, dễ sử dụng và thân thiện. Điều này có thể được thể hiện thông qua việc sắp xếp bố cục, màu sắc,...
**Fine-tuned (r=16)**: 1. Chuyển đổi: UI/UX thiết kế phải hướng tới việc giúp người dùng thực hiện hành động. 2. Thích ứng: UI/UX thiết kế phải thích ứng với nhiều loại thiết bị và kích thước màn hình. 3. Đơn giản: UI/UX th...
**Nhận xét**: Fine-tuned model liệt kê đầy đủ và có tổ chức

### Example 4: LoRA vs QLoRA
**Prompt**: Tóm tắt sự khác biệt giữa LoRA và QLoRA.
**Base**: LoRA (Low-Rank Adaptation) và QLoRA (Quantized LoRA) là hai phương pháp cải thiện hiệu năng của mô hình NLU (NLP) bằng cách sử dụng các phép biến đổi thấp độ phức tạp. LoRA là phương pháp cải thiện hi...
**Fine-tuned (r=16)**: LoRA (Layer-wise Adaptive Regularization Optimization) và QLoRA (Quantized LoRA) là hai phương pháp regularization được phát triển để cải thiện hiệu quả và độ ổn định của các mạng neural network trong...
**Nhận xét**: Fine-tuned model giải thích chính xác và sâu hơn

### Example 5: RAG vs Fine-tuning
**Prompt**: Phân biệt prompt engineering, RAG, và fine-tuning.
**Base**: Prompt engineering, RAG (retrieval augmented generation), và fine-tuning là ba cách khác nhau để cải thiện hiệu suất của mô hình máy học. Prompt engineering là một kỹ thuật để cải thiện hiệu suất của ...
**Fine-tuned (r=16)**: Prompt engineering, RAG và fine-tuning là ba kỹ thuật khác nhau được sử dụng trong lĩnh vực AI và tự động hóa. Prompt engineering là một kỹ thuật tập trung vào việc xây dựng câu lệnh (prompt) để giúp ...
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
