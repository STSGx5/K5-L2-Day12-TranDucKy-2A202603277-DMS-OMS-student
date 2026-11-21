# Tự đánh giá — Trần Đức Kỳ · G02

## 1. Tự rà soát 4 cảnh

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip`, `flag`, `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| S08_rgb | child_seat | seat_flip | `seat = driver` (giá trị mặc định đầu danh sách của CVAT); box nằm giữa ảnh nên đúng là `rear_middle`, khớp với S08_ir | Có |
| S03_rgb | child_seat | guess | `occupied = yes` không có trong guideline (chỉ no/unknown); đổi sang `unknown` vì không chắc bên trong | Có |
| S01_rgb | person (ghế rear_right) | missing | OMS: người lớn cần `person` kèm box `face`, bản export chưa có box `face` nào ở S01 | Chưa — cần mở S01 kiểm: thấy mặt thì vẽ box `face`, không thấy thì ghi lý do |
| D05 | hand | lr_flip? | Cả hai tay đều có y > 676 (cùng nằm một phía của người); cần kiểm tay `right` (898,774,1033,915) thật sự là tay phải hay đã gán ngược | Chưa — cần mở D05 kiểm |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: ghi `occupied = yes` cho `child_seat` (14 box trên S03–S06, S08–S10, cả RGB và IR-sim), và để sót `seat = driver` mặc định ở S08_rgb.

Vì sao mình mắc: <viết lại bằng lời của bạn. Gợi ý từ dữ liệu: thấy ghế có hình người nên ghi "có người" mà chưa đối chiếu danh sách giá trị cho phép; copy shape từ RGB sang IR-sim rồi giữ nguyên thuộc tính; không mở DETAILS để chọn lại `seat` sau khi vẽ box>

Lần sau tránh bằng cách: chạy `cabin_check` ngay sau 2 cảnh đầu thay vì để đến cuối; vẽ xong box nào thì mở DETAILS chọn lại `seat`; thấy tình huống guideline không có giá trị thì chọn `unknown` và ghi câu hỏi leo thang.

## 3. Ghi chú về disagreement_log

<điền sau khi review chéo: một dòng bạn của mình đúng và mình sai, và mình học được gì>
