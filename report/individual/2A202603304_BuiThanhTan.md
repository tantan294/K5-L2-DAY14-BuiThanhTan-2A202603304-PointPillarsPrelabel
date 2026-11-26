# Báo cáo Day 14 — Bùi Thanh Tân — 2A202603304

- Mã học viên dùng với `--student`: `2A202603304`
- `seed_tag` trong `frames_A.txt`: `82ac3100fd`

## 1. Dự đoán trước sweep (bước 5b)

Ghi **trước** khi chạy `scripts/pp_sweep.py`, rồi commit và push ngay (lịch sử commit là bằng chứng thời điểm viết). Không sửa phần này sau khi có kết quả.

- Tham số tôi nghĩ giảm công sửa nhiều nhất: **`score_thr`** (tăng ngưỡng điểm, khoảng 0.3–0.4). Tham số thứ hai là `min_points` (bỏ box có quá ít điểm LiDAR).
- Vì sao (nhìn ảnh BEV tập A: box thừa nhiều hay box sót nhiều?): **Box thừa nhiều hơn box sót rất nhiều.** Trên ảnh BEV và ảnh camera của cả 3 frame, phần lớn box đỏ (Car) nằm trên hàng rào cây/bụi cỏ ven đường, còn box xanh lá (Cyclist) nằm trên cột đèn, biển báo và lan can. Đo cấu hình gốc so với `ref_A.json`: dự đoán 98 box nhưng GT chỉ có 42; khớp 40, **xóa 58, thêm 2, sửa 3**, chi phí 20.9. Trong đó riêng phần xóa đã chiếm khoảng 58 × 0.3 ≈ 17.4 điểm (≈ 83%). Các box thừa này thường có điểm thấp và ít điểm LiDAR, nên tăng `score_thr` sẽ loại được nhiều box thừa. Box sót chỉ có 2 cái, nên nâng ngưỡng có rủi ro làm sót thêm vài box thật (mỗi box thêm tốn 1 điểm). Vì vậy tôi đoán ngưỡng tối ưu ở mức vừa phải, khoảng 0.3–0.4, chứ không phải mức cao nhất.

## 2. Tập A: sửa tay và tune

| Cấu hình | Xóa | Thêm | Sửa | Chi phí |
|---|---:|---:|---:|---:|
| Gốc | | | | |
| `best_config.json` | | | | |

- Tham số đã đổi trong `best_config.json`: ...
- Kết quả leave-one-out (dán các dòng `scripts/pp_sweep.py` in ra): ...
- Cấu hình có ổn định trên từng frame không? Vì sao tôi vẫn chọn (hoặc không chọn) nó: ...

## 3. Trước/sau (bước 8)

![so sánh](../../d14_<ID>/compare/<frame>.jpg)

- `scripts/pp_compare.py` báo giảm (hoặc tăng) ...% công sửa trên tập A.
- Dự đoán ở mục 1 đúng hay sai? Giải thích bằng số box xóa/thêm/sửa: ...

## 4. Tập B

Số đo từ `scripts/pp_prelabel.py --frames d14_<ID>/frames_B.txt --config d14_<ID>/best_config.json --gt d14_<ID>/ref_B.json`:

| Xóa | Thêm | Sửa | Chi phí |
|---:|---:|---:|---:|
| | | | |

- Loại box tốn công sửa nhất ở tập B: ...

## 5. Giới hạn

- Nêu ít nhất 2 giới hạn (ví dụ: chỉ 3 frame tune; model KITTI không có Truck/Bus; chi phí là công thức, chưa phải thời gian sửa thật): ...
