# 5. Ports, services, database, external dependencies

## Ports

- Không có backend server/API app mở port cố định.
- Browser demo chạy dạng static hosting; script `docs/download.sh` gợi ý dùng `python -m http.server 8080` để serve local => port thường dùng là **8080** (tuỳ người chạy).

## Services nội bộ

- Không có service tách rời (không có app service + db service kiểu microservices).
- Không có queue consumer, scheduler service, hay worker process độc lập.

## Database / cache / MQ

- **Không có** PostgreSQL/MySQL/MongoDB/Redis/RabbitMQ/Kafka trong codebase.

## External dependencies quan trọng

1. **Hugging Face Hub / Datasets**
   - Download pre-trained model (`python -m guppylm download`)
   - Download dataset trong notebook
   - Push model/dataset qua tool scripts

2. **ONNX Runtime Web CDN**
   - `docs/index.html` import `onnxruntime-web` từ jsDelivr

3. **Model/tokenizer artifact local**
   - `docs/model.onnx`, `docs/tokenizer.json` (browser)
   - `checkpoints/best_model.pt`, `data/tokenizer.json` (python inference)

4. **System tools phụ (tuỳ flow)**
   - `curl` + bash cho `docs/download.sh`
   - môi trường có GPU (CUDA) giúp train nhanh nhưng không bắt buộc

## OS/Runtime yêu cầu suy ra từ code

- Python runtime để chạy CLI/tools/notebooks
- Linux/macOS thuận tiện cho `download.sh`; Windows có thể dùng Git Bash/WSL hoặc tải file thủ công.
- Không yêu cầu Java, .NET, Node runtime để chạy core Python pipeline (Node chỉ gián tiếp qua browser runtime CDN, không cần cài local cho demo tĩnh).
