# 4. Environment variables và config

## File config quan trọng

1. `guppylm/config.py`
   - `GuppyConfig`: cấu hình model
   - `TrainConfig`: cấu hình train (batch size, LR, step, data/output dir, device...)

2. `.env.example`
   - `HF_TOKEN`
   - `HF_REPO`
   - `HF_DATASET`

3. `checkpoints/config.json`
   - Được tạo khi train (`train.py`)
   - Dùng cho inference/export để reconstruct config

## Biến môi trường phát hiện được

### 1) `HF_TOKEN`
- **Dùng ở:** `tools/export_model.py`, `tools/export_onnx.py`, `tools/export_dataset.py`, notebook export cell
- **Mục đích:** xác thực để push model/dataset lên Hugging Face
- **Mức độ bắt buộc:**
  - **Bắt buộc** nếu muốn push
  - **Không bắt buộc** nếu chỉ local export/chat/train

### 2) `HF_REPO`
- **Dùng ở:** `tools/export_model.py`, `tools/export_onnx.py`
- **Mục đích:** repo model trên HF (ví dụ `username/guppylm-9M`)
- **Mức độ bắt buộc:** bắt buộc khi push model

### 3) `HF_DATASET`
- **Dùng ở:** `tools/export_dataset.py`
- **Mục đích:** repo dataset trên HF
- **Mức độ bắt buộc:** bắt buộc khi push dataset

## Config không qua env nhưng quan trọng

- Đường dẫn mặc định:
  - checkpoint: `checkpoints/best_model.pt`
  - tokenizer: `data/tokenizer.json`
  - data dir: `data/`
- Hyperparameters train mặc định cố định trong `TrainConfig`

## Nhận xét

- Repo có `.env.example` và `.gitignore` đã ignore `.env` (ổn về bảo mật cơ bản).
- Không thấy API key bên thứ ba khác (OpenAI, Anthropic, DB_URL, Redis URL...).
