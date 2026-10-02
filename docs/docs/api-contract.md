API contract – Khảo sát hài lòng CSAT / NPS (Track SE)
Phạm vi: các story MUST (US1, US2, US3). Định dạng JSON, mã hóa UTF-8. Dữ liệu mẫu là dữ liệu của case study và có thể thay bằng dữ liệu thật của đơn vị.
1. Danh sách endpoint
Phương thức	Đường dẫn	Mục đích	Truy vết
POST	/api/survey-templates	Tạo mẫu khảo sát	US1 / FR1
POST	/api/survey-invitations	Tạo và gửi lời mời khảo sát	US2 / FR2
GET	/api/public/surveys/{token}	Lấy nội dung khảo sát theo đường dẫn riêng	US3 / FR3
POST	/api/public/surveys/{token}/responses	Gửi phản hồi khảo sát	US3 / FR4
Hai endpoint đầu cần đăng nhập (Quản lý, Nhân viên CSKH); hai endpoint /api/public/... không cần đăng nhập và được bảo vệ bằng token.
2. Chi tiết endpoint
2.1 POST /api/survey-templates (US1)
Request:
json
{
  "name": "CSAT sau hỗ trợ",
  "type": "CSAT",
  "question": "Bạn hài lòng thế nào về lần hỗ trợ vừa rồi?",
  "allowComment": true
}
Response 201 Created:
json
{
  "id": 7,
  "name": "CSAT sau hỗ trợ",
  "type": "CSAT",
  "scaleMin": 1,
  "scaleMax": 5,
  "question": "Bạn hài lòng thế nào về lần hỗ trợ vừa rồi?",
  "allowComment": true,
  "createdAt": "2026-10-02T09:15:00+07:00"
}
Mã	Khi nào	Body mẫu
201	Tạo thành công	như trên
400	Thiếu hoặc sai trường	{"code":"VALIDATION_ERROR","errors":[{"field":"type","message":"Chỉ nhận CSAT hoặc NPS"}]}
409	Trùng tên mẫu	{"code":"TEMPLATE_NAME_EXISTS","message":"Tên mẫu đã tồn tại"}
2.2 POST /api/survey-invitations (US2)
Request:
json
{
  "supportRequestId": "YC-20261002-014",
  "customerId": 1052,
  "templateId": 7,
  "channel": "EMAIL"
}
Response 201 Created:
json
{
  "id": 3301,
  "supportRequestId": "YC-20261002-014",
  "customer": { "id": 1052, "name": "Nguyễn Thị Lan", "email": "lan.nguyen@example.com" },
  "templateId": 7,
  "channel": "EMAIL",
  "status": "SENT",
  "link": "https://smartcrm.example.com/survey/q3Z8xK1mP0aVb7YtR2dWnC5uHs9LfJ4e",
  "sentAt": "2026-10-02T09:20:00+07:00",
  "expiresAt": "2026-10-16T09:20:00+07:00"
}
Mã	Khi nào	Body mẫu
201	Tạo và chuyển cho Dịch vụ Email/SMS thành công	như trên
400	Thiếu trường, channel không phải EMAIL/SMS, yêu cầu hỗ trợ chưa "Hoàn tất", khách thiếu email (EMAIL) hoặc thiếu số điện thoại (SMS)	{"code":"VALIDATION_ERROR","errors":[{"field":"customerId","message":"Khách hàng chưa có email"}]}
404	Không tìm thấy khách hàng, mẫu khảo sát hoặc yêu cầu hỗ trợ	{"code":"NOT_FOUND","message":"Không tìm thấy mẫu khảo sát"}
409	Yêu cầu hỗ trợ đã có lời mời; hoặc khách đã từ chối nhận khảo sát	{"code":"INVITATION_EXISTS","message":"Yêu cầu hỗ trợ này đã được gửi khảo sát"}
2.3 GET /api/public/surveys/{token} (US3)
Response 200 OK:
json
{
  "templateName": "CSAT sau hỗ trợ",
  "type": "CSAT",
  "scaleMin": 1,
  "scaleMax": 5,
  "question": "Bạn hài lòng thế nào về lần hỗ trợ vừa rồi?",
  "allowComment": true,
  "expiresAt": "2026-10-16T09:20:00+07:00"
}
Mã	Khi nào	Body mẫu
200	Token hợp lệ, còn hạn, chưa trả lời	như trên
404	Token không tồn tại	{"code":"NOT_FOUND","message":"Đường dẫn không hợp lệ"}
409	Lời mời hết hạn hoặc đã trả lời	{"code":"EXPIRED","message":"Khảo sát đã hết hạn"} hoặc {"code":"ALREADY_ANSWERED","message":"Bạn đã gửi phản hồi"}
2.4 POST /api/public/surveys/{token}/responses (US3)
Request:
json
{
  "score": 2,
  "comment": "Nhân viên đến trễ hai ngày so với hẹn."
}
Response 201 Created:
json
{
  "id": 8120,
  "invitationId": 3301,
  "score": 2,
  "comment": "Nhân viên đến trễ hai ngày so với hẹn.",
  "flaggedNegative": true,
  "submittedAt": "2026-10-03T20:41:00+07:00"
}
Mã	Khi nào	Body mẫu
201	Lưu phản hồi thành công	như trên
400	Thiếu score hoặc điểm ngoài thang của mẫu	{"code":"VALIDATION_ERROR","errors":[{"field":"score","message":"Điểm phải từ 1 đến 5"}]}
404	Token không tồn tại	{"code":"NOT_FOUND","message":"Đường dẫn không hợp lệ"}
409	Lời mời hết hạn hoặc đã trả lời	{"code":"ALREADY_ANSWERED","message":"Bạn đã gửi phản hồi"}
3. Quy tắc validation
Endpoint	Trường	Bắt buộc	Kiểu	Độ dài / Dải giá trị
POST survey-templates	name	Có	string	3–100 ký tự, không trùng
	type	Có	string	CSAT hoặc NPS
	question	Có	string	10–300 ký tự
	allowComment	Không	boolean	mặc định true
POST survey-invitations	supportRequestId	Có	string	khớp yêu cầu hỗ trợ ở trạng thái "Hoàn tất"
	customerId	Có	integer	> 0, khách tồn tại
	templateId	Có	integer	> 0, mẫu tồn tại
	channel	Có	string	EMAIL hoặc SMS
GET public/surveys/{token}	token	Có	string	đúng 32 ký tự chữ-số
POST public/.../responses	score	Có	integer	CSAT: 1–5; NPS: 0–10
	comment	Không	string	0–500 ký tự
4. Tự kiểm truy vết
Mỗi endpoint trong mục 1 đều có một User Story ở cột "Truy vết" và khớp bảng truy vết mục 6 của docs/srs.md.
