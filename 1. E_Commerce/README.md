# E_Commerce Analysis
![SQL Server](https://img.shields.io/badge/SQL_Server-CC2927?style=for-the-badge&logo=microsoftsqlserver&logoColor=white)
![Xomdata](https://img.shields.io/badge/Xomdata-0052CC?style=for-the-badge)

---

## Table of Contents
- [E\_Commerce Analysis](#e_commerce-analysis)
  - [Table of Contents](#table-of-contents)
  - [1. Project Overview](#1-project-overview)
  - [2. Data Analysis: Business Questions, SQL Queries \& Insights](#2-data-analysis-business-questions-sql-queries--insights)


---

## 1. Project Overview

Dự án này phân tích tập dữ liệu từ Sàn thương mại điện tử B2C đa market (Xóm E-Com), tập trung vào các mặt hàng Office, Furniture và Tech. Mục tiêu của dự án là khai phá dữ liệu đơn hàng online, phân khúc khách hàng và hiệu suất bán hàng theo từng khu vực địa lý.

---

## 2. Data Analysis: Business Questions, SQL Queries & Insights

Phần này mô tả chi tiết quá trình khám phá dữ liệu. Mỗi câu hỏi kinh doanh đều đi kèm với một giải pháp truy vấn SQL và các đề xuất chiến lược rút ra từ kết quả.

--- 

### Câu 1: Em ơi, CEO hỏi nhanh: tổng doanh thu từ trước đến nay là bao nhiêu, và tổng profit là bao nhiêu?Chị cần 2 con số để trả lời CEO trong 10 phút nữa. <!-- omit from toc -->
— Head of Finance

**SQL Queries**
```sql
SELECT SUM(es.sales) AS tong_doanh_thu,
       SUM(es.profit) AS tong_profit
FROM e_commerce.ecom_sales AS es;
```
**Query Results:**

<img width="375" height="73" alt="image" src="https://github.com/user-attachments/assets/9eb693bd-d37b-416b-9819-f1036d834fbd" />

**Insight:**



-- Anh cần tổng doanh thu + tổng profit theo từng segment khách hàng (Consumer / Corporate / Home Office)
-- để so sánh độ đóng góp của 3 nhóm. Sắp theo doanh thu giảm dần.
-- — Head of Growth
SELECT es.segment,
       SUM(es.sales + es.profit) AS tong_dt_profit
FROM e_commerce.ecom_sales AS es
GROUP BY es.segment
ORDER BY SUM(es.sales) DESC;

-- Chị đang planning restock, cần top 10 sản phẩm bán chạy nhất theo số lượng (không tính doanh thu).
-- Gửi chị: mã sản phẩm, tên sản phẩm, tổng số lượng đã bán.
-- — Head of Merchandising
SELECT TOP(10)
       p.product_code AS ma_sp,
       p.product AS ten_sp,
       SUM(es.quantity) AS so_luong
FROM e_commerce.product AS p
JOIN e_commerce.ecom_sales AS es ON p.product_code = es.product_code
GROUP BY p.product_code, p.product
ORDER BY so_luong DESC;

-- Team CRM cần bảng phân bố khách hàng theo giới tính và nghề nghiệp để chọn targeting cho campaign email.
-- Output pivot table: nghề nghiệp (hàng) × giới tính (cột) × số khách.
-- — Head of CRM
SELECT c.occupation AS nghe_nghiep,
       SUM(CASE WHEN c.gender = 'M' THEN 1 ELSE 0 END) AS nam,
       SUM(CASE WHEN c.gender = 'F' THEN 1 ELSE 0 END) AS nu
FROM e_commerce.customer AS c
GROUP BY c.occupation
ORDER BY c.occupation;

-- Em ơi, chị muốn biết mức discount trung bình đang áp dụng theo từng category sản phẩm.
-- Trả discount dạng %. Category nào đang discount nhiều nhất?
-- — CMO
SELECT p.category AS loai_sp,
       AVG(es.discount) * 100.0 AS tb_sdung_discount
FROM e_commerce.product AS p
JOIN e_commerce.ecom_sales AS es ON p.product_code = es.product_code
GROUP BY p.category
ORDER BY tb_sdung_discount DESC;

-- Chị cần profit margin (%) theo từng market (US, EMEA, APAC, LATAM) để
-- xem market nào đang lãi nhiều, market nào lỗ. Chỉ xét market có ít nhất 100 đơn để số ổn định.
-- Margin = profit / doanh thu × 100.
-- — CFO
SELECT r.market,
       SUM(es.profit) / SUM(es.sales) * 100.0 AS profit_margin
FROM e_commerce.region AS r
JOIN e_commerce.ecom_sales AS es ON r.region_code = es.region_code
GROUP BY r.market
HAVING COUNT(*) >= 100
ORDER BY profit_margin DESC;

-- Anh nghi ngờ có đơn discount quá mạnh dẫn tới lỗ. Em check giúp anh: với mỗi segment,
-- có bao nhiêu đơn bị lỗ (profit âm), tổng số tiền lỗ bao nhiêu, và chiếm bao nhiêu % tổng số đơn của segment đó?
-- Anh cần đưa cảnh báo cho team pricing.
-- — CFO
SELECT es.segment,
       SUM(CASE WHEN es.profit < 0 THEN 1 ELSE 0 END) AS tong_don_lo,
       SUM(CASE WHEN es.profit < 0 THEN es.profit ELSE 0 END) AS tong_tien_lo,
       ROUND(SUM(CASE WHEN es.profit < 0 THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS don_lo_pct
FROM e_commerce.ecom_sales AS es
GROUP BY es.segment;

-- Chị cần danh sách khách hàng chi tiêu top 50 trong lifetime của họ, nhưng đã
-- 180 ngày không mua hàng. Đây là nhóm VIP sắp churn, team retention cần target ngay.
-- Gửi chị: tên khách, tổng lifetime spend, ngày mua gần nhất.
-- — Head of Retention
SELECT c.first_name + ' ' + c.last_name AS customer_name,
       SUM(es.sales) AS lifetime_spend,
       MAX(es.order_date) AS last_order_date
FROM e_commerce.customer AS c
JOIN e_commerce.ecom_sales AS es ON c.customer_id = es.customer_id
GROUP BY c.customer_id, c.first_name, c.last_name
HAVING DATEDIFF(DAY, MAX(es.order_date), GETDATE()) >= 180
ORDER BY lifetime_spend DESC;

-- Anh cần tỷ trọng doanh thu Consumer vs Corporate ở mỗi category để hiểu category nào thiên
-- về B2C, category nào thiên về B2B. Output: category, % Consumer, % Corporate (tổng = 100%).
-- — Head of Growth
WITH temp AS (
    SELECT p.category, es.segment, es.sales
    FROM e_commerce.product AS p
    JOIN e_commerce.ecom_sales AS es ON p.product_code = es.product_code
    WHERE es.segment IN ('Consumer', 'Corporate')
)
SELECT category,
       ROUND(SUM(CASE WHEN segment = 'Consumer' THEN sales ELSE 0 END) / SUM(sales) * 100.0, 2) AS consumer_pct,
       ROUND(SUM(CASE WHEN segment = 'Corporate' THEN sales ELSE 0 END) / SUM(sales) * 100.0, 2) AS corporate_pct
FROM temp
GROUP BY category
ORDER BY category;

-- Chị cần tổng hợp: top 15 quốc gia theo doanh thu, mỗi nước xem có bao nhiêu region active,
-- bao nhiêu customer, tổng doanh thu. Sắp theo doanh thu giảm dần.
-- — Head of International
SELECT TOP(15)
       r.country,
       COUNT(DISTINCT r.region) AS active_regions,
       COUNT(DISTINCT es.customer_id) AS total_customers,
       SUM(es.sales) AS total_sales
FROM e_commerce.region AS r
JOIN e_commerce.ecom_sales AS es ON r.region_code = es.region_code
GROUP BY r.country
ORDER BY total_sales DESC;

-- Anh cần doanh thu theo tháng + so sánh với cùng tháng năm trước (YoY growth %)
-- 24 tháng gần nhất. Board muốn thấy trend tăng trưởng rõ ràng. Output: year_month,
-- doanh thu, doanh thu cùng kỳ năm trước, % tăng trưởng YoY.
-- — CFO
WITH temp AS (
    SELECT FORMAT(es.order_date, 'yyyy-MM') AS year_month,
           SUM(es.sales) AS doanh_thu
    FROM e_commerce.ecom_sales AS es
    WHERE es.order_date >= DATEADD(MONTH, -24, GETDATE())
    GROUP BY FORMAT(es.order_date, 'yyyy-MM')
),
temp2 AS (
    SELECT *, LAG(doanh_thu, 12) OVER (ORDER BY year_month) AS dt_thang_nam_trc
    FROM temp
)
SELECT *, (doanh_thu - dt_thang_nam_trc) * 100.0 / dt_thang_nam_trc AS yoy_pct
FROM temp2
ORDER BY yoy_pct DESC;

WITH temp AS (
    SELECT FORMAT(es.order_date, 'yyyy-MM') AS year_month,
           SUM(es.sales) AS doanh_thu
    FROM e_commerce.ecom_sales AS es
    WHERE es.order_date >= DATEADD(MONTH, -56, GETDATE())
    GROUP BY FORMAT(es.order_date, 'yyyy-MM')
),
temp2 AS (
    SELECT *, LAG(doanh_thu, 12) OVER (ORDER BY year_month) AS dt_thang_nam_trc
    FROM temp
)
SELECT *, (doanh_thu - dt_thang_nam_trc) * 100.0 / dt_thang_nam_trc AS yoy_pct
FROM temp2
ORDER BY yoy_pct DESC;

-- Em cho chị top 3 sản phẩm có lifetime profit cao nhất trong mỗi category.
-- Dùng để làm product spotlight trong báo cáo Q4. Output: category,
-- tên sản phẩm, tổng profit, thứ hạng (1-3) trong category.
-- — Head of Merchandising
WITH temp AS (
    SELECT p.category, p.product,
           SUM(es.profit) AS tong_profit,
           ROW_NUMBER() OVER (PARTITION BY p.category ORDER BY SUM(es.profit) DESC) AS hang
    FROM e_commerce.product AS p
    JOIN e_commerce.ecom_sales AS es ON p.product_code = es.product_code
    GROUP BY p.category, p.product
)
SELECT *
FROM temp
WHERE hang <= 3
ORDER BY category;

-- Chị muốn hiểu: bao nhiêu % khách hàng có mua ở nhiều market khác nhau
-- (VD: mua ở US rồi sau đó mua ở APAC)? Xác định cho mỗi khách:
-- market họ mua lần đầu, và liệu họ có mua ở market khác sau đó không.
-- Output 1 con số % + top 20 khách cross-market tiêu nhiều nhất.
-- — Head of International
WITH temp AS (
    SELECT es.customer_id, r.market, es.order_date, es.sales
    FROM e_commerce.ecom_sales AS es
    JOIN e_commerce.region AS r ON es.region_code = r.region_code
),
temp2 AS (
    SELECT *,
           ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date) AS rn
    FROM temp
),
temp3 AS (
    SELECT customer_id,
           MAX(CASE WHEN rn = 1 THEN market END) AS first_market,
           COUNT(DISTINCT market) AS so_market,
           SUM(sales) AS tong_sales
    FROM temp2
    GROUP BY customer_id
)
SELECT TOP(20)
       customer_id, first_market, tong_sales
FROM temp3
WHERE so_market > 1
ORDER BY tong_sales DESC;

-- Team CRM cần phân khúc khách hàng theo RFM (Recency: số ngày từ đơn gần nhất,
-- Frequency: số đơn, Monetary: tổng chi tiêu). Chia mỗi chỉ số thành 5 mức
-- (1-5, 5 là tốt nhất). Tính điểm RFM tổng hợp.
-- Gửi chị top 20 khách có RFM cao nhất để team chạy loyalty program.
-- — Head of CRMALTER
WITH temp AS (
    SELECT es.customer_id,
           MAX(es.order_date) AS ngay_gan_nhat,
           COUNT(DISTINCT es.order_id) AS so_don,
           SUM(es.sales) AS chi_tieu
    FROM e_commerce.ecom_sales AS es
    GROUP BY es.customer_id
),
temp2 AS (
    SELECT *,
           DATEDIFF(DAY, ngay_gan_nhat, GETDATE()) AS so_ngay
    FROM temp
),
temp3 AS (
    SELECT customer_id,
           NTILE(5) OVER (ORDER BY so_don) AS frequency,
           NTILE(5) OVER (ORDER BY chi_tieu) AS monetary,
           NTILE(5) OVER (ORDER BY so_ngay DESC) AS recency
    FROM temp2
)
SELECT TOP (20)
       customer_id,
       frequency,
       monetary,
       recency,
       frequency + monetary + recency AS total_crm
FROM temp3
ORDER BY total_crm DESC;

-- Team product đang build recommendation. Em cho anh top 15 cặp sản phẩm hay
-- xuất hiện cùng trong 1 đơn hàng. Chỉ xét đơn có từ 2 sản phẩm khác nhau trở lên.
-- Output: sản phẩm A, sản phẩm B, số lần xuất hiện cùng.
-- — Head of Product
SELECT TOP (15)
       p1.product AS san_pham_A,
       p2.product AS san_pham_B,
       COUNT(*) AS so_lan_xuat_hien_cung
FROM e_commerce.ecom_sales AS es1
JOIN e_commerce.ecom_sales AS es ON es1.order_id = es2.order_id AND es1.product_code < es2.product_code
JOIN e_commerce.product AS p ON es1.product_code = p1.product_code
JOIN e_commerce.product AS p2 ON es2.product_code = p2.product_code
GROUP BY p1.product, p2.product
ORDER BY so_lan_xuat_hien_cung DESC, san_pham_A, san_pham_B;


