# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 27 skeleton, trung bình 16.0 khớp có v > 0 mỗi người
- Tổng: v=2 311 | v=1 121 | v=0 27

So sánh với `..\ban_cung_nhom\dataset\labels\train` (0 skeleton).
Cột **lệch** là hiệu số phần trăm v=1 - chỗ nào lệch nhiều nhất là chỗ guideline chưa nói rõ.

| # | Khớp | %v=1 (bạn) | %v=1 (đối chiếu) | lệch |
| ---: | --- | ---: | ---: | ---: |
| 3 | left_ear | 67% | 0% | 67 |
| 4 | right_ear | 52% | 0% | 52 |
| 9 | left_wrist | 33% | 0% | 33 |
| 10 | right_wrist | 33% | 0% | 33 |
| 1 | left_eye | 30% | 0% | 30 |
| 2 | right_eye | 30% | 0% | 30 |
| 11 | left_hip | 26% | 0% | 26 |
| 14 | right_knee | 26% | 0% | 26 |
| 16 | right_ankle | 26% | 0% | 26 |
| 0 | nose | 22% | 0% | 22 |
| 12 | right_hip | 22% | 0% | 22 |
| 13 | left_knee | 22% | 0% | 22 |
| 7 | left_elbow | 19% | 0% | 19 |
| 15 | left_ankle | 15% | 0% | 15 |
| 5 | left_shoulder | 11% | 0% | 11 |
| 8 | right_elbow | 11% | 0% | 11 |
| 6 | right_shoulder | 4% | 0% | 4 |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
