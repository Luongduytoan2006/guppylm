# 1. Tổng quan dự án

- **Mục đích:** Đây là repo huấn luyện và suy luận một mô hình ngôn ngữ nhỏ (~8.7M tham số) đóng vai cá Guppy.
- **Bài toán giải quyết:** demo end-to-end quy trình LLM “tự làm từ đầu” ở mức nhỏ: tạo dữ liệu hội thoại synthetic -> train tokenizer -> train transformer -> inference local/browser.
- **Loại ứng dụng:**
  - **Python package + CLI** (`python -m guppylm ...`)
  - **AI training/inference pipeline**
  - **Browser demo tĩnh** (ONNX + WASM trong `docs/index.html`)
  - **Tooling scripts** để export model/dataset lên Hugging Face
- **Không phải:** web backend/API server production, microservice, hay app có database.

## Khả năng chính đọc được từ code

1. **Chuẩn bị dữ liệu/train tokenizer**: `guppylm/prepare_data.py`, `guppylm/generate_data.py`
2. **Train mô hình**: `guppylm/train.py`
3. **Chat local**: `guppylm/inference.py`
4. **Download pre-trained từ Hugging Face**: `guppylm/__main__.py` (lệnh `download`)
5. **Demo browser**: `docs/index.html` dùng `docs/model.onnx` + `docs/tokenizer.json`
6. **Export model/dataset**: `tools/export_model.py`, `tools/export_onnx.py`, `tools/export_dataset.py`

---

# 2. Tech stack

## Ngôn ngữ & framework
- **Python** (core project)
- **PyTorch** (model + training)
- **Hugging Face datasets + hub** (download/push dataset/model)
- **tokenizers (HF)** để train và load BPE tokenizer
- **ONNX / ONNX Runtime** (export model và chạy web)
- **HTML + JavaScript (ES Module)** cho browser demo

## Dependency chính (xác nhận từ file)
- `requirements.txt`:
  - `torch>=2.0.0`
  - `tokenizers>=0.19.0`
  - `tqdm>=4.65.0`
  - `numpy>=1.24.0`
  - `datasets>=2.14.0`
- Dependency **phát sinh theo tool/notebook** (không nằm hết trong `requirements.txt`):
  - `huggingface_hub` (export/push)
  - `onnx`, `onnxruntime`, `onnxscript` (export ONNX/quantize)
  - browser dùng CDN `onnxruntime-web`

## Kiến trúc mô hình (xác nhận từ `config.py` + `model.py`)
- Transformer decoder-only đơn giản
- 6 layers, d_model 384, 6 heads, FFN 768, vocab 4096, max seq 128
- Learned positional embeddings, LayerNorm, ReLU FFN
- LM head tie weight với embedding

## Build/tooling
- `Makefile` chỉ có target tạo notebook: `python3 tools/make_colab.py`
- Không thấy `poetry`, `uv`, `pipenv`, `npm`, `pnpm`, `yarn`, `maven`, `gradle`, `dotnet`.
