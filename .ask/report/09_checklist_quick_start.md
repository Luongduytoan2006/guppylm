# 10. Checklist quick-start

## Con đường ngắn nhất để chạy project

### Mục tiêu A: Chat nhanh với model có sẵn
1. Cài Python + pip
2. Cài deps tối thiểu (`torch`, `tokenizers`)
3. Chạy download pre-trained
4. Chạy chat

### Mục tiêu B: Train từ đầu
1. Cài deps theo `requirements.txt` (+ cân nhắc thêm `huggingface_hub` nếu cần export)
2. Chạy `prepare`
3. Chạy `train`
4. Chạy `chat`

## Cần cài gì trước

- Bắt buộc:
  - Python environment
  - PyTorch
  - Tokenizers
- Nếu train từ dataset HF/notebook:
  - `datasets`
- Nếu export/publish:
  - `huggingface_hub`
  - `onnx`, `onnxruntime`, `onnxscript` (cho ONNX export)

## File nào nên chỉnh trước (tuỳ mục tiêu)

1. `guppylm/config.py`
   - chỉnh `max_steps`, `batch_size`, LR, device, output dir
2. `.env` (tạo từ `.env.example`)
   - nếu cần push artifact lên HF
3. (tuỳ chọn) `guppylm/generate_data.py`
   - chỉnh persona/topic/data distribution

## Lỗi dễ gặp nhất

1. Thiếu file model/tokenizer khi chat
   - quên `download` hoặc chưa train xong
2. Thiếu package export
   - chưa cài `huggingface_hub`/onnx libs
3. Sai env HF khi push
   - thiếu `HF_TOKEN`, `HF_REPO`, `HF_DATASET`
4. Chạy browser demo nhưng thiếu `docs/model.onnx` hoặc `docs/tokenizer.json`
   - cần tải lại qua script hoặc export lại
