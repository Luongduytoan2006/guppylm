# 8. Những file nên đọc đầu tiên (ưu tiên)

## Top file ưu tiên (12 file)

1. `README.md`
   - Nắm mục tiêu, quickstart, architecture khái quát, links demo/HF.

2. `guppylm/__main__.py`
   - Entrypoint thực tế của CLI, biết ngay các command chính.

3. `guppylm/config.py`
   - Tất cả hyperparameter model/train mặc định.

4. `guppylm/model.py`
   - Trái tim model transformer + logic generate.

5. `guppylm/train.py`
   - Luồng train thật (optimizer, LR schedule, eval, checkpoint).

6. `guppylm/dataset.py`
   - Cách dữ liệu JSONL được tokenize/pad/batch vào model.

7. `guppylm/inference.py`
   - Cách load model/tokenizer và tạo response.

8. `guppylm/prepare_data.py`
   - Điều phối tạo data và train tokenizer.

9. `guppylm/generate_data.py`
   - Logic tạo synthetic dataset, rất quan trọng để hiểu chất lượng đầu vào.

10. `tools/export_onnx.py`
    - Cầu nối từ checkpoint PyTorch sang model web ONNX quantized.

11. `docs/index.html`
    - Toàn bộ browser runtime (WASM inference, tokenizer JS, UI chat).

12. `.env.example`
    - Nhanh chóng biết biến môi trường nào cần cho publish/export.

## Nếu cần đọc thêm ngay sau đó
- `tools/export_model.py` (đóng gói chuẩn HF model repo)
- `tools/export_dataset.py` (publish dataset)
- `guppylm/eval_cases.py` (bộ case sanity)
- `requirements.txt` (deps thực tế)
