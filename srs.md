# Software Requirement Specification (SRS)
## CAB System – Nền tảng đặt xe

### 1. Giới thiệu
- **Mục tiêu**: Xây dựng hệ thống đặt xe trực tuyến mới, khắc phục hạn chế của hệ thống hiện tại, có khả năng mở rộng và phát triển lâu dài.
- **Thời gian triển khai**: 7 tuần.
- **Khách hàng**: Công ty ABC – doanh nghiệp cung cấp dịch vụ đặt xe trực tuyến.

### 2. Phạm vi hệ thống
Hệ thống phục vụ ba nhóm người dùng chính:
- **Khách hàng**: Đăng ký, đăng nhập, đặt xe, theo dõi chuyến đi, thanh toán, đánh giá.
- **Tài xế**: Quản lý hồ sơ, phương tiện, trạng thái, nhận/chấp nhận chuyến, cập nhật tiến trình chuyến đi.
- **Nhân viên vận hành**: Quản trị khách hàng, tài xế, phương tiện, chuyến đi, xử lý sự cố, báo cáo.

### 3. Yêu cầu chức năng
- **Quản lý tài khoản**: Đăng ký, đăng nhập, cập nhật thông tin cho khách hàng và tài xế.
- **Đặt xe**: Nhập điểm đón/điểm đến, chọn loại xe, gửi yêu cầu.
- **Phân công tài xế**: Tìm tài xế phù hợp dựa trên vị trí và trạng thái, xử lý khi tài xế từ chối hoặc không phản hồi.
- **Theo dõi chuyến đi**: Hiển thị trạng thái chuyến, thời gian dự kiến, thông tin tài xế.
- **Thanh toán**: Tính cước, hỗ trợ tiền mặt và thanh toán điện tử, tích hợp nhà cung cấp bên ngoài.
- **Thông báo**: Gửi thông báo cho khách hàng và tài xế về các sự kiện quan trọng.
- **Quản trị**: Giao diện quản lý, phân quyền, báo cáo hoạt động.

### 4. Yêu cầu phi chức năng
- **Hiệu năng**: Hoạt động ổn định khi tải cao, mở rộng độc lập từng thành phần.
- **Triển khai**: Cho phép bổ sung chức năng mới mà không ảnh hưởng hệ thống hiện tại.
- **Bảo mật**: Xác thực, phân quyền, bảo vệ dữ liệu cá nhân, lưu vết thao tác.

### 5. Quy tắc nghiệp vụ
- Ưu tiên tài xế gần khách hàng và sẵn sàng.
- Tự động tìm tài xế khác nếu người đầu tiên từ chối hoặc không phản hồi.
- Không lưu thông tin nhạy cảm của thẻ/tài khoản thanh toán trong hệ thống.
- Chính sách xử lý khi thanh toán thất bại hoặc mất kết nối mạng cần được xác định.

### 6. Các điểm cần làm rõ
- Cách tính cước chi tiết.
- Tiêu chí ưu tiên tài xế.
- Thời gian phản hồi của tài xế.
- Chính sách hủy chuyến.
- Thời gian lưu trữ dữ liệu.

### 7. Kỳ vọng của khách hàng
- Một nền tảng đặt xe lâu dài, linh hoạt, hỗ trợ đầy đủ quy trình từ đặt xe đến thanh toán và đánh giá.
- Doanh nghiệp có thể phối hợp nội bộ và theo dõi hoạt động qua hệ thống.
- Business Analyst xác định rõ phạm vi, tác nhân, quy trình, yêu cầu chức năng/phi chức năng, quy tắc nghiệp vụ, ngoại lệ và các điểm chưa rõ để xác nhận với khách hàng.






```mermaid
quadrantChart
    title CAB System - Stakeholder Matrix
    x-axis Low Interest --> High Interest
    y-axis Low Power --> High Power

    quadrant-1 MANAGE CLOSELY
    quadrant-2 KEEP SATISFIED
    quadrant-3 MONITOR
    quadrant-4 KEEP INFORMED

    ABC Management: [0.82, 0.95]
    Product Owner: [0.92, 0.85]
    Project Manager: [0.85, 0.78]
    Operations Staff: [0.82, 0.58]
    Development Team: [0.75, 0.65]
    Payment Provider: [0.42, 0.62]
    Customer: [0.90, 0.38]
    Driver: [0.85, 0.32]
    QA Tester: [0.68, 0.42]
    External Partner: [0.28, 0.30]





