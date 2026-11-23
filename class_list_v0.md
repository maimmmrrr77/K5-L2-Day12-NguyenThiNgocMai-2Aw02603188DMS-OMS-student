# Danh sách lớp v0 — <Họ tên> · <Gxx>

Viết tiếp từ slide 54 bài giảng. Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**
Xóa 2 dòng ví dụ trước khi nộp.

## DMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `gaze_zone` (`face`) | `road` / `mirror_l` / `mirror_r` / `mirror_c` / `cluster` / `center` / `lap` / `other` | Phát hiện tài xế mất tập trung, rời mắt khỏi đường lái quá lâu | Có, dựa theo hướng đầu và tròng mắt (trừ khi đeo kính râm tối hoặc lóa sáng) |
| 2 | `face13` (13 điểm mốc mặt) | Tọa độ `(x, y)` + cờ `occluded` / `outside` | Tính góc xoay đầu (yaw, pitch, roll) và độ mở mắt (EAR) để cảnh báo gật gù, quay đầu | Có, trừ các điểm bị tay, chai nước hoặc đốm lóa kính che khuất |
| 3 | `glasses` (`face`) | `none` / `clear` / `sun` / `unknown` | Hiệu chỉnh ngưỡng nhận diện mắt và giải thích nguyên nhân mất tín hiệu đồng tử | Có thể thấy gọng/tròng, nhưng trên NIR kính râm dễ trông trong suốt như kính thường |
| 4 | `hand` (`on_wheel`, `side`, `whose`) | `yes` / `no` / `unknown` (`left`/`right`, `driver`/`passenger`) | Giám sát tài xế có giữ tay trên vô lăng hay không khi xe chạy chế độ bán tự động / tự lái | Có, nhận diện rõ hình dạng bàn tay và vị trí tương đối với vô lăng |
| 5 | `phone` (`state`) | `held` / `mounted` / `lap` | Cảnh báo hành vi sử dụng điện thoại cầm tay gây mất an toàn khi đang lái xe | Có nếu thấy rõ cạnh, góc hoặc màn hình phát sáng (khó phân biệt nếu bị tay che kín) |
| 6 | `seatbelt` (`person` - driver) | `worn_correct` / `worn_incorrect` / `not_worn` / `unknown` | Nhắc nhở tài xế thắt dây an toàn đúng quy cách trước và trong khi xe chạy | Có, nhưng dễ bị mất dấu nếu dải dây tối màu trùng với áo tối hoặc nằm trong vùng tối |

## OMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `person` (`seat`) | `rear_left` / `rear_middle` / `rear_right` (theo xe) | Đếm số lượng và xác định vị trí hành khách ngồi trên xe để tối ưu điều hòa và bung túi khí | Có, thấy rõ hình khối cơ thể (cần tăng sáng nếu ngồi góc khuất/tối) |
| 2 | `seatbelt` (`person` - OMS) | `worn_correct` / `worn_incorrect` / `not_worn` / `unknown` | Cảnh báo hành khách hàng ghế sau chưa thắt dây hoặc thắt sai quy cách (dưới nách/sau lưng) | Hạn chế: dễ chìm vào bóng tối hoặc nếp gấp áo trên ảnh IR độ phân giải thấp |
| 3 | `child_seat` (`seat`, `occupied`) | `no` / `unknown` (theo ghế) | Nhận diện ghế an toàn trẻ em đang lắp cố định trên hàng ghế sau để điều chỉnh chế độ an toàn | Có (qua hình dáng khung ghế tựa cao đặc trưng) |
| 4 | `infant_seat.facing` | `rear` / `front` | Xác định hướng lắp nôi em bé (quay ngược về sau hay quay trước) để kích hoạt khóa túi khí phù hợp | Có (dựa vào hướng quai xách và phần tựa đầu của nôi) |
| 5 | `object` (`type`, `seat`) | `bag` / `box` / `cup` / `other` | Phân biệt đồ vật để trên ghế với hành khách thật, tránh báo động giả nhắc thắt dây an toàn | Thấy được hình khối vật thể, nhưng khó phân loại chi tiết chất liệu/màu sắc |
| 6 | `frame_meta.camera_blocked` | `yes` / `no` | Phát hiện camera cabin bị che khuất, bẩn hoặc hỏng để cảnh báo hệ thống mất khả năng giám sát | Có, nhận diện ngay qua vùng ảnh bị che đen hoàn toàn hoặc mất chi tiết |

## 2 câu hỏi leo thang

Câu hỏi bạn muốn hỏi người viết guideline (hoặc khách hàng) vì guideline v1.0 chưa trả lời. Mỗi câu kèm **tên khung** làm ví dụ.

1. Câu hỏi: Trong bộ ảnh OMS (SVIRO), khi bên trong nôi em bé có người, guideline v1.0 hiện chỉ cho phép `child_seat.occupied` nhận `no`/`unknown`/`yes`, `infant_seat` không có thuộc tính `occupied`, và quy định `person` chỉ dành cho người lớn. Vậy khi gặp trường hợp này thì phải gán nhãn như thế nào để model không học nhầm?
   Khung ví dụ: G01 - S02_ir.png
   Bạn tạm gán thế nào và vì sao: Em hỏi coach và được hướng dẫn là bỏ qua không gán person cho em bé.