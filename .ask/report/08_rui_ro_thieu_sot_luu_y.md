# 9. Rủi ro / thiếu sót / chỗ cần lưu ý

## 1) Không có Docker/Compose/CI
- Không thấy `Dockerfile`, `docker-compose`, `.github/workflows`.
- Hệ quả: khó chuẩn hóa môi trường và tự động hoá build/test/deploy.

## 2) Test tự động còn thiếu
- Không có test suite chuẩn.
- `eval_cases.py` chỉ là danh sách case, chưa có runner assert tự động.

## 3) Dependency chưa đồng bộ hoàn toàn
- `requirements.txt` không liệt kê hết deps dùng trong tools/notebook (`huggingface_hub`, `onnx`, `onnxruntime`, `onnxscript`).
- Dễ gặp lỗi “ModuleNotFoundError” khi chạy script export.

## 4) Lệch nhẹ giữa README / script / code runtime
- README mô tả một số hành vi có thể không khớp 100% với mã hiện tại.
- `docs/download.sh` in hướng dẫn `cd web && python -m http.server 8080` trong khi thư mục hiện tại là `docs/`.

## 5) Rủi ro parse config trong inference
- `train.py` ghi `checkpoints/config.json` dạng lồng `{"model": ..., "train": ...}`.
- `inference.py` khi đọc `config.json` lại kỳ vọng key phẳng (`vocab_size`, `hidden_size`, ...), nên thực tế thường rơi về default.
- Hiện chưa gây lỗi vì default trùng config mặc định, nhưng có thể sai nếu training config tùy biến.

## 6) Artifact phụ thuộc bên ngoài
- Nhiều luồng quan trọng cần tải từ Hugging Face (model, dataset).
- Nếu không có mạng/HF lỗi rate limit thì workflow bị chặn.

## 7) `.ask/` bị ignore
- `.gitignore` đang ignore cả `.ask/` nên các report/ghi chú review không đi kèm commit (nếu bạn muốn lưu versioned thì cần điều chỉnh).

## 8) Không có pin phiên bản Python rõ ràng
- Không có `pyproject.toml` hay file quy định phiên bản Python cố định.
- Có thể phát sinh khác biệt môi trường giữa máy.
