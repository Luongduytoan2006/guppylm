# 6. Giải thích cấu trúc folder

## Root level

- `README.md`: tài liệu tổng quan + quickstart + mô tả design.
- `requirements.txt`: dependency Python core.
- `Makefile`: target tiện ích để regenerate notebook.
- `train_guppylm.ipynb`, `use_guppylm.ipynb`: notebook train/chat phục vụ Colab.
- `.env.example`: mẫu biến môi trường cho export/publish.

## `guppylm/` — business logic cốt lõi

- `__main__.py`: **CLI entrypoint chính** (`prepare/train/chat/download`).
- `config.py`: cấu hình model/train.
- `model.py`: kiến trúc transformer và generate.
- `dataset.py`: đọc JSONL + tokenize + dataloader.
- `generate_data.py`: generator dữ liệu synthetic theo topic/persona.
- `prepare_data.py`: orchestration tạo dataset + train tokenizer.
- `train.py`: training loop, eval, checkpointing.
- `inference.py`: load checkpoint/tokenizer và sinh response.
- `eval_cases.py`: bộ case kiểm tra thủ công.

## `tools/` — script vận hành/phân phối

- `make_colab.py`: sinh lại notebook từ source code.
- `export_model.py`: xuất model về format chuẩn HF + optional push.
- `export_onnx.py`: export ONNX + quantize + optional push.
- `export_dataset.py`: generate/export dataset lên HF.
- `dataset_card.md`, `model_card.md`: metadata README cho HF artifacts.

## `docs/` — frontend demo + artifact web

- `index.html`: demo chat chạy trực tiếp trong browser bằng ONNX Runtime Web.
- `model.onnx`: model quantized phục vụ demo web.
- `tokenizer.json`: tokenizer cho demo web.
- `download.sh`: script tải `model.onnx` và tokenizer từ HF.

## `assets/`

- chứa hình/logo (`guppy.png`) dùng cho README/docs/model card.

## `.ask/`

- thư mục làm việc nội bộ cho prompt/review/report; hiện `report/` là nơi chứa bản phân tích.
