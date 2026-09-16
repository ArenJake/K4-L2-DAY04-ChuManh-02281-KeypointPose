# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Chu Mạnh   Nhóm: ______   Ngày: 16/9/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số                         |      Giá trị |
| -------------------------------- | -------------: |
| Số ảnh đã gán               |             20 |
| Số skeleton                     |             27 |
| v=2 / v=1 / v=0                  | 302 / 134 / 23 |
| Thời gian trung bình mỗi ảnh |        3 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. Left ear - 70%
2. Right ear - 59%
3. Left eye - 33%

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Đúng, nhiều ảnh có tai và mắt bị che bởi mũ hoặc tóc hoặc do quay mặt đi nên mình hay để v=1

<!-- Trả lời 2–4 câu. Phân biệt “hay bị che” với “khó xác định vị trí giải phẫu”; nêu bằng
chứng nhìn thấy thay vì chỉ nêu cảm giác. -->

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số                | Trước rework | Sau rework |
| ----------------------- | -------------: | ---------: |
| OKS trung bình         |          0.958 |      0.950 |
| OKS@0.50                |          0.931 |      1.000 |
| OKS@0.75                |          0.931 |      1.000 |
| Lỗi`dao_trai_phai`   |              0 |          0 |
| Lỗi`nham_nguoi`      |              0 |          0 |
| Lỗi`xoa_khop_bi_che` |              0 |          0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào): trong train_04, người thứ hai được mình gắn nhãn ban đầu không có hai bên hông do mình không nhìn rõ. Sau khi chấm với gold thì mình add thêm hai điểm đấy vào

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?:`0.845`** Không có lỗi đảo trái/phải.” Rồi thêm 1 câu: bạn đã kiểm bằng màu xanh (trái) / cam (phải) trong `outputs/vis_train`.

<!-- Nếu không có lỗi, ghi rõ “Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.” -->

## 3. Kiểm chéo

Bạn cùng nhóm: ______

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| ----- | ---: | --: | ----: | --------------------------------------- |
|       |      |     |       |                                         |
|       |      |     |       |                                         |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số       | yolo26n-pose gốc | Sau fine-tune | Chênh |
| -------------- | ----------------: | ------------: | -----: |
| pose_mAP50     |         `0.845` |     `0.845` |      0 |
| pose_mAP50-95  |        `0.6853` |    `0.6908` |     55 |
| pose_precision |        `0.9734` |    `0.9792` |     58 |
| pose_recall    |        `0.8462` |    `0.8462` |      0 |
| box_mAP50-95   |        `0.8119` |    `0.8041` |     78 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?

pose_mAP50-95 tăng lên 55

1. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?

Hai số liệu kia cách nhau 0.1133 sau fine tune. Model tìm

 người dễ hơn tìm chính xác các khớp , vì bounding box chỉ cần bao quanh người, còn pose phải xác định chính xác từng mắt, vai, khuỷu tay, cổ tay, đầu gối và cổ chân. Các khớp dễ bị che, nhỏ, hoặc thay đổi theo tư thế nên khó hơn.

1. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):

Ở ảnh `test_07`, model mắc lỗi  **nhầm người** : một số khớp của người đứng trước bị gắn sang cơ thể người đứng sau. Đây không phải lỗi lệch nhẹ vì vị trí khớp bị kéo sang một người khác.

1. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?

Ảnh `train_06` có OKS thấp nhất giữa model và nhãn của mình, với OKS `0.634`. Mình cho rằng  **nhãn của tôi đúng hơn** , vì khi so với gold, nhãn `train_06` đạt OKS `0.9577`. Model bị sai chủ yếu ở các khớp nhỏ hoặc bị che như mũi, mắt, cổ tay, đầu gối và cổ chân; gold cũng xác nhận các vị trí tôi gán là phù hợp.

1. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?

Không, sau rework, ảnh mình gán tệ nhất là `train_04`

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

Key point 307 trong ảnh train_13 có 2 người ở đằng sau nhưng bị mờ do họ không phải là trọng tâm. Mình ban đầu định để cho người keypoints do gold bảo vậy nhưng mà mình muốn quyến định để người đằng sau cùng là v=0. Lí do là họ bị mờ quá, nếu mình không nhìn ra được thì máy cũng khó tìm. Nếu mình vẫn để là bị che thì máy có thể lầm tưởng đốm màu trong ảnh có thể là người bị che. Máy cần phải được lấy data train chính xác để hoạt động chính xác nhất.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->
