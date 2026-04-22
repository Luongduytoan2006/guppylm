# Mức độ chắc chắn

- **Mức độ chắc chắn tổng thể:** **Cao (~90%)** cho kiến trúc, luồng chạy, entrypoint, config, dependency chính.
- **Trung bình (~70-80%)** cho vài điểm hành vi runtime phụ thuộc artifact ngoài (HF content hiện tại, browser runtime thực tế theo môi trường trình duyệt).

## Những gì đã xác nhận rõ từ code

1. Entrypoint CLI là `python -m guppylm` với 4 subcommand `prepare/train/chat/download`.
2. Pipeline train/data/inference được cài đặt đầy đủ trong package `guppylm/`.
3. Repo có browser demo thuần static (`docs/index.html`) chạy ONNX + WASM.
4. Không có Docker/Compose/CI/test framework.
5. Env vars có thật trong code: `HF_TOKEN`, `HF_REPO`, `HF_DATASET`.
6. Export scripts phụ thuộc Hugging Face và ONNX tooling.

## Những gì là suy luận / cần kiểm tra thêm

1. Hiệu năng thực tế (thời gian train/chat) trên máy cụ thể — chưa chạy thực nghiệm theo yêu cầu.
2. Mức tương thích chính xác theo từng OS/Python version — repo chưa pin rõ Python version.
3. Một số khác biệt README vs runtime hiện tại có thể do tiến hóa code; cần xác thực lại bởi tác giả nếu muốn kết luận “bug” chính thức.
4. Chất lượng output model trong các edge case cụ thể cần benchmark thực thi (chưa làm vì bạn yêu cầu chỉ đọc code).
