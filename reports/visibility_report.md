# Visibility report

- Thư mục nhãn: `/home/peter/Documents/Day4-NguyenMinhQuan-2A202602268/dataset/labels/train`
- 20 ảnh, 28 skeleton, trung bình 16.04 khớp có v > 0 mỗi người
- Tổng: v=2 361 | v=1 88 | v=0 27

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 23 | 5 | 0 | 18% |
| 1 | left_eye | 21 | 7 | 0 | 25% |
| 2 | right_eye | 22 | 6 | 0 | 21% |
| 3 | left_ear | 12 | 16 | 0 | 57% |
| 4 | right_ear | 15 | 13 | 0 | 46% |
| 5 | left_shoulder | 26 | 2 | 0 | 7% |
| 6 | right_shoulder | 27 | 1 | 0 | 4% |
| 7 | left_elbow | 24 | 4 | 0 | 14% |
| 8 | right_elbow | 26 | 2 | 0 | 7% |
| 9 | left_wrist | 21 | 7 | 0 | 25% |
| 10 | right_wrist | 21 | 6 | 1 | 21% |
| 11 | left_hip | 24 | 4 | 0 | 14% |
| 12 | right_hip | 25 | 3 | 0 | 11% |
| 13 | left_knee | 20 | 4 | 4 | 14% |
| 14 | right_knee | 21 | 3 | 4 | 11% |
| 15 | left_ankle | 16 | 3 | 9 | 11% |
| 16 | right_ankle | 17 | 2 | 9 | 7% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
