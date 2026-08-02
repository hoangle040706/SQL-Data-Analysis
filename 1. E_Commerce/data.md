### 1. 🗺️ Data Schema (Sơ đồ dữ liệu)

<p align="center">
  <!-- Lưu ý: Đảm bảo đường dẫn src khớp với tên file ảnh và thư mục trên GitHub của bạn -->
  <img src=".\image\xomdata_dataset 2 - xomdata_dataset - e_commerce.png" alt="E-Commerce Data Schema" width="85%">
</p>

---

### 2. 📖 Data Dictionary (Từ điển dữ liệu)

Dưới đây là chi tiết ý nghĩa kinh doanh của các trường dữ liệu quan trọng trong hệ thống:

**Bảng `region` (Thông tin khu vực địa lý)**

| Tên cột | Kiểu dữ liệu | Ràng buộc | Mô tả (Ý nghĩa kinh doanh) |
| :--- | :--- | :--- | :--- |
| **region_code** | `nvarchar(10)` | **Khóa chính (PK) / Not Null** | Mã định danh duy nhất cho từng khu vực/vùng địa lý |
| **city** | `nvarchar(50)` | Null | Tên thành phố |
| **state** | `nvarchar(50)` | Null | Tên bang/tỉnh thành |
| **country** | `nvarchar(50)` | Null | Tên quốc gia |
| **country_latitude** | `decimal` | Null | Vĩ độ của quốc gia |
| **country_longitude** | `decimal` | Null | Kinh độ của quốc gia |
| **region** | `nvarchar(50)` | Null | Khu vực cụ thể |
| **market** | `nvarchar(50)` | Null | Phân khúc thị trường rộng lớn hơn |

**Bảng `customer` (Thông tin nhân khẩu học của khách hàng)**

| Tên cột | Kiểu dữ liệu | Ràng buộc | Mô tả (Ý nghĩa kinh doanh) |
| :--- | :--- | :--- | :--- |
| **customer_id** | `nvarchar(20)` | **Khóa chính (PK) / Not Null** | Mã định danh duy nhất của mỗi khách hàng |
| **first_name** | `nvarchar(50)` | Null | Tên của khách hàng |
| **last_name** | `nvarchar(50)` | Null | Họ của khách hàng |
| **birth_date** | `date` | Null | Ngày tháng năm sinh |
| **marital_status** | `nvarchar(20)` | Null | Tình trạng hôn nhân |
| **gender** | `nvarchar(1)` | Null | Giới tính của khách hàng (M/F) |
| **email_address** | `nvarchar(100)`| Null | Địa chỉ email liên hệ |
| **annual_income** | `int` | Null | Thu nhập hàng năm của khách hàng |
| **education_level**| `nvarchar(50)` | Null | Trình độ học vấn |
| **occupation** | `nvarchar(50)` | Null | Nghề nghiệp hiện tại |
| **home_owner** | `nvarchar(1)` | Null | Tình trạng sở hữu nhà |

**Bảng `ecom_sales` (Thông tin chi tiết giao dịch & số liệu bán hàng)**

| Tên cột | Kiểu dữ liệu | Ràng buộc | Mô tả (Ý nghĩa kinh doanh) |
| :--- | :--- | :--- | :--- |
| **row_id** | `int` | **Khóa chính (PK) / Not Null** | ID định danh duy nhất cho mỗi dòng chi tiết của đơn hàng |
| **order_id** | `nvarchar(50)` | Null | Mã đơn hàng |
| **order_date** | `date` | Null | Ngày thực hiện giao dịch |
| **customer_id** | `nvarchar(20)` | **Khóa ngoại (FK)** / Null | Mã khách hàng |
| **segment** | `nvarchar(50)` | Null | Phân khúc của đơn hàng |
| **region_code** | `nvarchar(10)` | **Khóa ngoại (FK)** / Null | Mã khu vực giao hàng |
| **product_code**| `nvarchar(10)` | **Khóa ngoại (FK)** / Null | Mã sản phẩm được bán |
| **quantity** | `int` | Null | Số lượng sản phẩm được mua trong giao dịch này |
| **sales** | `decimal` | Null | Doanh thu từ giao dịch |
| **discount** | `decimal` | Null | Mức giảm giá được áp dụng |
| **profit** | `decimal` | Null | Lợi nhuận từ giao dịch |

**Bảng `product` (Thông tin danh mục sản phẩm)**

| Tên cột | Kiểu dữ liệu | Ràng buộc | Mô tả (Ý nghĩa kinh doanh) |
| :--- | :--- | :--- | :--- |
| **product_code** | `nvarchar(10)` | **Khóa chính (PK) / Not Null** | Mã định danh duy nhất cho mỗi sản phẩm |
| **product** | `nvarchar(255)`| Null | Tên hiển thị chi tiết của sản phẩm |
| **category** | `nvarchar(50)` | Null | Danh mục ngành hàng chính |
| **subcategory** | `nvarchar(50)` | Null | Danh mục phụ |
