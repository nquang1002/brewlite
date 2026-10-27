# Đặc tả yêu cầu phần mềm (SRS rút gọn) - BrewLite

## 1. Quy ước

- Mã yêu cầu: `FR-xx` (chức năng), `NFR-xx` (phi chức năng). Không đổi mã sau khi đã commit.
- Mức ưu tiên MoSCoW: Must / Should / Could / Won't (cho phiên bản này).
- Trạng thái đơn: PENDING, PAID, PAYMENT_FAILED, PREPARING, READY, COMPLETED, CANCELLED.
- Actor: Khách hàng (Customer), Nhân viên quầy (Staff), Payment Service.

## 2. Yêu cầu chức năng

### 2.1 Menu

| Mã    | Mô tả                                              | MoSCoW | Tiêu chí chấp nhận                                                                                                   |
| ----- | -------------------------------------------------- | ------ | -------------------------------------------------------------------------------------------------------------------- |
| FR-01 | Hiển thị danh sách đồ uống gồm hình, tên, giá      | Must   | `GET /products` trả 200 và mảng sản phẩm có imageUrl, name, price; UI hiển thị đủ các sản phẩm seed                  |
| FR-02 | Hiển thị trạng thái loading, empty, error của menu | Must   | Khi API chậm có loading; khi danh sách rỗng có thông báo "Chưa có sản phẩm"; khi API lỗi có thông báo và nút thử lại |
| FR-03 | Lọc menu theo danh mục                             | Could  | Chọn danh mục thì chỉ hiện sản phẩm thuộc danh mục đó                                                                |

### 2.2 Chi tiết sản phẩm

| Mã    | Mô tả                        | MoSCoW | Tiêu chí chấp nhận                                                                       |
| ----- | ---------------------------- | ------ | ---------------------------------------------------------------------------------------- |
| FR-04 | Xem chi tiết một sản phẩm    | Must   | `GET /products/:id` trả 200 với dữ liệu đúng; id không tồn tại trả 404                   |
| FR-05 | Chọn size (S/M/L) và topping | Must   | Đổi size/topping thì giá hiển thị cập nhật đúng theo bảng giá; có thể chọn nhiều topping |

### 2.3 Giỏ hàng

| Mã    | Mô tả                                               | MoSCoW | Tiêu chí chấp nhận                                                                  |
| ----- | --------------------------------------------------- | ------ | ----------------------------------------------------------------------------------- |
| FR-06 | Thêm, sửa số lượng, xóa món trong giỏ               | Must   | Thêm món thì badge số lượng tăng; sửa/xóa thì giỏ cập nhật; số lượng tối thiểu là 1 |
| FR-07 | Tự tính tổng tiền giỏ hàng                          | Must   | Tổng = Σ (đơn giá theo size/topping × số lượng); kiểm chứng bằng unit test          |
| FR-08 | Giữ giỏ hàng khi chưa đăng nhập hoặc thanh toán lỗi | Should | Sau khi đăng nhập hoặc thanh toán lỗi, giỏ hàng vẫn còn nguyên các món              |

### 2.4 Xác thực

| Mã    | Mô tả                                    | MoSCoW | Tiêu chí chấp nhận                                                                       |
| ----- | ---------------------------------------- | ------ | ---------------------------------------------------------------------------------------- |
| FR-09 | Đăng ký tài khoản bằng email và mật khẩu | Must   | `POST /auth/register` trả 201; email trùng trả 409; email/mật khẩu sai định dạng trả 400 |
| FR-10 | Đăng nhập và nhận JWT                    | Must   | `POST /auth/login` đúng thông tin trả 200 kèm token; sai trả 401                         |
| FR-11 | Khôi phục mật khẩu                       | Could  | Có luồng yêu cầu đặt lại mật khẩu qua email giả lập                                      |

### 2.5 Đặt đơn

| Mã    | Mô tả                                   | MoSCoW | Tiêu chí chấp nhận                                                                                         |
| ----- | --------------------------------------- | ------ | ---------------------------------------------------------------------------------------------------------- |
| FR-12 | Tạo đơn từ giỏ hàng, trạng thái PENDING | Must   | `POST /orders` có JWT hợp lệ trả 201, đơn gồm OrderItem và total đúng; thiếu JWT trả 401; giỏ rỗng trả 400 |
| FR-13 | Kiểm tra và trừ tồn kho khi tạo đơn     | Must   | Món hết hàng thì trả lỗi và không tạo đơn; đặt đồng thời không làm tồn kho âm (có test)                    |
| FR-14 | Hủy đơn khi chưa chuẩn bị               | Should | Đơn PENDING/PAYMENT_FAILED hủy được thành CANCELLED và hoàn tồn kho; đơn PREPARING trở đi bị từ chối       |

### 2.6 Thanh toán

| Mã    | Mô tả                                          | MoSCoW | Tiêu chí chấp nhận                                                                              |
| ----- | ---------------------------------------------- | ------ | ----------------------------------------------------------------------------------------------- |
| FR-15 | Thanh toán không tiền mặt bằng Ví/Thẻ mô phỏng | Must   | `POST /payments` thành công thì Payment ghi nhận và đơn chuyển PAID                             |
| FR-16 | Chống thanh toán trùng bằng Idempotency-Key    | Must   | Gửi 2 lần cùng Idempotency-Key chỉ tạo 1 Payment và trả cùng kết quả                            |
| FR-17 | Xử lý thanh toán thất bại và cho thử lại       | Must   | Payment Service từ chối thì đơn thành PAYMENT_FAILED, giỏ giữ nguyên, khách thanh toán lại được |
| FR-18 | Áp mã khuyến mãi                               | Could  | Mã hợp lệ làm giảm total đúng; mã hết hạn/không hợp lệ bị từ chối                               |
| FR-19 | Cộng điểm tích lũy sau khi đơn PAID            | Could  | Sau PAID tạo LoyaltyTransaction và cập nhật điểm của User, chỉ cộng một lần cho mỗi đơn         |

### 2.7 Lịch sử đơn

| Mã    | Mô tả                                                 | MoSCoW | Tiêu chí chấp nhận                                                                                 |
| ----- | ----------------------------------------------------- | ------ | -------------------------------------------------------------------------------------------------- |
| FR-20 | Hiển thị xác nhận đơn (mã đơn, trạng thái, tổng tiền) | Must   | Sau thanh toán thành công UI hiện mã đơn, trạng thái PAID và tổng tiền                             |
| FR-21 | Xem lịch sử đơn của chính mình                        | Must   | `GET /orders/me` chỉ trả đơn của user trong JWT, sắp xếp mới nhất trước; không thấy đơn người khác |

### 2.8 Quản lý trạng thái đơn

| Mã    | Mô tả                                                 | MoSCoW | Tiêu chí chấp nhận                                                                                     |
| ----- | ----------------------------------------------------- | ------ | ------------------------------------------------------------------------------------------------------ |
| FR-22 | Khách xem trạng thái đơn hiện tại                     | Must   | Trang chi tiết đơn hiển thị đúng trạng thái hiện tại của đơn                                           |
| FR-23 | Chỉ cho phép chuyển trạng thái hợp lệ (state machine) | Must   | PENDING → COMPLETED bị chặn và trả lỗi; chuỗi PENDING → PAID → PREPARING → READY → COMPLETED chạy được |
| FR-24 | Nhân viên cập nhật trạng thái chuẩn bị đơn            | Should | `PATCH /orders/:id/status` bởi Staff chuyển PAID → PREPARING → READY → COMPLETED; Customer gọi trả 403 |

### 2.9 Ngoài phạm vi (Won't)

| Mã    | Mô tả                                 | MoSCoW | Ghi chú           |
| ----- | ------------------------------------- | ------ | ----------------- |
| FR-25 | Thanh toán thật qua Momo/VNPay/Stripe | Won't  | Chỉ mô phỏng      |
| FR-26 | Giao hàng tận nơi, theo dõi shipper   | Won't  | Chỉ nhận tại quầy |

## 3. Yêu cầu phi chức năng

| Mã     | Nhóm                | Mô tả                                    | MoSCoW | Tiêu chí chấp nhận                                                                                  |
| ------ | ------------------- | ---------------------------------------- | ------ | --------------------------------------------------------------------------------------------------- |
| NFR-01 | Hiệu năng           | API đọc phản hồi nhanh với dữ liệu mẫu   | Must   | `GET /products` với 100 sản phẩm phản hồi < 500 ms (đo bằng test hoặc log)                          |
| NFR-02 | Hiệu năng           | Menu hiển thị nhanh trên di động         | Should | Menu hiển thị nội dung đầu tiên < 3 giây trên mạng 4G giả lập (Lighthouse/DevTools)                 |
| NFR-03 | Bảo mật             | Mật khẩu được băm                        | Must   | Cột passwordHash dùng bcrypt; không có mật khẩu thô trong DB và log                                 |
| NFR-04 | Bảo mật             | API đơn hàng, thanh toán yêu cầu JWT     | Must   | Gọi `POST /orders`, `POST /payments`, `GET /orders/me` không JWT trả 401; token sai/hết hạn trả 401 |
| NFR-05 | Bảo mật             | Validate dữ liệu ở frontend và backend   | Must   | Dữ liệu sai định dạng bị từ chối với 400 và thông báo lỗi rõ ràng (class-validator)                 |
| NFR-06 | Bảo mật             | Secret không nằm trong repo và log CI    | Must   | Repo chỉ có `.env.example`; quét repo không thấy secret thật                                        |
| NFR-07 | Bảo mật             | Production dùng HTTPS; DB không public   | Must   | URL production là https; không kết nối trực tiếp được tới cổng PostgreSQL từ Internet               |
| NFR-08 | Khả dụng            | Giao diện mobile-first                   | Must   | Hiển thị đúng, không cuộn ngang ở màn hình rộng 375 px                                              |
| NFR-09 | Khả dụng            | Phản hồi rõ ràng khi tải, rỗng, lỗi      | Must   | Mọi màn hình gọi API có đủ 3 trạng thái loading/empty/error                                         |
| NFR-10 | Khả dụng            | Tương thích trình duyệt phổ biến         | Should | Chạy đúng luồng chính trên Chrome, Edge, Safari bản mới                                             |
| NFR-11 | Toàn vẹn dữ liệu    | Không bán quá tồn kho khi đặt đồng thời  | Must   | Test N yêu cầu đồng thời với tồn kho thấp: số đơn thành công ≤ tồn kho                              |
| NFR-12 | Khả năng triển khai | Chạy toàn hệ thống ở local bằng một lệnh | Must   | `docker-compose up` chạy web, API, DB; người ngoài nhóm làm theo README chạy được                   |
| NFR-13 | Khả năng triển khai | Có health check                          | Must   | `GET /health` trả 200 khi API và DB sẵn sàng                                                        |
| NFR-14 | Khả năng triển khai | CI/CD tự động                            | Must   | Pull request bị chặn merge nếu lint/test/build lỗi; push/tag chạy deploy và smoke test              |
| NFR-15 | Khả năng triển khai | Migration và seed tái tạo được DB        | Must   | Chạy migration + seed trên DB trống tạo đủ bảng và dữ liệu mẫu                                      |
| NFR-16 | Khả năng bảo trì    | Có logging cho lỗi và giao dịch          | Should | Log ghi request lỗi và kết quả thanh toán, không chứa dữ liệu nhạy cảm                              |

## 4. Bảng đối chiếu nhóm yêu cầu tối thiểu theo đề

| Nhóm theo đề           | Mã yêu cầu             |
| ---------------------- | ---------------------- |
| Menu                   | FR-01, FR-02, FR-03    |
| Chi tiết sản phẩm      | FR-04, FR-05           |
| Giỏ hàng               | FR-06, FR-07, FR-08    |
| Xác thực               | FR-09, FR-10, FR-11    |
| Đặt đơn                | FR-12, FR-13, FR-14    |
| Thanh toán             | FR-15 đến FR-19        |
| Lịch sử đơn            | FR-20, FR-21           |
| Quản lý trạng thái đơn | FR-22, FR-23, FR-24    |
| Hiệu năng              | NFR-01, NFR-02         |
| Bảo mật                | NFR-03 đến NFR-07      |
| Tính khả dụng          | NFR-08, NFR-09, NFR-10 |
| Khả năng triển khai    | NFR-12 đến NFR-15      |

## 5. Sơ đồ use-case tổng quát

Xem brewlite\docs\diagrams\BaoCao_UseCaseTongQuat
