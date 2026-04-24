Bạn đang làm việc trong workspace chung của 2 project:

- Frontend Angular: D:\Spring\ttl-project\ecommerce-ng
- Backend Spring Boot: D:\Spring\ttl-project\ecommerce-shop

Mục tiêu của bạn:
- Đọc đúng rule AI của cả frontend và backend trước khi sửa
- Phân tích ảnh hưởng end-to-end giữa frontend và backend
- Chọn thay đổi nhỏ nhất nhưng đúng
- Sau mỗi lần thay đổi phải ghi lại tiến trình để team/AI khác follow tiếp được

Quy trình bắt buộc phải tuân thủ:

1. Đọc rule và tài liệu AI trước khi làm
Frontend:
- D:\Spring\ttl-project\ecommerce-ng\AI\README.md
- D:\Spring\ttl-project\ecommerce-ng\AI\instructions\angular-coding-standards.md
- D:\Spring\ttl-project\ecommerce-ng\AI\instructions\change-workflow.md
- Các file trong `AI/references/` nếu task liên quan auth, routes, API, navigation

Backend:
- D:\Spring\ttl-project\ecommerce-shop\.ai\AI_CONFIGURATION_GUIDE.md
- D:\Spring\ttl-project\ecommerce-shop\.ai\BASELINE.md
- D:\Spring\ttl-project\ecommerce-shop\.ai\instructions\java-backend.instructions.md
- D:\Spring\ttl-project\ecommerce-shop\.ai\instructions\java-testing.instructions.md
- D:\Spring\ttl-project\ecommerce-shop\.ai\instructions\docs-sync.instructions.md nếu thay đổi API, behavior, config, docs

Shared progress:
- D:\Spring\ttl-project\AI_PROGRESS.md

2. Trước khi sửa code
- Tóm tắt lại yêu cầu thật ngắn
- Xác định task thuộc:
  - frontend
  - backend
  - hay shared frontend-backend
- Tìm các file liên quan trước khi sửa
- Không giả định contract API nếu chưa đọc code hoặc docs hiện tại

3. Nguyên tắc triển khai
- Ưu tiên thay đổi nhỏ nhất đúng với convention hiện có
- Không thêm abstraction nếu chưa cần
- Không đổi kiến trúc rộng nếu task không yêu cầu
- Giữ controller backend mỏng, business logic ở service
- Không expose entity trực tiếp từ backend
- Frontend phải đi đúng flow auth/state/interceptor hiện có
- Khi thay đổi contract API, phải kiểm tra ảnh hưởng cả 2 phía

4. Khi task liên quan frontend-backend
- Xác định rõ:
  - endpoint nào đang dùng
  - request/response contract hiện tại
  - frontend đang map dữ liệu ra sao
  - backend đang trả dữ liệu ra sao
- Nếu có lệch contract, sửa theo hướng rõ ràng, ổn định, dễ maintain
- Nếu cần endpoint mới, mô tả rõ:
  - URL
  - method
  - auth requirement
  - response shape
  - frontend mapping point

5. Kiểm tra sau khi sửa
- Frontend: build/test phần bị ảnh hưởng nếu khả thi
- Backend: test/build phần bị ảnh hưởng nếu khả thi
- Nếu không chạy được, nêu rõ lý do
- Tóm tắt chính xác file nào đã đổi và vì sao

6. Quy tắc log tiến trình bắt buộc
Sau mỗi change set, append 1 entry mới vào:
- D:\Spring\ttl-project\AI_PROGRESS.md

Mỗi entry phải có:
- thời gian
- scope: frontend | backend | shared
- danh sách file đã đổi
- tóm tắt thay đổi
- verification
- todo tiếp theo
- notes/handoff nếu có

Không xóa log cũ. Chỉ append.

7. Cách trả lời
- Không nói dài dòng
- Làm xong thì báo:
  - đã sửa gì
  - ở file nào
  - đã verify gì
  - còn todo gì
- Nếu có rủi ro hoặc điểm chưa chắc, nêu rõ ngay

Bây giờ hãy thực hiện task sau:
