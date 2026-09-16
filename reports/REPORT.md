# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Trần Đăng Ka Song   Nhóm: 45   Ngày: 16/9/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 27 |
| v=2 / v=1 / v=0 | 311 / 121 / 27 |
| Thời gian trung bình mỗi ảnh | 8phut |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. left_ear (18/27 ~ 66.6%)
2. right_ear (14/27 ~ 51.8%)
3. left_wrist / right_wrist (9/27 ~ 33.3%)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Đúng, đây là những khớp thường xuyên bị che khuất và khó xác định vị trí. Khớp tai (`ear`) hay bị tóc, mũ bảo hiểm hoặc góc nghiêng che khuất. Khớp cổ tay (`wrist`) thường bị che bởi vật cầm nắm, túi xách, hoặc bị thân người che lấp ở các góc chụp hẹp. Việc xác định vị trí giải phẫu khi chúng bị che là khá khó khăn do thiếu điểm tựa thị giác rõ ràng.

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.9219 | 0.922 |
| OKS@0.50 | 0.931 |0.931 |
| OKS@0.75 | 0.8966 | 0.897|
| Lỗi `dao_trai_phai` | 0 | 0 |
| Lỗi `nham_nguoi` | 1 | 0 |
| Lỗi `xoa_khop_bi_che` | 0 | 0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):
Ảnh gốc `train_13.jpg`: 2 người
- Người #0 (trái, áo xanh dương): cả thân dưới và tay phải đều bị che khuất hoàn toàn bởi người #1 và xe máy. Model gán thiếu người.
- Người #1 (phải, áo đen): chỉ thấy phần đầu, ngực và tay trái. Model cố gắng dự đoán cả phần dưới và gán nhầm khớp. Sau khi xóa người #1, người #0 còn 10/17 khớp.
-> Sửa: xóa người #1 → người còn 10 khớp.
Ảnh gốc `train_15.jpg`: 3 người
- Người bị che nhiều nhất (áo đen, góc trên trái) chỉ thấy một phần nhỏ phần thân trên.
-> Sửa: giữ nguyên (model đã làm đúng)

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

- `train_13.jpg` – Người #1 – Xóa người (toàn bộ khớp bị che khuất, model thiếu người).
- `train_13.jpg` – Người #0 – Giữ lại (10/17 khớp còn lại sau khi xóa người #1).
- `train_15.jpg` – Không sửa (model đã dự đoán đúng, không cần thay đổi).

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh; do không có lỗi, không có ảnh nào dễ hay khó liên quan tới loại lỗi này.

## 3. Kiểm chéo

Bạn cùng nhóm: ______

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| | | | | |
| | | | | |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->

-

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
pose_mAP50      |    0.8450    |      0.8450  |  +0.0000
pose_mAP50_95   |    0.6853     |     0.6908  |  +0.0055
pose_precision   |   0.9734     |     0.9792  | +0.0058
pose_recall      |   0.8462      |    0.8462  | +0.0000
box_mAP50        |   0.9785     |     0.9600  | -0.0185
box_mAP50_95     |   0.8119       |   0.8041  | -0.0078

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. **Thay đổi `pose_mAP50-95`**: từ **0.6853** (trước fine‑tune) lên **0.6908** (sau fine‑tune) → **+0.0055** (tăng 0.55 %).
   - Nếu nó giảm, nghĩa là các 20 ảnh của bạn đã cung cấp cho model các trường hợp **đặc biệt khó** mà dữ liệu COCO chưa có (ví dụ: độ che khuất (occlusion) mạnh, góc nhìn cực đoan, hoặc các khớp bị che hoàn toàn). Khi mô hình học những trường hợp này mà không đủ kiến thức chuẩn, nó có thể **học được các mẫu sai** (hallucination) và làm suy giảm khả năng tổng quát hoá trên dữ liệu chuẩn.
2. **Khoảng cách `box_mAP` – `pose_mAP`**: 
   - `box_mAP50_95` (sau fine‑tune) = **0.8041**
   - `pose_mAP50_95` (sau fine‑tune) = **0.6908**
   - **Hiệu số ≈ 0.1133 (11.3 %)**.
   - Điều này cho thấy model **dễ dàng phát hiện người (bounding box)** hơn **tìm vị trí chi tiết các khớp**.  Bounding‑box là nhiệm vụ **cô rơng**, yêu cầu chỉ xác định vùng chung, trong khi pose estimation đòi hỏi **độ chính xác cao** cho 17 keypoint, nhạy cảm với occlusion và góc chụp.
3. **Ví dụ lỗi mô hình**:  Ảnh **`train_04.jpg`**, khớp **`left_wrist`** của người #1 bị gán nhầm vào người khác → **lỗi “nhầm người”** (`nham_nguoi`). Đây là một trong các 4 loại lỗi được liệt kê trong slide‑43.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?

Ảnh có OKS thấp nhất giữa nhãn của tôi và model là `train_15` (OKS = 0.528). Dựa vào quan sát trên ảnh gốc:
- **Nguyên nhân bất đồng:** Ảnh có hai người chịu hiệu ứng che khuất (occlusion) rất nặng. Người mặc áo đen cúi người phía sau xe máy bị che mất gần như toàn bộ phần thân dưới và tay; người đứng bên phải nhìn nghiêng nên bị khuất một nửa người.
- **Ai đúng:** Khả năng cao cả model và người gán nhãn đều gặp khó khăn. Model thường cố đoán (hallucinate) các khớp bị che hoàn toàn (đáng lẽ phải gán `v=0`) hoặc dự đoán sai lệch vị trí các khớp `v=1`. Cần dùng `visualize_pose.py` overlay cả 2 kết quả để soi chi tiết từng khớp mới khẳng định được bên nào đúng.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?

Có, ảnh tôi gán tệ nhất (có OKS so với gold thấp nhất là 0.616) cũng chính là ảnh model đoán tệ nhất so với nhãn của tôi (`train_15`, OKS = 0.528). Điều này cho thấy đây là một bức ảnh cực kỳ khó (outlier) với độ che khuất (occlusion) rất cao. Khi các khớp bị che lấp quá nhiều bởi vật thể (xe máy) hoặc do góc nhìn, cả con người (người gán nhãn, gold) và model đều không có đủ căn cứ thị giác để xác định chính xác vị trí giải phẫu, dẫn đến độ sai lệch và bất đồng rất lớn.

## 5. Một rule evidence bạn đã dùng

Trong ảnh `train_13.jpg` (người áo xanh dương bên trái), tôi phải quyết định gán `v=0` hay `v=1` cho các khớp phần thân dưới và tay phải. Căn cứ thị giác cho thấy toàn bộ các bộ phận này bị che khuất hoàn toàn bởi người đứng trước (áo đen) và chiếc xe máy, không lộ ra bất kỳ đường nét nào của trang phục hay cơ thể liền kề. Do không có cơ sở để xác định vị trí giải phẫu bên dưới lớp che khuất này, tôi quyết định loại bỏ (xóa người này hoặc gán `v=0` cho các khớp đó) thay vì cố suy đoán (hallucinate) vị trí của chúng.
