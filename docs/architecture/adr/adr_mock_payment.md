# ADR 003: Sử dụng Dịch vụ Thanh toán Giả lập (Mock Payment Gateway)

## Trạng thái
Đã chấp nhận (Accepted)

## Bối cảnh
BrewLite là ứng dụng đặt cà phê không tiền mặt, yêu cầu tích hợp thanh toán qua ví điện tử hoặc thẻ ngân hàng (Momo, VNPay, Thẻ). Trong phạm vi bài tập lớn, việc tích hợp cổng thanh toán thật (sandbox/live) gặp nhiều rào cản về xác thực tài khoản doanh nghiệp và phí dịch vụ.

## Quyết định
Xây dựng một **Mock Payment Service** nằm bên trong Backend NestJS để mô phỏng hoàn toàn các kịch bản thanh toán (thành công, thất bại, timeout) kèm cơ chế chống trùng lặp giao dịch (`Idempotency-Key`).

## Hệ quả (Trade-offs)
*   **Thuận lợi:**
    *   Giúp kiểm thử toàn bộ luồng nghiệp vụ end-to-end một cách ổn định, độc lập với các dịch vụ bên ngoài.
    *   Dễ dàng thiết lập các trường hợp kiểm thử tự động (Integration tests) cho trạng thái thanh toán.
*   **Bất lợi:**
    *   Không phản ánh chính xác 100% các độ trễ mạng thực tế từ API bên thứ ba, nhưng hoàn toàn đáp ứng đủ tiêu chí học tập và nghiệm thu của đồ án.