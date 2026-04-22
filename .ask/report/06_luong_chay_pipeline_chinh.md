# 7. Luồng chạy chính / pipeline chính

## Flow A — Prepare dữ liệu + tokenizer

1. **Điểm kích hoạt:** `python -m guppylm prepare`
2. **Entry:** `guppylm/__main__.py` -> `prepare_data.prepare()`
3. **Xử lý chính:**
   - `prepare_data.py` gọi `generate_data.generate_dataset()`
   - `generate_data.py` tạo sample synthetic theo ~60 topic, ghi `data/train.jsonl`, `data/eval.jsonl`, và biến thể OpenAI jsonl
   - quay lại `prepare_data.py` đọc text và train BPE tokenizer
   - lưu `data/tokenizer.json`
4. **Đầu ra:** file dữ liệu train/eval + tokenizer

---

## Flow B — Training mô hình

1. **Điểm kích hoạt:** `python -m guppylm train`
2. **Entry:** `guppylm/__main__.py` -> `train.train()`
3. **Xử lý chính:**
   - load config (`GuppyConfig`, `TrainConfig`)
   - tạo model `GuppyLM`
   - tạo train/eval dataloader từ JSONL + tokenizer
   - train loop với AdamW + cosine LR + grad clip + AMP (nếu CUDA)
   - evaluate định kỳ
   - lưu checkpoint
4. **Đầu ra:**
   - `checkpoints/best_model.pt`
   - `checkpoints/final_model.pt`
   - `checkpoints/step_*.pt`
   - `checkpoints/config.json`

---

## Flow C — Inference local (CLI)

### C1. Tải pre-trained model
1. **Trigger:** `python -m guppylm download`
2. `__main__.py` tải từ Hugging Face:
   - `pytorch_model.bin` -> `checkpoints/best_model.pt`
   - `tokenizer.json` -> `data/tokenizer.json`
   - `config.json` -> `checkpoints/config.json`
3. **Output:** đủ artifact để chat

### C2. Chat
1. **Trigger:** `python -m guppylm chat` hoặc `python -m guppylm chat --prompt "..."`
2. **Entry:** `inference.py::main()`
3. **Xử lý:**
   - load tokenizer + checkpoint + config
   - format prompt theo ChatML
   - generate token autoregressive (`model.generate`)
   - cắt tại `<|im_end|>`
4. **Output:** trả lời text của Guppy trên terminal

---

## Flow D — Browser inference (không backend)

1. **Trigger:** mở `docs/index.html` qua static server
2. **Thành phần tham gia:**
   - `docs/index.html`
   - `docs/model.onnx`
   - `docs/tokenizer.json`
   - CDN `onnxruntime-web`
3. **Xử lý:**
   - JS load tokenizer + ONNX model
   - encode prompt
   - chạy autoregressive generation trong browser (WASM)
4. **Output:** hội thoại trực tiếp trên trang web, không cần API server

---

## Flow E — Export/Publish artifact lên HF

1. **Trigger:** chạy `tools/export_model.py`, `tools/export_onnx.py`, `tools/export_dataset.py`
2. **Yêu cầu:** env `HF_TOKEN` + repo id phù hợp
3. **Xử lý:** convert/generate/upload model hoặc dataset
4. **Output:** model/dataset repo trên Hugging Face
