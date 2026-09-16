# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 27 skeleton, trung bình 16.15 khớp có v > 0 mỗi người
- Tổng: v=2 302 | v=1 134 | v=0 23

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 21 | 6 | 0 | 22% |
| 1 | left_eye | 18 | 9 | 0 | 33% |
| 2 | right_eye | 19 | 8 | 0 | 30% |
| 3 | left_ear | 8 | 19 | 0 | 70% |
| 4 | right_ear | 11 | 16 | 0 | 59% |
| 5 | left_shoulder | 26 | 1 | 0 | 4% |
| 6 | right_shoulder | 26 | 1 | 0 | 4% |
| 7 | left_elbow | 21 | 6 | 0 | 22% |
| 8 | right_elbow | 22 | 5 | 0 | 19% |
| 9 | left_wrist | 18 | 9 | 0 | 33% |
| 10 | right_wrist | 19 | 7 | 1 | 26% |
| 11 | left_hip | 18 | 9 | 0 | 33% |
| 12 | right_hip | 20 | 7 | 0 | 26% |
| 13 | left_knee | 14 | 10 | 3 | 37% |
| 14 | right_knee | 15 | 9 | 3 | 33% |
| 15 | left_ankle | 14 | 5 | 8 | 19% |
| 16 | right_ankle | 12 | 7 | 8 | 26% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
