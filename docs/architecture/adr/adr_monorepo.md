# ADR 001: Lựa chọn cấu trúc Monorepo cho dự án BrewLite

## Trạng thái
Đã chấp nhận (Accepted)

## Bối cảnh
Dự án BrewLite bao gồm hai thành phần phần mềm chính: ứng dụng phía client (Frontend bằng Next.js) và ứng dụng phía server (Backend bằng NestJS). Nhóm cần quyết định xem nên tách thành hai repository riêng biệt hay gộp chung vào một repository theo cấu trúc Monorepo.

## Quyết định
Nhóm quyết định sử dụng cấu trúc **Monorepo** với thư mục gốc quản lý cả hai phân vùng `apps/web` và `apps/api`, kết hợp thư mục tài liệu chung `docs/`.

## Hệ quả (Trade-offs)
*   **Thuận lợi:**
    *   Dễ dàng theo dõi tiến độ toàn diện của đồ án qua một kho mã duy nhất.
    *   Thuận tiện cho việc cấu hình các công cụ tự động hóa CI/CD, kiểm thử toàn trình (E2E) và quản lý biến môi trường mẫu (`.env.example`).
    *   Hạn chế tình trạng lệch phiên bản dữ liệu/API giữa client và server trong quá trình phát triển Sprint 1 và Sprint 2.
*   **Bất lợi:**
    *   Kích thước repository lớn hơn một chút, đòi hỏi quy ước commit và phân nhánh (Branching strategy) rõ ràng giữa các thành viên trong nhóm.