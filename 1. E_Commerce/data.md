### 1. Data Schema (Sơ đồ dữ liệu)

<p align="center">
  <!-- Lưu ý: Đảm bảo đường dẫn src khớp với tên file ảnh và thư mục trên GitHub của bạn -->
  <img src=".\image\xomdata_dataset 2 - xomdata_dataset - e_commerce.png" alt="E-Commerce Data Schema" width="85%">
</p>

---

### 2. Data Dictionary (Từ điển dữ liệu)

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

---

### 3. Dữ liệu mẫu (Sample Data): 

**Bảng `region`**

| region_code | city | state | country | country_latitude | country_longitude | region | market |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| R0001 | Jalalabad | Nangarhar | Afghanistan | 33.939110 | 67.709953 | Southern Asia | Asia Pacific |
| R0002 | Herat | Hirat | Afghanistan | 33.939110 | 67.709953 | Southern Asia | Asia Pacific |
| R0003 | Kabul | Kabul | Afghanistan | 33.939110 | 67.709953 | Southern Asia | Asia Pacific |
| R0004 | Kandahar | Kandahar | Afghanistan | 33.939110 | 67.709953 | Southern Asia | Asia Pacific |
| R0005 | Elbasan | Elbasan | Albania | 41.153332 | 20.168331 | Southern Europe | Europe |

**Bảng `customer`**

| customer_id | first_name | last_name | birth_date | marital_status | gender | email_address | annual_income | education_level | occupation | home_owner |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| AA-10315102 | Bryant | Vazquez | 1980-01-21 | M | M | bryant6@hotmail.com | 20000 | Bachelors | Clerical | Y |
| AA-10315120 | Jon | Rubio | 1998-12-01 | S | M | jon6@hotmail.com | 20000 | Bachelors | Clerical | N |
| AA-10315139 | Jésus | Hernandez | 1973-01-27 | M | M | jésus20@hotmail.com | 30000 | Bachelors | Clerical | Y |
| AA-103151402 | Marcus | Taylor | 1999-01-24 | S | M | marcus31@hotmail.com | 40000 | Partial College | Skilled Manual | N |
| AA-103151404 | Jesse | Dominguez | 1968-09-11 | S | M | jesse34@hotmail.com | 40000 | High School | Skilled Manual | Y |

**Bảng `ecom_sales`**
| row_id | order_id | order_date | customer_id | segment | region_code | product_code | quantity | sales | discount | profit |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | MX-2015-SC2057582-42279 | 2023-10-02 | SC-2057582 | Consumer | R2140 | P000409 | 3 | 48.00 | 0.00 | 19.20 |
| 2 | MX-2013-KW1657028-41562 | 2021-10-15 | KW-1657028 | Consumer | R0816 | P000147 | 8 | 48.00 | 0.00 | 7.20 |
| 3 | MX-2013-KW1657028-41562 | 2021-10-15 | KW-1657028 | Consumer | R0816 | P000395 | 2 | 16.00 | 0.00 | 8.00 |
| 4 | MX-2013-KW1657028-41562 | 2021-10-15 | KW-1657028 | Consumer | R0816 | P002140 | 4 | 28.00 | 0.00 | 14.00 |
| 5 | MX-2013-KW1657028-41562 | 2021-10-15 | KW-1657028 | Consumer | R0816 | P002828 | 2 | 20.00 | 0.00 | 10.00 |

**Bảng `product`**

| product_code | product | category | subcategory |
| :--- | :--- | :--- | :--- |
| P000001 | Soap & Glory Endless Glove Moisturizing Hand Cream | Body care | bath oils, bubbles and soaks |
| P000002 | Pantene Pro-V Truly Relaxed Hair Lightweight Conditioner | Body care | bath oils, bubbles and soaks |
| P000003 | Estée Lauder Pure Color Envy Irresistible | Body care | bath oils, bubbles and soaks |
| P000004 | Gold Infinity Bracelet | Body care | body moisturizers |
| P000005 | Deborah Lippmann Nail Polish Lady is a Tramp Classic Red | Body care | body moisturizers |
