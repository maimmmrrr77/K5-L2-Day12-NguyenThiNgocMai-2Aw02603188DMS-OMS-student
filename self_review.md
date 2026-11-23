# Tự đánh giá — Nguyễn Thị Ngọc Mai · G01

## 1. Tự rà soát 4 cảnh

Nghỉ tay 1 phút, mở lại bài của mình, chọn 4 ảnh (2 S##, 2 D##), đọc như người lạ.

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| S01_rgb.png | frame_meta | wrong_class | Chọn nhầm `light` theo bối cảnh thay vì `unknown` cho ảnh dựng SVIRO | Có |
| S06_ir.png | person, child_seat | ok | Đã gọi đúng vị trí ghế theo xe (trái ảnh là `rear_right`), `seatbelt=not_worn` | Có |
| D01.png | face13 | lr_flip | Bật nhầm cờ `occluded` sang mắt phải thay vì mắt trái đang bị che | Có |
| D02.png | face, face13 | ok | Đã đặt đúng chiều mắt trái có tọa độ y lớn hơn mắt phải trên ảnh xoay 90° | Có |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: Gán nhầm thuộc tính light khác unknown trên bộ ảnh mô phỏng SVIRO và nhầm lẫn giữa mắt trái/mắt phải khi bật cờ occluded trên ảnh tài xế xoay 90°.

Vì sao mình mắc: Bị quen mắt nhìn theo bối cảnh sáng/tối thông thường trong ảnh màu thay vì nhớ SVIRO là ảnh dựng; đồng thời khi thao tác trên ảnh xoay ngang chưa đối chiếu kỹ lại hướng người lái trước khi gắn cờ cho từng điểm mắt.

Lần sau tránh bằng cách: Luôn kiểm tra quy định riêng của từng dataset trong Guideline trước khi gắn tag frame_meta, và luôn xác định trục tọa độ y trên ảnh xoay 90° trước khi chỉnh thuộc tính mắt và cờ occluded.

## 3. Ghi chú về disagreement_log

Một dòng bạn của mình đúng và mình sai. Mình học được gì từ dòng đó: tôi sai ở S06_rgb.png,frame_meta,modality, bạn sai ở D05.png,face,glasses. Bài học rút ra: luôn kiểm tra kỹ đuôi tên file trước khi đặt tag frame_meta, không để nguyên giá trị đầu danh sách mặc định của CVAT.
