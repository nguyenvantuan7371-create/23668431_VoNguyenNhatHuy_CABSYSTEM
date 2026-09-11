
# Software Requirement Specification (SRS)
## CAB System – Nền tảng đặt xe


## 1. Xác định Stakeholder

| Stakeholder                     | Role                    | Responsibilities & Concerns                                                                                                                  |
| ------------------------------- | ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| **Ban lãnh đạo (Ban giám đốc)** | Sponsor / Project Owner | Kỳ vọng xây dựng nền tảng CAB mới phục vụ số lượng lớn người dùng. Cần báo cáo về doanh thu, tỷ lệ hoàn thành/hủy chuyến và hiệu quả tài xế. |
| **Khách hàng**                  | End-User                | Đăng ký, đặt xe, chọn loại xe, theo dõi chuyến đi, xem số tiền phải trả và đánh giá chuyến đi.                                               |
| **Tài xế**                      | End-User                | Đăng ký hồ sơ, cập nhật trạng thái sẵn sàng, nhận thông báo, chấp nhận/từ chối chuyến và cập nhật trạng thái chuyến đi.                      |
| **Nhân viên vận hành**          | Admin                   | Quản lý khách hàng, tài xế, phương tiện; hỗ trợ xử lý lỗi và tra cứu lịch sử giao dịch.                                                      |
| **Nhà cung cấp thanh toán**     | External System         | Tích hợp và xử lý các giao dịch thanh toán điện tử.                                                                                          |
| **Business Analyst**            | Project Team            | Xác định phạm vi, quy trình, yêu cầu và làm rõ các quy tắc nghiệp vụ như tính cước, tiêu chí ưu tiên và thời gian phản hồi.                  |
| **Nhóm phát triển (Dev Team)**  | Project Team            | Xây dựng hệ thống ổn định, có khả năng mở rộng độc lập từng thành phần khi tải hệ thống tăng.                                                |



## 2. Stakeholder Matrix:
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
```




## 3. Business Rules - Quy tắc nghiệp vụ

Customer Management
| ID       | Business Rule                                                      | Nguồn yêu cầu     |
| -------- | ------------------------------------------------------------------ | ----------------- |
| **BR01** | Khách hàng phải có tài khoản hợp lệ để sử dụng dịch vụ đặt xe.     | Quản lý tài khoản |
| **BR02** | Khách hàng phải cung cấp điểm đón, điểm đến và loại xe khi đặt xe. | Đặt xe            |
| **BR03** | Khách hàng chỉ được theo dõi chuyến sau khi yêu cầu được xác nhận. | Theo dõi chuyến   |
| **BR04** | Khách hàng chỉ được đánh giá sau khi chuyến xe hoàn thành.         | Đánh giá          |

Driver Management
| ID       | Business Rule                                                                                                |
| -------- | ------------------------------------------------------------------------------------------------------------ |
| **BR05** | Chỉ tài xế có trạng thái sẵn sàng mới được nhận chuyến.                                                      |
| **BR06** | Hệ thống ưu tiên tài xế gần khách hàng và đang sẵn sàng.                                                     |
| **BR07** | Tài xế phải phản hồi yêu cầu chuyến trong thời gian quy định.                                                |
| **BR08** | Nếu tài xế từ chối chuyến, hệ thống phải tìm tài xế phù hợp tiếp theo.                                       |
| **BR09** | Nếu tài xế không phản hồi trong thời gian quy định, hệ thống phải coi yêu cầu là timeout và tìm tài xế khác. |
| **BR10** | Chuyến xe chỉ được xác nhận khi tài xế chấp nhận yêu cầu.                                                    |

Booking Management
| ID       | Business Rule                                                                      |
| -------- | ---------------------------------------------------------------------------------- |
| **BR11** | Mỗi yêu cầu đặt xe phải xác định được khách hàng, điểm đón, điểm đến và loại xe.   |
| **BR12** | Một yêu cầu đặt xe chỉ được chuyển thành chuyến khi có tài xế chấp nhận.           |
| **BR13** | Nếu không có tài xế phù hợp, hệ thống phải thông báo cho khách hàng.               |
| **BR14** | Hệ thống phải duy trì trạng thái của yêu cầu/chuyến xe trong suốt quá trình xử lý. |

Payment Management
| ID       | Business Rule                                                                                                |
| -------- | ------------------------------------------------------------------------------------------------------------ |
| **BR15** | Hệ thống phải hỗ trợ thanh toán bằng tiền mặt và thanh toán điện tử.                                         |
| **BR16** | Thanh toán điện tử phải được thực hiện thông qua nhà cung cấp thanh toán bên ngoài.                          |
| **BR17** | Hệ thống không được lưu thông tin nhạy cảm của thẻ hoặc tài khoản thanh toán.                                |
| **BR18** | Khi thanh toán thất bại, hệ thống phải ghi nhận trạng thái thất bại và thực hiện chính sách xử lý tương ứng. |


## 4. Module


```
│
├── 1. Customer Management
│   ├── 🔑 Registration / Login
│   ├── 👤 Profile Management
│   ├── 📅 Booking
│   ├── 📍 Trip Tracking
│   └── ⭐ Rating
│
├── 2. Driver Management
│   ├── 👤 Driver Profile
│   ├── 🚗 Vehicle Management
│   ├── ⏲️ Driver Availability
│   ├── 📍 Driver Location
│   ├── ✅ Ride Acceptance
│   └── 🚦 Trip Progress
│
├── 3. Booking & Dispatch Management
│   ├── ➕ Create Booking
│   ├── 🔍 Find Driver
│   ├── 📤 Driver Assignment
│   ├── 🔄 Reassignment
│   └── 📊 Booking Status
│
├── 4. Payment Management
│   ├── 🧮 Fare Calculation
│   ├── 💵 Cash Payment
│   ├── 💳 Electronic Payment
│   └── ❌ Payment Failure
│
└── 5. Operation & Administration
├── 👥 Customer Management
├── 👤 Driver Management
├── 🚗 Vehicle Management
├── 🗺️ Trip Management
├── ⚠️ Incident Management
├── 🔒 Permission Management
└── 📑 Reporting
```

## 5. Thiết kế Business Requirement
BR01 – Customer Registration & Account

Module: Customer Management

Business Objective:
Cho phép khách hàng tạo và sử dụng tài khoản để sử dụng dịch vụ CAB.

Actor: Customer

Business Requirement:
Hệ thống phải cho phép khách hàng đăng ký, đăng nhập và cập nhật thông tin tài khoản.

Business Rules:

Khách hàng phải có tài khoản hợp lệ để đặt xe.
Tài khoản không hoạt động không được phép tạo yêu cầu đặt xe.
```
Customer
   ↓
Register
   ↓
Provide Information
   ↓
System validates information
   ↓
Create Account
   ↓
Login
   ↓
Account Active
```




BR02 – Create Booking

Module: Customer Management / Booking Management

Business Objective:
Cho phép khách hàng tạo yêu cầu đặt xe.

Actor: Customer

Business Requirement:
Hệ thống phải cho phép khách hàng nhập điểm đón, điểm đến và loại xe để gửi yêu cầu đặt xe.

Business Rules:

Khách hàng phải đăng nhập.
Điểm đón phải được xác định.
Điểm đến phải được xác định.
Loại xe phải được lựa chọn.
```
Customer
   ↓
Enter Pickup
   ↓
Enter Destination
   ↓
Select Vehicle Type
   ↓
Submit Booking
   ↓
System creates Booking
```


BR03 – Driver Assignment

Module: Driver Management / Booking & Dispatch

Business Objective:
Tự động tìm và phân công tài xế phù hợp.

Actor: System

Business Requirement:
Hệ thống phải tìm tài xế dựa trên vị trí và trạng thái sẵn sàng.

Business Rules:

Chỉ tài xế Available mới được lựa chọn.
Ưu tiên tài xế gần khách hàng.
Tài xế phải phản hồi trong thời gian quy định.
Từ chối → tìm tài xế khác.
Timeout → tìm tài xế khác.
```
Booking Created
      ↓
Find Available Drivers
      ↓
Calculate / Compare Distance
      ↓
Select Nearest Driver
      ↓
Send Ride Request
      ↓
 ┌────┴─────────────┐
 ↓                  ↓
Accept          Reject / Timeout
 ↓                  ↓
Confirm Trip    Find Next Driver
```



BR04 – Trip Management

Module: Booking & Dispatch / Driver Management

Business Objective:
Quản lý toàn bộ vòng đời của chuyến xe.

Business Requirement:
Hệ thống phải cho phép tài xế cập nhật tiến trình chuyến và cho phép khách hàng theo dõi trạng thái.

Flow:
```
Assigned
   ↓
Driver Arriving
   ↓
Driver Arrived
   ↓
Trip Started
   ↓
Trip In Progress
   ↓
Trip Completed
```


BR05 – Payment Management

Module: Payment Management

Business Objective:
Cho phép khách hàng thanh toán chi phí chuyến xe.

Business Requirement:
Hệ thống phải hỗ trợ thanh toán tiền mặt và thanh toán điện tử thông qua nhà cung cấp bên ngoài.

Business Rules:

Không lưu dữ liệu nhạy cảm của thẻ/tài khoản.
Payment Provider xử lý thanh toán điện tử.
Hệ thống phải nhận kết quả thanh toán.
Thanh toán thất bại phải được ghi nhận.


```
Trip Completed
      ↓
Calculate Final Fare
      ↓
Select Payment Method
      ↓
 ┌────┴──────────┐
 ↓               ↓
Cash       Electronic Payment
 ↓               ↓
Record       Payment Provider
Payment          ↓
             Payment Result
                  ↓
           Success / Failed
```






BR06 – Trip Rating

Module: Customer Management

Business Objective:
Thu thập đánh giá chất lượng dịch vụ.

Business Requirement:
Khách hàng được phép đánh giá chuyến xe sau khi chuyến hoàn thành.
Business Rule:
```
Trip Completed
      ↓
Customer can Rate
      ↓
Submit Rating
      ↓
Store Rating
```




BR07 – Operation Management

Module: Operation & Administration

Business Objective:
Cho phép nhân viên vận hành quản lý và giám sát hệ thống.

Business Requirement:
Nhân viên vận hành phải có khả năng quản lý khách hàng, tài xế, phương tiện, chuyến đi, sự cố và báo cáo.

Business Rules:

Nhân viên chỉ được thực hiện chức năng theo quyền được cấp.
Các thao tác quản trị quan trọng phải được lưu vết.
Báo cáo phải phản ánh dữ liệu hoạt động của hệ thống.




## 6. Chức năng nghiệp vụ — FR
Module Customer Management
Quy trình nghiệp vụ
```
Khách hàng
    ↓
Đăng ký / Đăng nhập
    ↓
Quản lý thông tin cá nhân
    ↓
Đặt xe
    ↓
Theo dõi chuyến
    ↓
Thanh toán
    ↓
Đánh giá
```



Functional Requirements
| ID       | Functional Requirement | Mô tả                                         |
| -------- | ---------------------- | --------------------------------------------- |
| **FR01** | Đăng ký tài khoản      | Hệ thống cho phép khách hàng tạo tài khoản    |
| **FR02** | Đăng nhập              | Hệ thống xác thực khách hàng                  |
| **FR03** | Cập nhật thông tin     | Khách hàng có thể cập nhật thông tin cá nhân  |
| **FR04** | Tạo yêu cầu đặt xe     | Khách hàng nhập điểm đón, điểm đến, loại xe   |
| **FR05** | Theo dõi chuyến        | Khách hàng xem trạng thái và thông tin tài xế |
| **FR06** | Thanh toán chuyến      | Khách hàng thực hiện thanh toán               |
| **FR07** | Đánh giá chuyến        | Khách hàng đánh giá sau khi chuyến hoàn thành |



Module Driver Management
Quy trình nghiệp vụ
```
Tài xế
  ↓
Đăng nhập
  ↓
Quản lý hồ sơ
  ↓
Quản lý phương tiện
  ↓
Cập nhật trạng thái
  ↓
Nhận yêu cầu chuyến
  ↓
Chấp nhận / Từ chối
  ↓
Cập nhật tiến trình chuyến
```

Functional Requirements
| ID       | Functional Requirement | Mô tả                                       |
| -------- | ---------------------- | ------------------------------------------- |
| **FR08** | Đăng nhập tài xế       | Tài xế đăng nhập vào hệ thống               |
| **FR09** | Quản lý hồ sơ tài xế   | Cập nhật thông tin cá nhân/hồ sơ            |
| **FR10** | Quản lý phương tiện    | Quản lý thông tin phương tiện               |
| **FR11** | Cập nhật trạng thái    | Chuyển trạng thái Available/Unavailable     |
| **FR12** | Nhận yêu cầu chuyến    | Tài xế nhận thông tin chuyến được phân công |
| **FR13** | Chấp nhận chuyến       | Tài xế chấp nhận yêu cầu                    |
| **FR14** | Từ chối chuyến         | Tài xế từ chối yêu cầu                      |
| **FR15** | Cập nhật tiến trình    | Tài xế cập nhật trạng thái chuyến           |



Module Booking & Dispatch
Quy trình
```
Booking Request
      ↓
Tìm tài xế phù hợp
      ↓
Ưu tiên tài xế
      ↓
Gửi yêu cầu
      ↓
┌───────────────┐
│               │
Accept       Reject / Timeout
│               │
↓               ↓
Xác nhận      Tìm tài xế khác
chuyến            │
                  └──→ Gửi yêu cầu lại
```



Functional Requirements
| ID       | Functional Requirement | Mô tả                                    |
| -------- | ---------------------- | ---------------------------------------- |
| **FR16** | Tạo Booking            | Tạo yêu cầu đặt xe                       |
| **FR17** | Tìm tài xế             | Tìm tài xế dựa trên vị trí và trạng thái |
| **FR18** | Xếp hạng tài xế        | Ưu tiên tài xế phù hợp                   |
| **FR19** | Gửi yêu cầu chuyến     | Gửi request tới tài xế                   |
| **FR20** | Xử lý tài xế chấp nhận | Xác nhận chuyến khi tài xế nhận          |
| **FR21** | Xử lý tài xế từ chối   | Tìm tài xế khác                          |
| **FR22** | Xử lý timeout          | Tìm tài xế khác nếu không phản hồi       |
| **FR23** | Xử lý không có tài xế  | Thông báo khách hàng                     |



Module Trip Management
Quy trình
```
Trip Created
     ↓
Driver Assigned
     ↓
Driver Arriving
     ↓
Driver Arrived
     ↓
Trip In Progress
     ↓
Trip Completed
```

Functional Requirements
| ID       | Functional Requirement                    |
| -------- | ----------------------------------------- |
| **FR24** | Tạo chuyến                                |
| **FR25** | Gán tài xế cho chuyến                     |
| **FR26** | Cập nhật trạng thái chuyến                |
| **FR27** | Hiển thị trạng thái chuyến cho khách hàng |
| **FR28** | Hiển thị thông tin tài xế                 |
| **FR29** | Ghi nhận hoàn thành chuyến                |




Module Payment Management
Quy trình
```
Trip Completed
      ↓
Tính cước
      ↓
Chọn phương thức thanh toán
      ↓
┌──────────────┴──────────────┐
│                             │
Tiền mặt                 Điện tử
│                             │
↓                             ↓
Ghi nhận              Payment Provider
                              ↓
                       Thành công / Thất bại
```


Functional Requirements
| ID       | Functional Requirement      |
| -------- | --------------------------- |
| **FR30** | Tính cước chuyến            |
| **FR31** | Thanh toán tiền mặt         |
| **FR32** | Thanh toán điện tử          |
| **FR33** | Tích hợp Payment Provider   |
| **FR34** | Ghi nhận kết quả thanh toán |
| **FR35** | Xử lý thanh toán thất bại   |



Module Notification
| ID       | Functional Requirement          |
| -------- | ------------------------------- |
| **FR36** | Gửi thông báo đặt xe            |
| **FR37** | Gửi thông báo phân công tài xế  |
| **FR38** | Gửi thông báo trạng thái chuyến |
| **FR39** | Gửi thông báo thanh toán        |




Module Operation & Administration
| ID       | Functional Requirement |
| -------- | ---------------------- |
| **FR40** | Quản lý khách hàng     |
| **FR41** | Quản lý tài xế         |
| **FR42** | Quản lý phương tiện    |
| **FR43** | Quản lý chuyến đi      |
| **FR44** | Xử lý sự cố            |
| **FR45** | Quản lý phân quyền     |
| **FR46** | Xem báo cáo hoạt động  |




## 7. Yêu cầu phi chức năng (Non-Functional Requirements - NFR)
## Non-Functional Requirements (NFR)

| ID        | Danh mục                               | Yêu cầu phi chức năng (NFR)                                                                                                                                                      |
| --------- | -------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **NFR01** | **Hiệu năng (Performance)**            | Hệ thống phải hoạt động ổn định vào các thời điểm nhu cầu đặt xe tăng cao.                                                                                                       |
| **NFR02** | **Khả năng mở rộng (Scalability)**     | Các thành phần của hệ thống (thanh toán, thông báo,...) cần có khả năng mở rộng độc lập khi tải tăng mà không làm ngừng toàn bộ hệ thống.                                        |
| **NFR03** | **Khả năng bảo trì (Maintainability)** | Kiến trúc phải linh hoạt để dễ dàng bổ sung dịch vụ mới, thêm phương thức thanh toán hoặc nhà cung cấp thông báo trong tương lai. Các chức năng mới có thể triển khai từng phần. |
| **NFR04** | **Bảo mật (Security)**                 | Khách hàng và tài xế phải được xác thực trước khi sử dụng các chức năng yêu cầu tài khoản.                                                                                       |
| **NFR05** | **Bảo mật dữ liệu (Data Privacy)**     | Không lưu trữ trực tiếp thông tin nhạy cảm của thẻ hoặc tài khoản thanh toán trong hệ thống CAB.                                                                                 |
| **NFR06** | **Kiểm toán (Audit)**                  | Hệ thống phải lưu vết các thao tác quản trị quan trọng để phục vụ kiểm tra khi có sự cố.                                                                                         |
  
## 8. Mô hình thực thể kết hợp (ERD Entities - Mức khái quát)

## 9. Thiết kế Usecase (Danh sách Usecase cho MVP)
   ## Use Case List

### 👤 Actor: Khách hàng (Customer)

* **UC01 - Quản lý tài khoản:** Đăng ký, đăng nhập và cập nhật thông tin cá nhân.
* **UC02 - Đặt xe:** Nhập điểm đón, điểm đến, chọn loại xe và xác nhận gửi yêu cầu đặt xe.
* **UC03 - Theo dõi chuyến:** Xem trạng thái tìm tài xế, thông tin tài xế đã nhận chuyến, thời gian dự kiến tài xế đến và trạng thái hiện tại của chuyến đi.
* **UC04 - Thanh toán:** Xem số tiền phải trả và lựa chọn phương thức thanh toán (**Tiền mặt / Điện tử**).
* **UC05 - Đánh giá:** Đánh giá tài xế và chuyến đi sau khi chuyến đi hoàn thành.

### 🚗 Actor: Tài xế (Driver)

* **UC06 - Đăng nhập & Hồ sơ:** Đăng nhập, cập nhật hồ sơ cá nhân và thông tin phương tiện.
* **UC07 - Cập nhật trạng thái làm việc:** Bật hoặc tắt trạng thái **sẵn sàng nhận chuyến (Available)**.
* **UC08 - Xử lý yêu cầu đặt xe:** Nhận thông báo về chuyến mới và lựa chọn **chấp nhận hoặc từ chối** chuyến.
* **UC09 - Cập nhật hành trình:** Chuyển đổi trạng thái chuyến đi theo trình tự: **Đã đến điểm đón → Đã đón khách → Đang di chuyển → Hoàn thành chuyến**.

### ⚙️ Actor: Hệ thống (System)

* **UC10 - Phân công tự động (Dispatch):** Xác định vị trí tài xế, tìm tài xế phù hợp và gần nhất đang ở trạng thái **Available**. Tự động tiếp tục tìm tài xế khác khi tài xế được chọn **từ chối hoặc không phản hồi**.

## 10. Tiêu chí chấp nhận (Acceptance Criteria - AC)

### AC cho UC02 – Đặt xe

* Hệ thống không cho phép khách hàng gửi yêu cầu nếu để trống **"Điểm đón"** hoặc **"Điểm đến"**.
* Hệ thống phải hiển thị rõ các loại xe (VD: **4 chỗ, 7 chỗ, xe máy**) để khách hàng chọn.

### AC cho UC10 – Phân công tự động

* Khi khách hàng tạo chuyến, hệ thống chỉ gửi yêu cầu cho tài xế có trạng thái **"Sẵn sàng" (Available)**.
* Nếu tài xế đầu tiên nhấn **"Từ chối"**, hệ thống phải tự động tiếp tục tìm tài xế khác phù hợp mà không yêu cầu khách hàng tạo lại yêu cầu đặt xe.
* Trong trường hợp hệ thống không tìm được bất kỳ tài xế nào, khách hàng phải được nhận **thông báo rõ ràng**.

### AC cho UC04 – Thanh toán điện tử

* Hệ thống bắt buộc phải gửi **request** sang Cổng thanh toán bên ngoài (**Payment Provider**).
* Hệ thống **không được lưu giữ** thông tin thẻ tín dụng/tài khoản ngân hàng của khách hàng trong Database của hệ thống CAB.
* Nếu thanh toán điện tử thất bại, hệ thống phải **hiển thị thông báo lỗi** cho khách hàng.
* Hệ thống phải cho phép khách hàng **thực hiện thanh toán lại**.
 
## 11. Bảng truy vết (Traceability Matrix)


| ID Yêu cầu nghiệp vụ (BR)     | ID Yêu cầu chức năng (FR)                | ID Use Case (UC) | Đối tượng tương tác (Actor)  |
| ----------------------------- | ---------------------------------------- | ---------------- | ---------------------------- |
| **BR01 - Customer Account**   | FR01, FR02, FR03                         | UC01             | Khách hàng                   |
| **BR02 - Create Booking**     | FR04, FR16                               | UC02             | Khách hàng                   |
| **BR03 - Driver Assignment**  | FR17, FR18, FR19, FR21, FR22, FR23       | UC08, UC10       | Tài xế, Hệ thống             |
| **BR04 - Trip Management**    | FR05, FR15, FR24, FR26, FR27, FR28, FR29 | UC03, UC09       | Khách hàng, Tài xế           |
| **BR05 - Payment Management** | FR30, FR31, FR32, FR33, FR34, FR35       | UC04             | Khách hàng, Payment Provider |
| **BR06 - Trip Rating**        | FR07                                     | UC05             | Khách hàng                   |
| **BR07 - Notification**       | FR36, FR37, FR38, FR39                   | Gắn kèm các UC   | Khách hàng, Tài xế, Hệ thống |

   
