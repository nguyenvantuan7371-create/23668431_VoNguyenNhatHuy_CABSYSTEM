# KỊCH BẢN KIỂM THỬ (TEST SCENARIO)

## 1. Thông tin chung
* **Mã kịch bản:** TS_DRIVER_CANCEL_01
* **Tên kịch bản:** Tài xế hủy chuyến sau khi đã nhận chuyến
* **Phân hệ:** Quản lý Tài xế (Driver API), Điều phối chuyến (Dispatch API), Quản lý chuyến đi (Trip Management API), Thông báo (Notification API)
* **Yêu cầu SRS liên quan:** FR08 - FR12, FR05, FR17, FR18, FR21 - FR24, FR36 - FR37
* **Mức độ ưu tiên:** High (Cao)

---

## 2. Tiền điều kiện & Điều kiện sau
* **Tiền điều kiện (Pre-conditions):**
  1. Khách hàng đã đặt chuyến xe thành công.
  2. Tài xế đang ở trạng thái `ONLINE` và đã nhấn "Chấp nhận chuyến".
  3. Chuyến xe đang ở trạng thái `ACCEPTED` hoặc `DRIVER_ARRIVING`.
* **Điều kiện sau (Post-conditions):**
  1. Trạng thái chuyến xe chuyển thành `CANCELLED_BY_DRIVER` hoặc kích hoạt luồng `RE_DISPATCHING` (tìm tài xế mới).
  2. Hệ thống gửi thông báo Real-time cho Khách hàng về việc hủy chuyến.
  3. Lý do hủy chuyến của tài xế được lưu vào nhật ký hệ thống (Audit Log).
  4. Cập nhật tỷ lệ hủy chuyến (Cancellation Rate) của tài xế.

---

## 3. Các API liên quan
* `PUT /api/v1/driver/trips/{tripId}/cancel` - Tài xế gửi yêu cầu hủy chuyến.
* `POST /api/v1/dispatch/re-dispatch` - Hệ thống thực hiện điều phối lại tài xế khác.
* `POST /api/v1/notifications/push` - Gửi thông báo đến ứng dụng Khách hàng.

---

## 4. Danh sách trường hợp kiểm thử (Test Cases)

| STT | Mã Test Case | Mô tả Test Case | Dữ liệu đầu vào (Test Data) | Các bước thực hiện | Kết quả mong đợi | Mức độ |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | TC_CANCEL_01 | Tài xế hủy chuyến thành công với lý do hợp lệ trước khi đón khách | `tripId`: Valid<br>`reason`: "Hỏng xe đột xuất" | 1. Gọi API `PUT /trips/{tripId}/cancel` với lý do hợp lệ.<br>2. Kiểm tra trạng thái chuyến xe.<br>3. Kiểm tra thông báo gửi cho khách hàng. | - Response HTTP 200 OK.<br>- Trạng thái chuyến chuyển sang `CANCELLED_BY_DRIVER` / `RE_DISPATCHING`.<br>- Khách hàng nhận được thông báo tài xế đã hủy chuyến. | High |
| 2 | TC_CANCEL_02 | Tài xế hủy chuyến không nhập lý do hủy | `tripId`: Valid<br>`reason`: "" (Rỗng) | 1. Gọi API `PUT /trips/{tripId}/cancel` không truyền field `reason`. | - Response HTTP 400 Bad Request.<br>- Báo lỗi validation: "Lý do hủy chuyến không được để trống".<br>- Trạng thái chuyến xe không thay đổi. | Medium |
| 3 | TC_CANCEL_03 | Tài xế cố gắng hủy chuyến khi chuyến xe đã ở trạng thái `IN_PROGRESS` (Đang di chuyển) | `tripId`: Valid<br>Status: `IN_PROGRESS` | 1. Tài xế bắt đầu chuyến đi (`IN_PROGRESS`).<br>2. Gửi yêu cầu hủy chuyến qua API `PUT /trips/{tripId}/cancel`. | - Response HTTP 409 Conflict hoặc 400 Bad Request.<br>- Báo lỗi: "Không thể hủy chuyến xe đang trong hành trình". | High |
| 4 | TC_CANCEL_04 | Hệ thống tự động kích hoạt điều phối lại (Re-dispatch) tài xế mới khi tài xế cũ hủy chuyến | `tripId`: Valid<br>`autoReDispatch`: true | 1. Tài xế A hủy chuyến thành công.<br>2. Kiểm tra log hệ thống Dispatch API. | - Hệ thống tự động chuyển chuyến xe sang hàng chờ `RE_DISPATCHING`.<br>- Bắn thông báo tìm tài xế B gần khu vực khách hàng. | High |
| 5 | TC_CANCEL_05 | Kiểm tra ghi nhận tỷ lệ phạt / lịch sử hủy chuyến của tài xế | `driverId`: Valid | 1. Thực hiện hủy chuyến thành công.<br>2. Gọi API lấy thông tin tài xế `GET /api/v1/driver/profile`. | - Lịch sử chuyến xe hiển thị lý do hủy.<br>- Tỷ lệ hủy chuyến (Cancellation Rate) của tài xế tăng lên theo công thức quy định. | Low |
