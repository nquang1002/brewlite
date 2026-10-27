# Tài liệu Thiết kế Kiến trúc Hệ thống - BrewLite

## 1. Giới thiệu tổng quan
Hệ thống **BrewLite** được thiết kế theo mô hình kiến trúc phân lớp (Layered Architecture) kết hợp với cấu trúc Monorepo (quản lý cả Frontend và Backend trong cùng một kho mã nguồn). Mô hình này đảm bảo tính tách biệt rõ ràng giữa các thành phần giao diện, logic nghiệp vụ, và lưu trữ dữ liệu, đồng thời tối ưu hóa quá trình phát triển, kiểm thử và triển khai (CI/CD).

---

## 2. Mô hình kiến trúc C4 (Cấp độ 1 & 2)

### 2.1. Sơ đồ Ngữ cảnh (System Context)
*   **Khách hàng (User/Customer):** Tương tác qua trình duyệt web trên thiết bị di động hoặc máy tính (PWA/SPA) để xem thực đơn, quản lý giỏ hàng, đặt đơn và thanh toán trực tuyến.
*   **Nhân viên / Quản lý (Staff/Admin):** Theo dõi đơn hàng và cấu hình hệ thống (ngoài phạm vi MVP cốt lõi).
*   **Hệ thống BrewLite:** Xử lý toàn bộ logic nghiệp vụ đặt cà phê, xác thực, quản lý kho và thanh toán.
*   **Cổng thanh toán giả lập (Mock Payment Gateway):** Mô phỏng các dịch vụ thanh toán trực tuyến (Momo, VNPay, Thẻ ngân hàng) để xử lý giao dịch không tiền mặt.

### 2.2. Sơ đồ Thành phần (Container Diagram)
*   **Frontend App (Next.js):** Ứng dụng Web Client được xây dựng bằng Next.js (React/TypeScript) hỗ trợ Server-Side Rendering (SSR) và Client-Side Rendering (CSR), thiết kế theo định hướng *mobile-first*.
*   **Backend API (NestJS):** Ứng dụng RESTful API được xây dựng theo mô hình Module/Domain-driven trên nền tảng NestJS (TypeScript), chịu trách nhiệm xử lý nghiệp vụ, phân quyền JWT, và giao tiếp với cơ sở dữ liệu.
*   **Database (PostgreSQL):** Hệ quản trị cơ sở dữ liệu quan hệ lưu trữ thông tin Người dùng, Sản phẩm, Đơn hàng, Mục đơn hàng, Thanh toán và Khuyến mãi.
*   **ORM (Prisma):** Công cụ ánh xạ quan hệ đối tượng (Object-Relational Mapping) giúp thao tác với cơ sở dữ liệu an toàn kiểu dữ liệu (Type-safe) và quản lý migration hiệu quả.

---

## 3. Quyết định công nghệ và Chi tiết thành phần

| Thành phần | Công nghệ lựa chọn | Lý do và Mục đích |
| :--- | :--- | :--- |
| **Frontend** | Next.js (React, TypeScript), TailwindCSS | Hỗ trợ SEO tốt (SSR), hiệu năng cao, tối ưu hóa giao diện di động (Mobile-first) và phát triển component nhanh chóng. |
| **Backend** | NestJS (TypeScript), REST, Passport (JWT) | Kiến trúc module sẵn có giúp mã nguồn có tổ chức, dễ mở rộng, bảo mật mạnh mẽ với class-validator và JWT Guards. |
| **Database & ORM** | PostgreSQL + Prisma | PostgreSQL đảm bảo tính toàn vẹn dữ liệu (ACID) cho giao dịch tài chính/đơn hàng; Prisma cung cấp migration mượt mà và type-safety tuyệt đối. |
| **Thanh toán** | Mock Payment Service | Mô phỏng cổng thanh toán thực tế, tích hợp cơ chế chống trùng lặp giao dịch bằng `Idempotency-Key`. |
| **DevOps & Logging** | Docker Compose, Git, Winston/Built-in Logger | Đảm bảo môi trường chạy đồng bộ giữa local và production, quản lý cấu hình qua biến môi trường (`.env`). |

---

## 4. Ưu điểm, Hạn chế và Trade-offs

### 4.1. Ưu điểm
*   **Tính mô-đun cao:** Cả Frontend và Backend đều được tổ chức rõ ràng, dễ dàng bảo trì và phân công công việc trong nhóm.
*   **Đồng nhất ngôn ngữ:** Sử dụng TypeScript cho cả Frontend và NestJS giúp chia sẻ kiểu dữ liệu (types) và giảm thiểu lỗi runtime.
*   **Khả năng mở rộng:** Dễ dàng mở rộng thêm các vi dịch vụ (Microservices) hoặc tính năng mới trong các phiên bản tiếp theo.

### 4.2. Hạn chế & Trade-offs
*   **Monorepo Overhead:** Quản lý chung một kho mã có thể làm tăng nhẹ độ phức tạp khi cấu hình CI/CD ban đầu. Tuy nhiên, lợi ích mang lại về tính đồng bộ mã nguồn hoàn toàn vượt trội so với việc tách repo.
*   **Ràng buộc kết nối:** Phụ thuộc vào tốc độ phản hồi của CSDL quan hệ, tuy nhiên được tối ưu hóa bằng các chỉ mục (indexes) và transaction chặt chẽ.