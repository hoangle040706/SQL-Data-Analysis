### 1. 🗺️ Data Schema (Sơ đồ dữ liệu)

<p align="center">
  <!-- Lưu ý: Đảm bảo đường dẫn src khớp với tên file ảnh và thư mục trên GitHub của bạn -->
  <img src="1. E_Commerce\image\xomdata_dataset 2 - xomdata_dataset - e_commerce.png" alt="E-Commerce Data Schema" width="85%">
</p>

---

### 2. 📖 Data Dictionary (Từ điển dữ liệu)

Dưới đây là chi tiết ý nghĩa kinh doanh của các trường dữ liệu quan trọng trong hệ thống:

**Bảng `region` (Thông tin khu vực địa lý)**

| Tên cột | Kiểu dữ liệu | Ràng buộc | Mô tả (Ý nghĩa kinh doanh) |
| :--- | :--- | :--- | :--- |
| **region_code** | `nvarchar(10)` | **Khóa chính (PK)** | Mã định danh duy nhất cho từng khu vực/vùng địa lý |
| **city** | `nvarchar(50)` | Null | Tên thành phố |
| **state** | `nvarchar(50)` | Null | Tên bang/tỉnh thành |
| **country** | `nvarchar(50)` | Null | Tên quốc gia |
| **country_latitude** | `decimal(10,6)` | Null | Vĩ độ của quốc gia (Dùng để vẽ biểu đồ bản đồ Map) |
| **country_longitude** | `decimal(10,6)` | Null | Kinh độ của quốc gia (Dùng để vẽ biểu đồ bản đồ Map) |
| **region** | `nvarchar(50)` | Null | Khu vực cụ thể (Ví dụ: Đông Nam Á, Bắc Mỹ...) |
| **market** | `nvarchar(50)` | Null | Phân khúc thị trường rộng lớn hơn (Ví dụ: Châu Á Thái Bình Dương - APAC) |

