1. Giới thiệu
1.1 Mục đích
Tài liệu mô tả yêu cầu cho chức năng khảo sát mức độ hài lòng của khách hàng (CSAT/NPS) trong SmartCRM: tạo mẫu khảo sát, gửi lời mời sau khi xử lý xong yêu cầu hỗ trợ, thu phản hồi và tổng hợp điểm.
1.2 Phạm vi
•	Trong phạm vi: tạo mẫu khảo sát; gửi và nhắc lời mời khảo sát qua email/SMS; khách hàng trả lời qua đường dẫn riêng; tính CSAT và NPS theo kỳ; xử lý phản hồi tiêu cực; xuất báo cáo CSV; từ chối nhận khảo sát.
•	Ngoài phạm vi (WON'T ở phiên bản này): gửi khảo sát qua Zalo/Messenger, khảo sát nhiều câu hỏi, phân tích cảm xúc tự động trên phần nhận xét.
1.3 Bảng thuật ngữ (mỗi khái niệm chỉ dùng MỘT tên, kể cả trong sơ đồ Use Case và API)
Thuật ngữ dùng trong tài liệu	Định nghĩa	Tên trong mã/API
Khảo sát	Một lần hỏi ý kiến khách hàng về mức hài lòng	survey
Mẫu khảo sát	Cấu hình câu hỏi: loại CSAT (thang 1–5) hoặc NPS (thang 0–10)	survey-template
Lời mời khảo sát	Bản ghi gửi một khảo sát cho một khách hàng, kèm đường dẫn riêng có hạn	survey-invitation
Phản hồi	Câu trả lời của khách hàng cho một lời mời khảo sát	response
Yêu cầu hỗ trợ	Một yêu cầu của khách hàng được nhân viên CSKH xử lý; khi hoàn tất sẽ kích hoạt khảo sát	support-request
CSAT	Tỷ lệ phản hồi chấm 4–5 trên tổng số phản hồi CSAT, tính theo %	–
NPS	% người ủng hộ (chấm 9–10) trừ % người chê (chấm 0–6) trên tổng số phản hồi NPS, giá trị từ −100 đến +100	–
Phản hồi tiêu cực	Phản hồi CSAT ≤ 2 hoặc NPS ≤ 6	–

2. Mô tả tổng quan
2.1 Tác nhân (actor)
Actor	Loại	Vai trò
Quản lý	Người	Tạo mẫu khảo sát, xem và xuất báo cáo
Nhân viên CSKH	Người	Gửi lời mời khảo sát, xử lý phản hồi tiêu cực
Khách hàng	Người	Trả lời hoặc từ chối khảo sát
Bộ lập lịch	Thời gian	Kích hoạt nhắc nhở và đánh dấu lời mời hết hạn
Dịch vụ Email/SMS	Hệ thống ngoài	Chuyển phát lời mời và nhắc nhở tới khách hàng
2.2 Môi trường và giả định
•	Ứng dụng web (React + Node.js/Express), cơ sở dữ liệu quan hệ.
•	Khách hàng trả lời trên điện thoại hoặc máy tính, không có tài khoản SmartCRM.
•	Mỗi yêu cầu hỗ trợ tối đa một lời mời khảo sát.
2.3 Use Case Diagram
Xem file gốc docs/diagrams/usecase-csat-nps.drawio: 5 actor, 8 use case.
Mã	Use case	Actor chính
UC1	Tạo mẫu khảo sát	Quản lý
UC2	Gửi lời mời khảo sát	Nhân viên CSKH
UC3	Gửi phản hồi khảo sát	Khách hàng
UC4	Xem báo cáo CSAT/NPS	Quản lý
UC5	Gửi nhắc nhở khảo sát	Bộ lập lịch
UC6	Xử lý phản hồi tiêu cực	Nhân viên CSKH
UC7	Xuất báo cáo ra CSV	Quản lý
UC8	Từ chối nhận khảo sát	Khách hàng

3. Yêu cầu chức năng
3.1 User Story (đã gán MoSCoW)
Mã	User Story	MoSCoW
US1	Là Quản lý, tôi muốn tạo mẫu khảo sát CSAT hoặc NPS để chuẩn hóa cách đo mức hài lòng của khách hàng.	MUST
US2	Là Nhân viên CSKH, tôi muốn gửi lời mời khảo sát cho khách ngay khi xử lý xong yêu cầu hỗ trợ để thu phản hồi khi trải nghiệm còn mới.	MUST
US3	Là Khách hàng, tôi muốn trả lời khảo sát qua một đường dẫn mà không cần đăng nhập để phản hồi trong chưa đầy một phút.	MUST
US4	Là Quản lý, tôi muốn xem điểm CSAT và NPS theo tuần hoặc tháng để đánh giá chất lượng dịch vụ theo thời gian.	SHOULD
US5	Là Quản lý, tôi muốn hệ thống tự nhắc khách chưa trả lời sau 3 ngày để tăng tỷ lệ phản hồi.	SHOULD
US6	Là Nhân viên CSKH, tôi muốn thấy danh sách phản hồi tiêu cực chưa xử lý để liên hệ lại khách kịp thời.	SHOULD
US7	Là Quản lý, tôi muốn xuất báo cáo CSAT/NPS ra CSV để gửi cho ban giám đốc.	COULD
US8	Là Khách hàng, tôi muốn từ chối nhận khảo sát về sau để không bị làm phiền.	COULD
Kiểm INVEST (I-N-V-E-S-T, ✓ = trả lời CÓ): US1 ✓✓✓✓✓✓ · US2 ✓✓✓✓✓✓ · US3 ✓✓✓✓✓✓ · US4 ✓✓✓✓✓✓ · US5 ✓✓✓✓✓✓ (phụ thuộc mềm vào US2, vẫn kiểm thử riêng được bằng dữ liệu mẫu) · US6 ✓✓✓✓✓✓ · US7 ✓✓✓✓✓✓ · US8 ✓✓✓✓✓✓.
3.2 Tiêu chí chấp nhận Given–When–Then (story MUST)
US1 – Tạo mẫu khảo sát
•	AC1.1 Given Quản lý đã đăng nhập, When nhập tên "CSAT sau hỗ trợ", chọn loại CSAT và lưu, Then hệ thống tạo mẫu khảo sát và hiển thị trong danh sách.
•	AC1.2 (ngoại lệ) Given đã có mẫu tên "CSAT sau hỗ trợ", When Quản lý lưu mẫu khác cùng tên, Then hệ thống từ chối và báo "Tên mẫu đã tồn tại".
US2 – Gửi lời mời khảo sát
•	AC2.1 Given yêu cầu hỗ trợ ở trạng thái "Hoàn tất" và khách có email, When Nhân viên CSKH chọn mẫu khảo sát và bấm gửi, Then hệ thống tạo lời mời khảo sát và chuyển cho Dịch vụ Email/SMS trong vòng 60 giây.
•	AC2.2 Given lời mời vừa tạo, When xem chi tiết, Then đường dẫn riêng có hạn 14 ngày kể từ lúc gửi.
•	AC2.3 (ngoại lệ) Given yêu cầu hỗ trợ đã có lời mời khảo sát, When Nhân viên CSKH gửi thêm lần nữa, Then hệ thống từ chối và báo "Yêu cầu hỗ trợ này đã được gửi khảo sát".
US3 – Gửi phản hồi khảo sát
•	AC3.1 Given lời mời còn hạn và chưa trả lời, When khách mở đường dẫn, chọn điểm và bấm gửi, Then hệ thống lưu phản hồi và hiển thị lời cảm ơn.
•	AC3.2 (ngoại lệ) Given lời mời đã trả lời, When khách mở lại đường dẫn, Then hệ thống không cho trả lời lần hai và báo "Bạn đã gửi phản hồi".
•	AC3.3 (ngoại lệ) Given lời mời quá 14 ngày, When khách mở đường dẫn, Then hệ thống báo "Khảo sát đã hết hạn" và không hiển thị form.
3.3 Danh sách yêu cầu chức năng (FR)
Mã	Yêu cầu	Ưu tiên
FR1	Hệ thống cho Quản lý tạo mẫu khảo sát gồm tên, loại (CSAT thang 1–5 hoặc NPS thang 0–10), nội dung câu hỏi và một ô nhận xét tùy chọn; tên mẫu không trùng.	MUST
FR2	Hệ thống tạo lời mời khảo sát gắn với một yêu cầu hỗ trợ và một khách hàng, sinh đường dẫn riêng hết hạn sau 14 ngày, rồi chuyển cho Dịch vụ Email/SMS.	MUST
FR3	Hệ thống hiển thị form khảo sát theo đường dẫn riêng mà không yêu cầu đăng nhập.	MUST
FR4	Hệ thống ghi nhận phản hồi; mỗi lời mời chỉ nhận một phản hồi; từ chối lời mời đã dùng hoặc hết hạn.	MUST
FR5	Hệ thống tính CSAT và NPS theo tuần hoặc tháng và hiển thị số phản hồi kèm theo.	SHOULD
FR6	Hệ thống gửi nhắc nhở một lần sau 3 ngày cho lời mời chưa trả lời.	SHOULD
FR7	Hệ thống gắn cờ phản hồi tiêu cực, liệt kê cho Nhân viên CSKH và cho ghi chú trạng thái xử lý.	SHOULD
FR8	Hệ thống xuất báo cáo CSAT/NPS theo kỳ ra tệp CSV.	COULD
FR9	Hệ thống ghi nhận khách từ chối và không gửi thêm lời mời hoặc nhắc nhở cho khách đó.	COULD
3.4 Đặc tả chi tiết use case quan trọng nhất – UC3 Gửi phản hồi khảo sát
Mục	Nội dung
Actor	Khách hàng
Mục tiêu	Khách hàng chấm điểm và gửi phản hồi cho một yêu cầu hỗ trợ đã hoàn tất
Điều kiện trước	Khách đã nhận lời mời khảo sát; lời mời chưa trả lời và chưa quá 14 ngày
Điều kiện sau (thành công)	Phản hồi được lưu; lời mời chuyển sang trạng thái "Đã trả lời"; phản hồi tiêu cực được gắn cờ
Luồng chính
1.	Khách mở đường dẫn riêng trong email/SMS.
2.	Hệ thống kiểm tra đường dẫn hợp lệ, chưa hết hạn, chưa trả lời.
3.	Hệ thống hiển thị câu hỏi của mẫu khảo sát (thang 1–5 hoặc 0–10) và ô nhận xét tùy chọn.
4.	Khách chọn điểm.
5.	Khách nhập nhận xét (tùy chọn) và bấm "Gửi".
6.	Hệ thống kiểm tra điểm hợp lệ.
7.	Hệ thống lưu phản hồi, đổi trạng thái lời mời, gắn cờ nếu là phản hồi tiêu cực.
8.	Hệ thống hiển thị lời cảm ơn.
Luồng ngoại lệ (đánh số theo bước)
•	2a Đường dẫn không tồn tại: hệ thống báo "Đường dẫn không hợp lệ", kết thúc.
•	2b Lời mời quá 14 ngày: hệ thống báo "Khảo sát đã hết hạn", kết thúc.
•	2c Lời mời đã có phản hồi: hệ thống báo "Bạn đã gửi phản hồi", kết thúc.
•	6a Khách chưa chọn điểm hoặc điểm ngoài thang: hệ thống báo lỗi tại ô điểm và quay lại bước 4.
•	7a Lỗi lưu dữ liệu: hệ thống báo "Gửi chưa thành công, vui lòng thử lại" và giữ nguyên điểm đã chọn; quay lại bước 5.
4. Yêu cầu phi chức năng (có ngưỡng số)
Mã	Nhóm	Yêu cầu	Ngưỡng đo
NFR1	Hiệu năng	Trang form khảo sát tải xong	≤ 2 giây ở phân vị 95 với 100 khách truy cập đồng thời
NFR2	Hiệu năng	Lời mời được chuyển cho Dịch vụ Email/SMS sau khi Nhân viên CSKH bấm gửi	≤ 60 giây
NFR3	Hiệu năng	Tính CSAT và NPS cho 10.000 phản hồi	≤ 3 giây
NFR4	Bảo mật	Đường dẫn riêng khó đoán	token ngẫu nhiên ≥ 128 bit, hết hạn sau 14 ngày
NFR5	Khả dụng	Hệ thống hoạt động trong giờ hành chính (8:00–18:00, thứ 2–thứ 6)	≥ 99% thời gian mỗi tháng
NFR6	Tương thích	Form khảo sát dùng được trên điện thoại	màn hình rộng từ 360 px, hoàn tất trong ≤ 3 lần chạm

5. Ràng buộc và giao diện ngoài
•	Dịch vụ Email/SMS: giao tiếp qua API của nhà cung cấp; lỗi chuyển phát phải được ghi log để gửi lại.
•	Dữ liệu cá nhân: chỉ lưu tên, email, số điện thoại của khách ở mức cần thiết; khách có quyền từ chối nhận khảo sát (FR9).
•	Công nghệ: React (frontend), Node.js/Express (backend), cơ sở dữ liệu quan hệ; API dạng REST/JSON, xem docs/api-contract.md.
•	Quy ước đặt tên: dùng đúng các thuật ngữ ở mục 1.3.
6. Bảng truy vết
FR	User Story	Use Case	MoSCoW
FR1	US1	UC1 Tạo mẫu khảo sát	MUST
FR2	US2	UC2 Gửi lời mời khảo sát	MUST
FR3	US3	UC3 Gửi phản hồi khảo sát	MUST
FR4	US3	UC3 Gửi phản hồi khảo sát	MUST
FR5	US4	UC4 Xem báo cáo CSAT/NPS	SHOULD
FR6	US5	UC5 Gửi nhắc nhở khảo sát	SHOULD
FR7	US6	UC6 Xử lý phản hồi tiêu cực	SHOULD
FR8	US7	UC7 Xuất báo cáo ra CSV	COULD
FR9	US8	UC8 Từ chối nhận khảo sát	COULD

  [Use Case Diagram](docs/docs/Untitled Diagram.drawio.png)