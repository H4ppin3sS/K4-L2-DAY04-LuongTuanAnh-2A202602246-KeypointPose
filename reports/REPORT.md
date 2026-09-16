# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: ___Lương Tuấn Anh___   Nhóm: __T046__   Ngày: _16/09/2026_

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số                         | Giá trị |
| -------------------------------- | --------: |
| Số ảnh đã gán               |        20 |
| Số skeleton                     |        29 |
| v=2 / v=1 / v=0                  | 338/60/95 |
| Thời gian trung bình mỗi ảnh |     4.35p |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`): left_eye, left_ankle, right_ankle

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.
Có, đấy là những lớp mà tôi phải đoán, ước lượng xem là các khớp đó ở vị trí nào.

<!-- Trả lời 2–4 câu. Phân biệt “hay bị che” với “khó xác định vị trí giải phẫu”; nêu bằng
chứng nhìn thấy thay vì chỉ nêu cảm giác. -->

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số                | Trước rework | Sau rework |
| ----------------------- | -------------: | ---------: |
| OKS trung bình         |          0.915 |      0.920 |
| OKS@0.50                |          1.000 |      1.000 |
| OKS@0.75                |          1.000 |      1.000 |
| Lỗi`dao_trai_phai`   |              0 |          0 |
| Lỗi`nham_nguoi`      |              1 |          1 |
| Lỗi`xoa_khop_bi_che` |              7 |          7 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?


Lỗi đảo trái/phải của tôi xảy ra ở ảnh `train_13.jpg`, người #1. Cụ thể, tôi đã đảo vị trí của hai khớp vai trái/phải và hai khớp hông trái/phải.

Đây là một ảnh tương đối dễ vì người vẫn nằm trong khung hình và các vùng vai, hông có thể quan sát được. Tôi vẫn mắc lỗi vì đã xác định trái/phải theo hướng nhìn của ảnh thay vì theo cơ thể của người. Ngoài ra, tôi chưa kiểm tra lại tính nhất quán giữa mắt → vai → hông trước khi hoàn tất annotation.

![1789554050828](image/REPORT/1789554050828.png)

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

| Chỉ số       | yolo26n-pose gốc | Sau fine-tune |   Chênh |
| -------------- | ----------------: | ------------: | -------: |
| pose_mAP50     |            0.8450 |        0.8450 |  +0.0000 |
| pose_mAP50-95  |            0.6853 |        0.6908 |  +0.0055 |
| pose_precision |            0.9734 |        0.9792 |  +0.0058 |
| pose_recall    |            0.8462 |        0.8462 |  +0.0000 |
| box_mAP50-95   |            0.8119 |        0.8041 | −0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?  Tức là  tăng 0.55 điểm phần trăm , không giảm. Vì vậy không có chuyện 20 ảnh làm model “học thêm nhưng làm hỏng” ở chỉ số này.
2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?  Bounding box chỉ cần xác định tương đối chính xác vùng chứa người, trong khi pose phải xác định vị trí cụ thể của 17 khớp. Những khớp bị che, nằm gần nhau hoặc khó quan sát sẽ làm pose khó hơn box.
3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn): Ảnh train_13 – lỗi đảo trái/phải. Ở người thứ nhất, vị trí vai trái/phải và hông trái/phải bị đảo. Nguyên nhân là xác định trái/phải theo hướng nhìn của ảnh thay vì theo cơ thể người.
4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu? train_13 có OKS thấp nhất: 0.478. Điều này có nghĩa là vị trí keypoint của model và nhãn bạn gán  bất đồng khá lớn. Nhưng OKS = 0.478 không cho biết trực tiếp ai đúng. Muốn kết luận ai đúng phải xem: Ảnh gốc, Ground truth, prediction của model, kiểm tra các điểm của train_13
5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?

Có, Một ảnh annotation không nhất quán có thể trở thành nguồn gây nhiễu cho quá trình học và làm khó cả việc đánh giá model

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

![1789553238468](image/REPORT/1789553238468.png)

train_03.jpg. Đó là keypoints left_elbow và left_wrist khi mà tôi đang phân vân giữa việc nên ước lượng, đoán rằng tay ở đâu hoặc là bỏ và đánh v = 0. Sau khi quan sát kĩ hơn thì việc ước lượng sai còn gây hậu quả tệ hơn là việc bỏ đi các phần thật sự không thể nhìn thấy hay ước lượng được sẽ giúp model học được tốt hơn thay vì học các phần sai dẫn đến kết quả cuối cùng không đạt yêu cầu.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->
