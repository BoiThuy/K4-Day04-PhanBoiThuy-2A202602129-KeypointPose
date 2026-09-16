# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 14.52 khớp có v > 0 mỗi người
- Tổng: v=2 392 | v=1 29 | v=0 72

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 26 | 0 | 3 | 0% |
| 1 | left_eye | 23 | 1 | 5 | 3% |
| 2 | right_eye | 24 | 0 | 5 | 0% |
| 3 | left_ear | 20 | 5 | 4 | 17% |
| 4 | right_ear | 24 | 3 | 2 | 10% |
| 5 | left_shoulder | 29 | 0 | 0 | 0% |
| 6 | right_shoulder | 29 | 0 | 0 | 0% |
| 7 | left_elbow | 24 | 1 | 4 | 3% |
| 8 | right_elbow | 28 | 1 | 0 | 3% |
| 9 | left_wrist | 22 | 2 | 5 | 7% |
| 10 | right_wrist | 23 | 5 | 1 | 17% |
| 11 | left_hip | 21 | 3 | 5 | 10% |
| 12 | right_hip | 26 | 1 | 2 | 3% |
| 13 | left_knee | 18 | 2 | 9 | 7% |
| 14 | right_knee | 19 | 3 | 7 | 10% |
| 15 | left_ankle | 18 | 0 | 11 | 0% |
| 16 | right_ankle | 18 | 2 | 9 | 7% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
