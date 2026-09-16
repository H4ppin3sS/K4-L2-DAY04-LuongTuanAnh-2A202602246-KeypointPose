# Mini guideline - nhóm: ___T046___  |  người gán: Lương Tuấn Anh  |  ngày: _16/09/2026_

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống                                                                                                                 | Luật nhóm bạn chọn                                                                                                                                                                                    | Vì sao                                                                                                                                                            |
| ---------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Hông của người mặc quần áo dài<br />![1789548963852](image/GUIDELINE_MINI/1789548963852.png)                         | xác định vị trí của khớp hông theo cấu trúc của cơ thể, không chạy theo mép của quần áo                                                                                                | quần áo rộng, dài và có thể không ôm sát được lấy cơ thể. Nếu lấy mép thì có thể sẽ bị lệch                                               |
| Tai bị tóc hoặc mũ bảo hiểm che một phần<br />![1789548979186](image/GUIDELINE_MINI/1789548979186.png)               | Nếu vẫn có thể xác định được vị trí của tai, đặt điểm tại vị trí ước lượng và cho v = 1. Nếu bị che hoàn toàn nhưng vẫn có thể ước lượng được thì vẫn cho v = 1 | Tai vẫn nằm trong khung ảnh nhưng bị che khuất đi. Keypoints bị che nhưng vẫn phải đặt và gán v = 1                                                 |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên)<br />![1789548986594](image/GUIDELINE_MINI/1789548986594.png) | Các khớp nằm ngoài khung ảnh thì đánh v = 0                                                                                                                                                       | Không được đoán vị trí của keypoints ở ngoài ảnh, gắn lệch keypoints sẽ ảnh hưởng đến việc train model sau này                             |
| Cổ tay nằm sau tay lái / sau thân mình<br />![1789549041988](image/GUIDELINE_MINI/1789549041988.png)                    | Vẫn có thể ước lượng được dựa vào cấu trúc của cơ thể và đánh v = 1                                                                                                                   | Đây là trường hợp bị che khuất, các khớp có tồn tại chỉ là không trực tiếp nhìn thấy                                                           |
| Hai người chồng lên nhau<br />![1789549007952](image/GUIDELINE_MINI/1789549007952.png)                                   | xác định từng người riêng biệt, gán keypoints cho từng người, người bị khuất có các điểm nào không thể tự ước lượng được thì đánh v = 0                                 | Khi hai người chồng lên nhau, các bộ phận có thể rất gần nhau. Gán nhầm người sẽ làm skeleton và quan hệ không gian của người đó sai.     |
| Người nhỏ đến mức nào thì không gán nữa<br />![1789549030934](image/GUIDELINE_MINI/1789549030934.png)             | Nếu vẫn có thể xác định được thì vẫn gán keypoints theo quy ước                                                                                                                            | Tránh việc mỗi người tự chọn một ngưỡng kích thước khác nhau. Kích thước nhỏ không đồng nghĩa với việc người đó không cần annotation |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `___train_01.jpg___`, người thứ `_1__`, khớp `___RIGHT_KNEE, LEFT_KNEE___`

- Mơ hồ ở chỗ nào: hai phần đầu gối bị che khuất bởi tạp đề to.
- Bạn quyết thế nào: dựa vào đặc điểm cấu trúc cơ thể để ước lượng và đánh keypoints
- Vì sao: Vì nếu đánh bừa hoặc đánh theo mép của tạp đề có thể dẫn đến sai lệch
- Nếu người khác quyết ngược lại thì model học sai cái gì: model có thể học sai và nếu là đối tượng khác với bộ quần áo to hơn thì sẽ nhận diện sai

### Ca 2 - ảnh `___train_03.jpg__`, người thứ `_55__`, khớp `__LEFT_ELBOW , LEFT_WRIST____`

- Mơ hồ ở chỗ nào: phần khuỷu và cổ tay trái của người đó hoàn toàn không thể đoán được và bị khuất hoàn toàn.
- Bạn quyết thế nào: đánh v = 0
- Vì sao: nhìn trực tiếp vào ảnh không thể ước lượng được rằng phần khuỷu tay và cổ tay trái của họ nằm ở đâu sau lưng người đứng trước
- Nếu người khác quyết ngược lại thì model học sai cái gì:model học sai dẫn đến kết quả không tốt

### Ca 3 - ảnh `_train_13.jpg_____`, người thứ `_307__`, khớp `______`

- Mơ hồ ở chỗ nào: người ở xa, khá mờ
- Bạn quyết thế nào: dựa vào hình ảnh, vẫn sẽ đánh keypoints những phần có thể nhìn thấy được
- Vì sao: Vị trí đứng của người đó vẫn chă quá xa để mà lược bỏ đi
- Nếu người khác quyết ngược lại thì model học sai cái gì: có thể khi thấy mờ và xa sẽ bỏ đi không đánh keypoints dẫn đến khi train model sẽ không học được những người ở xa đó

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `______` (bạn `___%` / họ `___%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**:
- Luật mới bổ sung vào mục 2 sau khi thống nhất:
