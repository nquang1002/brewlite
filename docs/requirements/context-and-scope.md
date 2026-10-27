# Bối cảnh và phạm vi BrewLite

## 1. Vấn đề nghiệp vụ

Tại quán cà phê, khách phải xếp hàng gọi món và trả tiền mặt, làm tăng thời gian
chờ và khó theo dõi đơn. BrewLite là ứng dụng web (mobile-first/PWA) cho phép
khách xem menu, chọn size/topping, đặt đơn, thanh toán không tiền mặt và theo
dõi trạng thái đơn.

**Product Vision:** BrewLite giúp khách hàng đặt và thanh toán đồ uống không dùng
tiền mặt chỉ trong vài chạm, giảm thời gian xếp hàng và nhận đơn nhanh tại quầy.

## 2. Stakeholder

| Stakeholder            | Vai trò                      | Mục tiêu                       | Kỳ vọng                                                             |
| ---------------------- | ---------------------------- | ------------------------------ | ------------------------------------------------------------------- |
| Khách hàng             | Người đặt đồ uống            | Đặt nhanh, khỏi xếp hàng       | UI dễ dùng trên điện thoại, thanh toán an toàn, thấy trạng thái đơn |
| Nhân viên quầy/Barista | Tiếp nhận và pha chế         | Nhận đơn rõ ràng, không nhầm   | Xem danh sách đơn mới, cập nhật được trạng thái                     |
| Quản lý cửa hàng / PO  | Xác định yêu cầu, nghiệm thu | Tăng doanh thu, giảm tải quầy  | Menu, tồn kho, khuyến mãi quản lý được; sản phẩm đúng ưu tiên       |
| Giảng viên nghiệm thu  | Đánh giá                     | Kiểm tra quy trình và sản phẩm | Tài liệu, lịch sử commit, CI/CD, URL production hoạt động           |
| Nhóm phát triển        | Xây dựng                     | Hoàn thành đúng hạn            | Yêu cầu rõ, phạm vi vừa sức, tiêu chí kiểm thử được                 |
| Đơn vị thanh toán      | Xử lý giao dịch (mô phỏng)   | Giao dịch đúng, không trùng    | Request hợp lệ, có Idempotency-Key                                  |

## 3. Phạm vi

### 3.1 In-scope (MVP)

- Menu, chi tiết sản phẩm, chọn size và topping
- Giỏ hàng, tự tính tổng tiền
- Đăng ký, đăng nhập (JWT)
- Tạo đơn, kiểm tra tồn kho bằng transaction
- Thanh toán Ví/Thẻ mô phỏng, idempotent
- Xác nhận, trạng thái và lịch sử đơn
- Triển khai production, CI/CD, health check

### 3.2 Phạm vi mở rộng (làm nếu còn thời gian)

- Mã khuyến mãi (Promotion), tích điểm (LoyaltyTransaction)
- Nhân viên cập nhật trạng thái đơn (PREPARING, READY, COMPLETED)
- Quên mật khẩu

### 3.3 Out-of-scope

- Thanh toán thật (Momo/VNPay/Stripe)
- Giao hàng tận nơi, app native
- Quản lý nhân sự, báo cáo doanh thu nâng cao

## 4. Giả định

- Một cửa hàng duy nhất, chỉ nhận đơn nhận tại quầy.
- Cổng thanh toán được mô phỏng, luôn phản hồi trong vài giây.
- Khách có điện thoại/trình duyệt hiện đại và kết nối Internet.

## 5. Ràng buộc

- Bắt buộc dùng Next.js, NestJS, PostgreSQL; nhóm chọn thêm Prisma, Docker.
- Làm tuần tự 5 task, thời hạn trong học kỳ I 2026–2027.
- Secret không được đưa vào repository.
- Nền tảng triển khai: <điền nền tảng nhóm chọn>.

## 6. Rủi ro ban đầu

| Rủi ro                              | Mức độ     | Biện pháp                        |
| ----------------------------------- | ---------- | -------------------------------- |
| Bán quá tồn kho khi đặt đồng thời   | Cao        | Transaction + optimistic locking |
| Thanh toán trùng khi client gửi lại | Cao        | Idempotency-Key                  |
| Chậm tiến độ do phạm vi lớn         | Trung bình | Ưu tiên MoSCoW, chia 2 Sprint    |
| Lỗi triển khai production           | Trung bình | Smoke test, hướng dẫn rollback   |
| Rò rỉ secret                        | Cao        | .gitignore, CI secrets           |
