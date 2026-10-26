# -CNTT2-_-NhapMonCNTT-_Session12_Ex04
# BÀI TẬP: THIẾT KẾ HỆ THỐNG QUẢN LÝ BỆNH ÁN ĐIỆN TỬ CLINIC-CLOUD

## PHẦN 1. PHÂN TÍCH YÊU CẦU HỆ THỐNG

### 1.1. Tổng quan

Clinic-Cloud là hệ thống quản lý bệnh án điện tử dành cho phòng khám Startup. Hệ thống giúp lưu trữ thông tin bệnh nhân, hỗ trợ quá trình khám bệnh, quản lý hồ sơ và thống kê tình hình dịch bệnh.

Mục tiêu là xây dựng hệ thống có khả năng quản lý ít nhất 1.000 hồ sơ bệnh nhân, bảo đảm tính chính xác, an toàn và thuận tiện khi sử dụng.

### 1.2. Yêu cầu chức năng (Functional Requirements)

Yêu cầu chức năng mô tả những công việc hệ thống phải thực hiện.

| Mã | Yêu cầu chức năng | Mô tả |
|---|---|---|
| FR01 | Quản lý bệnh nhân | Thêm, sửa, tìm kiếm và cập nhật thông tin bệnh nhân. |
| FR02 | Quản lý bệnh án | Lưu trữ tiền sử bệnh, triệu chứng, chẩn đoán và kết quả khám. |
| FR03 | Quản lý lịch khám | Tạo lịch hẹn và theo dõi trạng thái khám bệnh. |
| FR04 | Quản lý người dùng | Phân quyền cho bác sĩ, nhân viên và quản trị viên. |
| FR05 | Thống kê dịch bệnh | Tổng hợp số lượng bệnh nhân theo từng nhóm bệnh và hiển thị bằng biểu đồ Donut. |
| FR06 | Tra cứu hồ sơ | Tìm kiếm hồ sơ theo mã bệnh nhân hoặc thông tin phù hợp với quyền truy cập. |

### 1.3. Yêu cầu phi chức năng (Non-functional Requirements)

Yêu cầu phi chức năng quy định chất lượng, hiệu suất và các điều kiện an toàn mà hệ thống phải đáp ứng.

| Mã | Yêu cầu | Mô tả |
|---|---|---|
| NFR01 | Bảo mật | Chỉ người dùng được cấp quyền mới có thể truy cập thông tin bệnh án. |
| NFR02 | Xác thực | Người dùng phải đăng nhập trước khi sử dụng các chức năng được bảo vệ. |
| NFR03 | Mã hóa | Mã hóa dữ liệu nhạy cảm khi truyền qua mạng và khi lưu trữ theo chính sách bảo mật. |
| NFR04 | Phân quyền | Bác sĩ, nhân viên và quản trị viên được cấp quyền phù hợp với nhiệm vụ. |
| NFR05 | Sao lưu | Sao lưu dữ liệu định kỳ và kiểm tra khả năng khôi phục dữ liệu. |
| NFR06 | Hiệu suất | Hỗ trợ quản lý tối thiểu 1.000 hồ sơ bệnh nhân và tra cứu dữ liệu trong thời gian đáp ứng mục tiêu hiệu suất đã xác định. |
| NFR07 | Nhật ký hệ thống | Ghi nhận các hoạt động truy cập và chỉnh sửa dữ liệu quan trọng để phục vụ kiểm tra. |
| NFR08 | Tính sẵn sàng | Hệ thống có phương án phục hồi khi xảy ra sự cố. |

**Phân biệt:** Yêu cầu chức năng mô tả hệ thống *làm gì*, còn yêu cầu phi chức năng mô tả hệ thống *phải hoạt động như thế nào*.

### 1.4. Bảo vệ dữ liệu y tế

Dữ liệu bệnh nhân là thông tin nhạy cảm, vì vậy Clinic-Cloud cần áp dụng các biện pháp sau:

- Chỉ thu thập thông tin cần thiết cho hoạt động khám chữa bệnh.
- Không chia sẻ hồ sơ bệnh nhân cho người không có thẩm quyền.
- Sử dụng mật khẩu mạnh và cơ chế xác thực phù hợp.
- Phân quyền truy cập theo vai trò.
- Sao lưu dữ liệu và kiểm tra khả năng khôi phục.
- Ghi nhật ký truy cập, chỉnh sửa và xuất dữ liệu.
- Tuân thủ quy định pháp luật hiện hành về bảo vệ dữ liệu cá nhân và thông tin y tế.

---

## PHẦN 2. TRIỂN KHAI HỆ THỐNG CLINIC-CLOUD

### 2.1. Quản lý dữ liệu bệnh nhân bằng Excel

Sử dụng Excel để quản lý 1.000 hồ sơ bệnh nhân theo tư duy Database Mindset.

**Cấu trúc bảng dữ liệu:**

| PatientID | FullName | DateOfBirth | Gender | Phone | Diagnosis | VisitDate |
|---|---|---|---|---|---|---|
| BN0001 | Nguyễn Văn A | 2000-05-10 | Nam | 09xxxxxxxx | Cúm | 2026-10-01 |
| BN0002 | Trần Thị B | 1998-08-15 | Nữ | 09xxxxxxxx | Viêm họng | 2026-10-02 |
| BN0003 | Lê Văn C | 2002-12-20 | Nam |  | Chưa xác định | 2026-10-03 |

*Lưu ý: Dữ liệu trên chỉ là ví dụ minh họa, không phải hồ sơ bệnh nhân có thật.*

### 2.2. Áp dụng Database Mindset

Khi thiết kế bảng dữ liệu, cần tuân thủ các nguyên tắc:

1. **Mỗi hàng là một bản ghi:** Mỗi hàng tương ứng với một bệnh nhân hoặc một bản ghi theo dõi được xác định rõ.
2. **Mỗi cột là một thuộc tính:** Ví dụ, PatientID, FullName và Diagnosis.
3. **Khóa chính duy nhất:** PatientID không được trùng lặp hoặc để trống.
4. **Không gộp ô:** Tránh Merge Cells trong vùng dữ liệu.
5. **Chuẩn hóa dữ liệu:** Thống nhất định dạng ngày tháng, số điện thoại và cách viết thông tin.
6. **Tách bảng khi cần thiết:** Nếu một bệnh nhân có nhiều lần khám, nên tách bảng Patients và Visits, liên kết bằng PatientID.
7. **Không dùng ô trống để biểu thị nhiều ý nghĩa:** Cần phân biệt thông tin chưa được cung cấp, chưa xác định và không áp dụng.

### 2.3. Xử lý bẫy dữ liệu trống và AutoFilter

**Vấn đề:** Bảng dữ liệu có hàng nghìn ô trống do bệnh nhân chưa cung cấp đầy đủ thông tin. Nếu vùng dữ liệu bị chia thành nhiều khối bởi hàng trống hoàn toàn, việc bật AutoFilter có thể không bao phủ toàn bộ dữ liệu.

**Cách khắc phục:**

- Không chèn hàng trống hoàn toàn vào giữa bảng dữ liệu.
- Nếu bệnh nhân thiếu thông tin, để trống ô tương ứng thay vì bỏ cả hàng dữ liệu.
- Chọn toàn bộ vùng dữ liệu và nhấn `Ctrl + T` để chuyển thành Excel Table.
- Đánh dấu tùy chọn **My table has headers** nếu bảng có hàng tiêu đề.
- Sử dụng bộ lọc tại hàng tiêu đề để tìm kiếm và lọc dữ liệu.

**Kết luận:** Nên sử dụng Excel Table thay vì chỉ chọn một vùng dữ liệu thông thường. Excel Table tự mở rộng khi thêm bản ghi và giúp duy trì phạm vi lọc ổn định.

### 2.4. Trực quan hóa dữ liệu bằng biểu đồ Donut

Biểu đồ Donut giúp thể hiện tỷ lệ bệnh nhân theo từng nhóm bệnh.

**Dữ liệu minh họa:**

| Nhóm bệnh | Số bệnh nhân |
|---|---:|
| Cúm | 400 |
| Viêm họng | 300 |
| Dị ứng | 200 |
| Nhóm bệnh khác | 100 |
| **Tổng cộng** | **1.000** |

**Các bước tạo biểu đồ trong Excel:**

1. Tạo bảng thống kê số lượng bệnh nhân theo nhóm bệnh.
2. Chọn hai cột Nhóm bệnh và Số bệnh nhân.
3. Vào **Insert → Insert Pie or Doughnut Chart → Doughnut**.
4. Thêm Data Labels để hiển thị tỷ lệ phần trăm.
5. Đặt tiêu đề biểu đồ là **Tỷ lệ bệnh nhân theo nhóm bệnh**.
6. Loại bỏ các hiệu ứng trang trí không cần thiết để biểu đồ dễ đọc.

**Tỷ lệ minh họa:**

- Cúm: 40%.
- Viêm họng: 30%.
- Dị ứng: 20%.
- Nhóm bệnh khác: 10%.

*Lưu ý: Số liệu chỉ phục vụ minh họa cách tạo biểu đồ, không phản ánh tình hình dịch bệnh thực tế.*

---

## PHẦN 3. KIỂM THỬ VÀ ĐỊNH DẠNG TÀI LIỆU SRS

### 3.1. Sử dụng Heading Styles trong Word

Trong tài liệu SRS, cần sử dụng các kiểu tiêu đề thống nhất để tạo cấu trúc rõ ràng.

- **Heading 1:** Tiêu đề chương lớn.
- **Heading 2:** Tiêu đề mục.
- **Heading 3:** Tiêu đề mục nhỏ.
- **Normal:** Nội dung văn bản thông thường.

Việc sử dụng Heading Styles giúp tạo mục lục tự động và duy trì định dạng thống nhất.

### 3.2. Khắc phục lỗi gán nhầm Heading Style

**Tình huống:** Một đoạn văn thông thường bị gán nhầm thành Heading 1 hoặc Heading 2, khiến đoạn văn xuất hiện trong mục lục.

**Cách xử lý:**

1. Bôi đen đoạn văn bị định dạng sai.
2. Nhấn tổ hợp phím `Ctrl + Shift + N`.
3. Word sẽ chuyển đoạn văn về kiểu **Normal**.
4. Kiểm tra lại cỡ chữ, khoảng cách và vị trí của đoạn văn.
5. Cập nhật mục lục tự động nếu cần.

**Kết quả:** Đoạn văn trở về kiểu nội dung thông thường, không còn được nhận diện là tiêu đề theo Heading Style.

### 3.3. Kiểm thử hệ thống

| Mã kiểm thử | Nội dung kiểm thử | Kết quả mong đợi |
|---|---|---|
| TC01 | Thêm hồ sơ bệnh nhân mới | Hồ sơ được lưu với PatientID duy nhất. |
| TC02 | Tìm kiếm theo PatientID | Hiển thị đúng hồ sơ tương ứng. |
| TC03 | Lọc dữ liệu khi một số ô bị trống | AutoFilter vẫn hoạt động trên toàn bộ bảng. |
| TC04 | Người không có quyền truy cập bệnh án | Hệ thống từ chối truy cập. |
| TC05 | Tạo biểu đồ Donut | Biểu đồ hiển thị đúng số lượng và tỷ lệ. |
| TC06 | Nhấn Ctrl + Shift + N với đoạn văn bị gán nhầm Heading | Đoạn văn được chuyển về Normal. |
| TC07 | Khôi phục dữ liệu từ bản sao lưu | Dữ liệu được khôi phục theo quy trình đã kiểm thử. |

---

phân quyền, mã hóa, nhật ký hoạt động và sao lưu để bảo vệ dữ liệu y tế.

Đây là nền tảng ban đầu để phát triển hệ thống quản lý phòng khám an toàn, hiệu quả và có khả năng mở rộng.
