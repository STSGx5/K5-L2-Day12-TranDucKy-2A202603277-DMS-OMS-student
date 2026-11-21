# Khai báo dùng AI — Trần Đức Kỳ

Nhắc lại: không đưa ảnh cabin hay ảnh tài xế (kể cả ảnh chụp màn hình) cho bất kỳ công cụ AI nào.

| # | Công cụ | Bạn hỏi gì (tóm tắt) | Bạn dùng kết quả thế nào | Bạn có kiểm lại không? Bằng gì? |
|---|---|---|---|---|
| 1 | Claude (Claude Code) | Đọc file RAR repo bài lab, liệt kê file cần nộp, chạy `cabin_check` trên `oms.xml` và `driver.xml` của mình và sửa lỗi nhãn | Đổi `child_seat.occupied` yes→unknown (14 box), sửa `seat` driver→rear_middle ở S08_rgb, gộp S01–S08 + D01–D06 thành `export_final.zip`, S09–S10 thành `export_extra.zip` | Có: chạy lại `scripts/cabin_check.py` bản của repo, 0 lỗi 0 cảnh báo (check_final.txt, check_extra.txt) |
| 2 | Claude (Claude Code) | Soạn nháp `ir_log.csv`, `class_list_v0.md`, `self_review.md`, `TEAM.md` | Dùng làm nháp; số liệu IR lấy từ `ir_pairs.csv` do `rgb_ir_diff.py` tạo, các nhãn lấy từ XML của mình; phần có dấu `<...>` mình tự điền | Mình đối chiếu số liệu với `ir_pairs.csv` và mở lại khung ảnh để xác nhận từng dòng (cần làm trước khi nộp) |

Ghi chú trung thực: file RAR tải lên AI có chứa các ảnh cabin (`G02/`, `ir_pairs/`). AI chỉ đọc XML, CSV và văn bản, không mở hay xem ảnh nào; không nhờ AI tự gán nhãn từ ảnh. Việc tải cả repo lên một công cụ AI trái với RULES §2, nên mình khai báo ở đây.
