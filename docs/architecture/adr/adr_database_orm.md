# ADR 002: Lựa chọn PostgreSQL và Prisma ORM

## Trạng thái
Đã chấp nhận (Accepted)

## Bối cảnh
Hệ thống BrewLite cần lưu trữ dữ liệu có tính ràng buộc cao về mặt giao dịch (tài khoản người dùng, sản phẩm, giỏ hàng, đơn hàng, thanh toán và điểm thưởng). Nhóm cần chọn hệ quản trị cơ sở dữ liệu và công cụ tương tác phù hợp với NestJS.

## Quyết định
Sử dụng **PostgreSQL** làm cơ sở dữ liệu quan hệ chính kết hợp với **Prisma ORM** để quản lý schema và thực hiện migration.

## Hệ quả (Trade-offs)
*   **Thuận lợi:**
    *   PostgreSQL hỗ trợ đầy đủ các tính năng ACID, đảm bảo an toàn tuyệt đối cho các giao dịch đặt hàng và thanh toán đồng thời.
    *   Prisma cung cấp giao diện quản lý schema trực quan, tự động sinh TypeScript types giúp giảm thiểu lỗi code và tăng tốc độ phát triển API.
*   **Bất lợi:**
    *   Yêu cầu cấu hình Docker container cho PostgreSQL ở môi trường local, tuy nhiên điều này đã được giải quyết trọn gói trong tệp `docker-compose.yml`.