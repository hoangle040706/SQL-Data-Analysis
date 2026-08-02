### 🗺️ Data Schema
%%{init: {'theme': 'base', 'themeVariables': { 'primaryColor': '#e1f5fe', 'edgeLabelBackground':'#ffffff', 'tertiaryColor': '#fff'}}}%%
erDiagram
    
    %% Định nghĩa bảng CUSTOMER (Dimension)
    CUSTOMER {
        nvarchar(20) customer_id PK
        nvarchar(50) first_name
        nvarchar(50) last_name
        date birth_date
        nvarchar(20) marital_status
        nvarchar(1) gender
        nvarchar(100) email_address
        int annual_income
        nvarchar(50) education_level
        nvarchar(50) occupation
        nvarchar(1) home_owner
    }
    
    %% Định nghĩa bảng PRODUCT (Dimension)
    PRODUCT {
        nvarchar(10) product_code PK
        nvarchar(255) product
        nvarchar(50) category
        nvarchar(50) subcategory
    }

    %% Định nghĩa bảng REGION (Dimension)
    REGION {
        nvarchar(10) region_code PK
        nvarchar(50) city
        nvarchar(50) state
        nvarchar(50) country
        decimal(10) country_latitude
        decimal(10) country_longitude
        nvarchar(50) region
        nvarchar(50) market
    }

    %% Định nghĩa bảng ECOM_SALES (Fact)
    ECOM_SALES {
        int row_id PK
        nvarchar(50) order_id
        date order_date
        nvarchar(20) customer_id FK
        nvarchar(50) segment
        nvarchar(10) region_code FK
        nvarchar(10) product_code FK
        int quantity
        decimal(10) sales
        decimal(10) discount
        decimal(10) profit
    }
```
```html
### 🗺️ Data Schema (Sơ đồ dữ liệu)

<p align="center">
  <img src="1. E_Commerce\image\xomdata_dataset 2 - xomdata_dataset - e_commerce.png" alt="E_Commerce Schema" width="80%">
</p>

### 2. Từ điển dữ liệu (Data Dictionary) cho bảng `region`

Bạn hãy thêm bảng này nối tiếp vào các bảng từ điển dữ liệu trước đó nhé:

### 📖 Data Dictionary: Bảng `region`

| Tên cột | Kiểu dữ liệu | Ràng buộc | Mô tả (Ý nghĩa kinh doanh) |
| :--- | :--- | :--- | :--- |
| **region_code** | `nvarchar(10)` | **Khóa chính (PK)** | Mã định danh duy nhất cho từng khu vực/vùng địa lý |
| **city** | `nvarchar(50)` | Null | Tên thành phố |
| **state** | `nvarchar(50)` | Null | Tên bang/tỉnh thành |
| **country** | `nvarchar(50)` | Null | Tên quốc gia |
| **country_latitude** | `decimal(10,6)` | Null | Vĩ độ của quốc gia (Dùng để vẽ biểu đồ bản đồ) |
| **country_longitude** | `decimal(10,6)` | Null | Kinh độ của quốc gia (Dùng để vẽ biểu đồ bản đồ) |
| **region** | `nvarchar(50)` | Null | Khu vực cụ thể (Ví dụ: Đông Nam Á, Bắc Mỹ...) |
| **market** | `nvarchar(50)` | Null | Phân khúc thị trường rộng lớn hơn (Ví dụ: Châu Á Thái Bình Dương - APAC) |