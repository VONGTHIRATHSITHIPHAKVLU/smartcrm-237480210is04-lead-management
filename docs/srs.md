# Báo cáo Buổi 4

| Mục | Nội dung |
|---|---|
| Sinh viên | Vongthirath Sithiphak – 237480210is04 |
| Track / Luồng | SE – L8 Khảo sát hài lòng CSAT / NPS |
| Phiên bản | 0.1 – Buổi 4, Chuyên đề tốt nghiệp 1 |

## 1. Link commit

https://github.com/VONGTHIRATHSITHIPHAKVLU/smartcrm-237480210is04-lead-management

## 2. User Story: __ story | __ MUST | __ tiêu chí GWT (__ ngoại lệ)

### 2.1 Use story

| Mã | User Story | MoSCoW |
|---|---|---|
| US1 | Là Quản lý, tôi muốn tạo mẫu khảo sát CSAT hoặc NPS để chuẩn hóa cách đo mức hài lòng của khách hàng. | MUST |
| US2 | Là Nhân viên CSKH, tôi muốn gửi lời mời khảo sát cho khách ngay khi xử lý xong yêu cầu hỗ trợ để thu phản hồi khi trải nghiệm còn mới. | MUST |
| US3 | Là Khách hàng, tôi muốn trả lời khảo sát qua một đường dẫn mà không cần đăng nhập để phản hồi trong chưa đầy một phút. | MUST |
| US4 | Là Quản lý, tôi muốn xem điểm CSAT và NPS theo tuần hoặc tháng để đánh giá chất lượng dịch vụ theo thời gian. | SHOULD |
| US5 | Là Quản lý, tôi muốn hệ thống tự nhắc khách chưa trả lời sau 3 ngày để tăng tỷ lệ phản hồi. | SHOULD |
| US6 | Là Nhân viên CSKH, tôi muốn thấy danh sách phản hồi tiêu cực chưa xử lý để liên hệ lại khách kịp thời. | SHOULD |
| US7 | Là Quản lý, tôi muốn xuất báo cáo CSAT/NPS ra CSV để gửi cho ban giám đốc. | COULD |
| US8 | Là Khách hàng, tôi muốn từ chối nhận khảo sát về sau để không bị làm phiền. | COULD |

### 2.1 Tiêu chí chấp nhận Given–When–Then (story MUST)

**US1 – Tạo mẫu khảo sát**

- **AC1.1** Given Quản lý đã đăng nhập, When nhập tên "CSAT sau hỗ trợ", chọn loại CSAT và lưu, Then hệ thống tạo mẫu khảo sát và hiển thị trong danh sách.
- **AC1.2 (ngoại lệ)** Given đã có mẫu tên "CSAT sau hỗ trợ", When Quản lý lưu mẫu khác cùng tên, Then hệ thống từ chối và báo "Tên mẫu đã tồn tại".

**US2 – Gửi lời mời khảo sát**

- **AC2.1** Given yêu cầu hỗ trợ ở trạng thái "Hoàn tất" và khách có email, When Nhân viên CSKH chọn mẫu khảo sát và bấm gửi, Then hệ thống tạo lời mời khảo sát và chuyển cho Dịch vụ Email/SMS trong vòng 60 giây.
- **AC2.2** Given lời mời vừa tạo, When xem chi tiết, Then đường dẫn riêng có hạn 14 ngày kể từ lúc gửi.
- **AC2.3 (ngoại lệ)** Given yêu cầu hỗ trợ đã có lời mời khảo sát, When Nhân viên CSKH gửi thêm lần nữa, Then hệ thống từ chối và báo "Yêu cầu hỗ trợ này đã được gửi khảo sát".

**US3 – Gửi phản hồi khảo sát**

- **AC3.1** Given lời mời còn hạn và chưa trả lời, When khách mở đường dẫn, chọn điểm và bấm gửi, Then hệ thống lưu phản hồi và hiển thị lời cảm ơn.
- **AC3.2 (ngoại lệ)** Given lời mời đã trả lời, When khách mở lại đường dẫn, Then hệ thống không cho trả lời lần hai và báo "Bạn đã gửi phản hồi".
- **AC3.3 (ngoại lệ)** Given lời mời quá 14 ngày, When khách mở đường dẫn, Then hệ thống báo "Khảo sát đã hết hạn" và không hiển thị form.

## 3. Danh sách used case diagram

| Mã | Use case | Actor chính |
|---|---|---|
| UC1 | Tạo mẫu khảo sát | Quản lý |
| UC2 | Gửi lời mời khảo sát | Nhân viên CSKH |
| UC3 | Gửi phản hồi khảo sát | Khách hàng |
| UC4 | Xem báo cáo CSAT/NPS | Quản lý |
| UC5 | Gửi nhắc nhở khảo sát | Bộ lập lịch |
| UC6 | Xử lý phản hồi tiêu cực | Nhân viên CSKH |
| UC7 | Xuất báo cáo ra CSV | Quản lý |
| UC8 | Từ chối nhận khảo sát | Khách hàng |

### 3.2 Use case diagram

![Use case diagram](images/use-case-diagram.png)

### 3.3 Đặc tả chi tiết use case quan trọng nhất – UC3 Gửi phản hồi khảo sát

| Mục | Nội dung |
|---|---|
| Actor | Khách hàng |
| Mục tiêu | Khách hàng chấm điểm và gửi phản hồi cho một yêu cầu hỗ trợ đã hoàn tất |
| Điều kiện trước | Khách đã nhận lời mời khảo sát; lời mời chưa trả lời và chưa quá 14 ngày |
| Điều kiện sau (thành công) | Phản hồi được lưu; lời mời chuyển sang trạng thái "Đã trả lời"; phản hồi tiêu cực được gắn cờ |

**Luồng chính**

1. Khách mở đường dẫn riêng trong email/SMS.
2. Hệ thống kiểm tra đường dẫn hợp lệ, chưa hết hạn, chưa trả lời.
3. Hệ thống hiển thị câu hỏi của mẫu khảo sát (thang 1–5 hoặc 0–10) và ô nhận xét tùy chọn.
4. Khách chọn điểm.
5. Khách nhập nhận xét (tùy chọn) và bấm "Gửi".
6. Hệ thống kiểm tra điểm hợp lệ.
7. Hệ thống lưu phản hồi, đổi trạng thái lời mời, gắn cờ nếu là phản hồi tiêu cực.
8. Hệ thống hiển thị lời cảm ơn.

**Luồng ngoại lệ (đánh số theo bước)**

- **2a** Đường dẫn không tồn tại: hệ thống báo "Đường dẫn không hợp lệ", kết thúc.
- **2b** Lời mời quá 14 ngày: hệ thống báo "Khảo sát đã hết hạn", kết thúc.
- **2c** Lời mời đã có phản hồi: hệ thống báo "Bạn đã gửi phản hồi", kết thúc.
- **6a** Khách chưa chọn điểm hoặc điểm ngoài thang: hệ thống báo lỗi tại ô điểm và quay lại bước 4.
- **7a** Lỗi lưu dữ liệu: hệ thống báo "Gửi chưa thành công, vui lòng thử lại" và giữ nguyên điểm đã chọn; quay lại bước 5.

## 4. Mục đích, phạm vi, bảng thuật ngữ

### Mục đích

Tài liệu mô tả yêu cầu cho chức năng khảo sát mức độ hài lòng của khách hàng (CSAT/NPS) trong SmartCRM: tạo mẫu khảo sát, gửi lời mời sau khi xử lý xong yêu cầu hỗ trợ, thu phản hồi và tổng hợp điểm.

### Phạm vi

- **Trong phạm vi:** tạo mẫu khảo sát; gửi và nhắc lời mời khảo sát qua email/SMS; khách hàng trả lời qua đường dẫn riêng; tính CSAT và NPS theo kỳ; xử lý phản hồi tiêu cực; xuất báo cáo CSV; từ chối nhận khảo sát.
- **Ngoài phạm vi (WON'T ở phiên bản này):** gửi khảo sát qua Zalo/Messenger, khảo sát nhiều câu hỏi, phân tích cảm xúc tự động trên phần nhận xét.

### Bảng thuật ngữ

Mỗi khái niệm chỉ dùng MỘT tên, kể cả trong sơ đồ Use Case và API.

| Thuật ngữ dùng trong tài liệu | Định nghĩa | Tên trong mã/API |
|---|---|---|
| Khảo sát | Một lần hỏi ý kiến khách hàng về mức hài lòng | `survey` |
| Mẫu khảo sát | Cấu hình câu hỏi: loại CSAT (thang 1–5) hoặc NPS (thang 0–10) | `survey-template` |
| Lời mời khảo sát | Bản ghi gửi một khảo sát cho một khách hàng, kèm đường dẫn riêng có hạn | `survey-invitation` |
| Phản hồi | Câu trả lời của khách hàng cho một lời mời khảo sát | `response` |
| Yêu cầu hỗ trợ | Một yêu cầu của khách hàng được nhân viên CSKH xử lý; khi hoàn tất sẽ kích hoạt khảo sát | `support-request` |
| CSAT | Tỷ lệ phản hồi chấm 4–5 trên tổng số phản hồi CSAT, tính theo % | – |
| NPS | % người ủng hộ (chấm 9–10) trừ % người chê (chấm 0–6) trên tổng số phản hồi NPS, giá trị từ −100 đến +100 | – |
| Phản hồi tiêu cực | Phản hồi CSAT ≤ 2 hoặc NPS ≤ 6 | – |

## 5. Mẫu track (API contract / Data spec / ML statement): Xong – thiếu:

| Phương thức | Đường dẫn | Mục đích | Truy vết |
|---|---|---|---|
| POST | `/api/survey-templates` | Tạo mẫu khảo sát | US1 / FR1 |
| POST | `/api/survey-invitations` | Tạo và gửi lời mời khảo sát | US2 / FR2 |
| GET | `/api/public/surveys/{token}` | Lấy nội dung khảo sát theo đường dẫn riêng | US3 / FR3 |
| POST | `/api/public/surveys/{token}/responses` | Gửi phản hồi khảo sát | US3 / FR4 |

## 6. Giờ thực tế xong:

| Hạng mục | Thời gian |
|---|---|
| User Story | 2:30 (gồm thời gian làm ở nhà) |
| Use Case | 1:00 |
| SRS + track | 2:00 |

## 7. Checklist đạt: 10/10 – các mục chưa đạt: không

## 8. Peer review

- Với bạn PHOUTTHAVONG Khanxay (track SE). Tóm tắt phạm vi: [Đúng/Sai].
- Góp ý sẽ sửa:
  1. Thêm bảng trạng thái của lời mời khảo sát (đã gửi / đã trả lời / hết hạn / đã từ chối) và ghi rõ FR nào dùng trạng thái nào.
  2. Ghi rõ điều kiện đo cho các NFR (số phản hồi, số người dùng đồng thời, môi trường đo) và đối chiếu với số liệu thật của case study.

## 9. Chỗ chưa xong/còn phân vân – đã thử gì, kết quả ra sao

- Các ngưỡng NFR (2 giây, 3 giây, 99%) là đề xuất của em, chưa có số liệu thật của case study để đối chiếu.
- Đã thử đặt ngưỡng theo quy mô ước tính và ghi điều kiện đo kèm theo; kết quả là cần số liệu thật để xác nhận hoặc chỉnh lại.

## 10. Một quyết định em đưa ra hôm nay – dựa trên tiêu chí nào đã học

Chọn 3 story MUST (US1, US2, US3) vì chỉ các story này nằm trên đường đi tối thiểu của luồng (tạo mẫu khảo sát → gửi lời mời → khách trả lời). Dựa trên quy tắc MoSCoW: 2–3 story MUST.

## 11. Việc còn dở – hạn em tự đặt (trước buổi 5)

- [ ] Điền Phụ lục A (khai báo công cụ AI) trong SRS.
- [ ] Commit và push bản SRS đã cập nhật (bảng trạng thái, điều kiện đo NFR), rồi merge vào `main`.
- [ ] Đối chiếu ngưỡng NFR với số liệu thật của case study.
- [ ] Đọc tài liệu tự học buổi 5 và vẽ nháp ERD, kiến trúc.

**Hạn:** /

## 12. Tự đánh giá: Đạt phần lớn

---

## Ba chỗ bạn cần kiểm lại trước khi nộp

- **Câu 6, peer review:** slide yêu cầu đổi bài với bạn khác track. Bạn Khanxay là SE, nên nếu track của bạn cũng là SE thì cần nhờ thêm một bạn track DA hoặc AI xem bài, hoặc ghi chú rõ lý do. Bạn cũng nên hỏi lại bạn ấy về "6 trạng thái" và "500 phiếu" trong góp ý cũ, vì SRS của bạn không có khái niệm đó.
- **Câu 4 và câu 5:** tổng 5 giờ 30 vượt buổi học 135 phút, nên mình đã thêm "(gồm thời gian làm ở nhà)". Chỉ giữ ghi chú này nếu đúng với thực tế của bạn. Tương tự, "10/10" chỉ nên giữ khi bạn đã kiểm đủ 10 mục và link commit mở được.
- **Câu 3 trong ảnh:** ô vuông bên cạnh "Xong / Dở" là ô chọn. Bạn xóa phần không dùng để câu chỉ còn một đáp án.