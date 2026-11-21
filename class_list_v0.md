# Danh sách lớp v0 — Trần Đức Kỳ · G02

Viết tiếp từ slide 54 bài giảng. Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**

## DMS (6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `eye_left` / `eye_right` | open / closed / unknown | Phát hiện buồn ngủ (mắt nhắm lâu) và mất tập trung; gán riêng từng mắt để không suy từ mắt còn lại | Có, trừ khi kính phản xạ hoặc kính râm tối |
| 2 | `gaze_zone` | road, mirror_l/r/c, cluster, center, lap, other | Cảnh báo tài xế không nhìn đường; biết đang nhìn gương hay màn hình giữa | Có, dựa vào hướng đầu và mắt; kém tin cậy khi mắt bị che |
| 3 | `face13` | 13 điểm, mỗi điểm cờ occluded/outside | Ước lượng tư thế đầu (yaw/pitch) và độ mở mắt, miệng; cờ che giúp model biết điểm nào không tin được | Có, nhưng điểm sau kính hoặc tay hay bị che |
| 4 | `glasses` | unknown / none / clear / sun | Chọn nhánh xử lý mắt: kính râm thì không đọc được eye state, kính trong thì dễ có phản xạ | Một phần: trên NIR kính râm có thể trông trong suốt, nên cần giá trị unknown |
| 5 | `hand.on_wheel` (+ `side`, `whose`) | yes / no / unknown | Kiểm tra tay trên vô lăng, cần cho cảnh báo yêu cầu nhận lại điều khiển ở xe tự lái | Có (hình dạng bàn tay, vô lăng) |
| 6 | `phone` | held / mounted / lap | Phát hiện dùng điện thoại khi lái (cầm tay khác với trên giá) | Có nếu thấy cạnh, góc hoặc màn hình; áp tai mà không thấy hình máy thì không gán |

## OMS (6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `person` + `seat` | driver, front_passenger, rear_left, rear_middle, rear_right | Đếm người theo ghế: nhắc thắt dây, điều chỉnh túi khí theo ghế có người | Có (hình dạng cơ thể) |
| 2 | `person.seatbelt` | unknown / worn_correct / worn_incorrect / not_worn | Nhắc thắt dây từng ghế; phân biệt thắt sai với không thắt | Khó: dây trên áo tối gần như mất, nên cần unknown |
| 3 | `child_seat` + `occupied` | no / unknown (không có yes ở v1.0) | Nhận ghế trẻ em để tắt túi khí ghế đó và nhắc quên trẻ khi có người bên trong | Có (hình dạng ghế); bên trong ghế thì khó |
| 4 | `infant_seat` + `facing` | rear / front | Ghế em bé quay sau không được đặt dưới túi khí đang bật; `facing` quyết định mức cảnh báo | Có (hình dạng) |
| 5 | `object` + `type` | bag / box / cup / other | Nhắc quên đồ trên ghế sau; tránh báo nhầm có người | Có (hình dạng); phân loại type phải dựa vào hình, không dựa màu |
| 6 | `frame_meta` (`modality`, `light`, `camera_blocked`) | ir/rgb · day/night/unknown · no/yes | Biết khung hình là loại nào và camera có bị che không để chọn model và ngưỡng phù hợp | Không áp dụng, là siêu dữ liệu của cả khung |

## 2 câu hỏi leo thang

Câu hỏi bạn muốn hỏi người viết guideline (hoặc khách hàng) vì guideline v1.0 chưa trả lời. Mỗi câu kèm **tên khung** làm ví dụ.

1. Câu hỏi: Guideline v1.0 chỉ cho `child_seat.occupied` là `no` hoặc `unknown` và nói bộ ảnh không có trẻ em ngồi. Nhưng nếu trong ghế trẻ em có hình giống người ngồi thì gán thế nào, và khi nào được dùng `no`? Có nên thêm giá trị `yes` để model học nhận ra ghế có người?
   Khung ví dụ: G02 S03_rgb, S04_rgb, S05_rgb (child_seat ở rear_middle / rear_right), đối chiếu với S02_rgb (ghế trống).
   Bạn tạm gán thế nào và vì sao: `occupied = unknown`. Không dùng `no` vì có thể sai nếu bên trong có người; không dùng `yes` vì v1.0 không cho phép. Nếu mở khung thấy rõ ghế trống thì đổi sang `no` (như S02).

2. Câu hỏi: Một cảnh SVIRO có hai ảnh RGB và IR-sim độ sáng rất khác nhau. `seatbelt` được gán theo từng ảnh hay phải thống nhất theo cảnh? Khi RGB quá tối nhưng IR-sim thấy rõ là không có dây thì RGB ghi `unknown` hay `not_worn`?
   Khung ví dụ: G02 S01_rgb (ghi `unknown`) và S01_ir (ghi `not_worn`); S07_rgb (RGB rất tối).
   Bạn tạm gán thế nào và vì sao: theo luật 4.4-1, mỗi ảnh theo những gì nhìn thấy trên chính ảnh đó (S01_rgb `unknown`, S01_ir `not_worn`); không suy từ ảnh kia của cùng cảnh.
