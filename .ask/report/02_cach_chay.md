# 3. Cách chạy

## 3.1 Local

### Ngôn ngữ / package manager
- **Python + pip** (suy ra từ `requirements.txt`, README, notebook cells `pip install ...`)

### Entry point CLI chính
- `python -m guppylm`
- Subcommands (xác nhận từ `guppylm/__main__.py`):
  - `prepare`: generate data + train tokenizer
  - `train`: train model
  - `chat`: chat với model local
  - `download`: tải model pre-trained từ HF

### Luồng local khả dụng
1. **Dùng pre-trained nhanh nhất**
   - Cài deps tối thiểu
   - `python -m guppylm download`
   - `python -m guppylm chat`
2. **Tự train từ đầu**
   - `python -m guppylm prepare`
   - `python -m guppylm train`
   - `python -m guppylm chat`

### Cách chạy qua notebook
- `train_guppylm.ipynb`: full pipeline train + test + export
- `use_guppylm.ipynb`: tải pretrain từ HF và chat
- Có script regenerate notebook: `make notebook` (chạy `tools/make_colab.py`)

---

## 3.2 Docker

- **Không tìm thấy `Dockerfile`**
- **Không tìm thấy `docker-compose.yml`/`compose.yml`**
- => Repo hiện tại **không có luồng Docker chính thức**.

---

## 3.3 Test

- **Không có thư mục/unit test chuẩn** (`tests/`, `test_*.py` không thấy)
- Có “evaluation cases” dạng dữ liệu kiểm thử thủ công tại `guppylm/eval_cases.py`.
- Notebook có cell “quick test” theo prompt mẫu, nhưng không phải test framework tự động.

---

## Ghi chú mâu thuẫn/khác biệt README vs code

- README có mô tả interactive chat bị dài context 128 tokens.
- `guppylm/inference.py` hiện tại (repo local) trong vòng lặp chat CLI **không lưu lịch sử message**, mỗi lượt về bản chất single-turn.
- Notebook sinh bởi `tools/make_colab.py` từng có biến thể multi-turn trong cell `inference.py` viết ra notebook.
- => Có dấu hiệu README/notebook và code runtime local đã thay đổi theo thời gian.
