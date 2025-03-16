# 📊 Project: Customer Churn Prediction & Segmentation for an E-Commerce Company | Python, Machine Learning 

## 📑 Table of Contents
1. [Background & Overview](#background--overview)
2. [Dataset Description & Data Structure](#dataset-description--data-structure)
3. [Main Process](#main-process)
4. [Final Conclusion & Recommendations](#final-conclusion--recommendations)

---

## 📌 Background & Overview

### 🎯 Objective:
Dự án này nhằm phân tích hành vi khách hàng và dự đoán khả năng rời bỏ dịch vụ (**churn**). Dựa trên dữ liệu giao dịch của khách hàng, chúng tôi muốn:

✔️ Xác định các yếu tố ảnh hưởng đến việc khách hàng rời bỏ dịch vụ.  
✔️ Cung cấp các insight giúp doanh nghiệp tăng retention rate.  
✔️ Xây dựng mô hình Machine Learning để dự đoán churn.  

### 👤 Who is this project for?
✔️ Data Analysts & Business Analysts.  
✔️ Các nhà quản lý và chiến lược kinh doanh.  
✔️ Bộ phận Marketing & CSKH để tối ưu chiến lược giữ chân khách hàng.  

---

## 📂 Dataset Description & Data Structure

### 📌 Data Source
- **Nguồn**: Cung cấp bởi công ty Ecommerce.
- **Định dạng**: `.xlsx`
- **Kích thước**: Dữ liệu gồm 5630 dòng và 20 cột dữ liệu

### 📊 Data Structure & Relationships
#### 1️⃣ **Tables Used**
- **Dataset chính**: Chứa thông tin của khách hàng, đơn hàng, và các đặc điểm khác.

#### 2️⃣ **Table Schema & Data Snapshot**
| Tên Cột                          | Kiểu Dữ Liệu | Mô Tả                                              |
|-----------------------------------|-------------|----------------------------------------------------|
| `CustomerID`                     | INT         | ID duy nhất của khách hàng                         |
| `Churn`                          | INT         | Cờ churn (1 = rời bỏ, 0 = hoạt động)               |
| `Tenure`                         | INT         | Số tháng khách hàng đã sử dụng dịch vụ             |
| `PreferredLoginDevice`           | TEXT        | Thiết bị đăng nhập ưu tiên của khách hàng         |
| `CityTier`                       | INT         | Phân loại thành phố                                |
| `WarehouseToHome`                | FLOAT       | Khoảng cách từ kho đến nhà khách hàng              |
| `PreferredPaymentMode`           | TEXT        | Phương thức thanh toán ưa thích                    |
| `Gender`                         | TEXT        | Giới tính của khách hàng                           |
| `HourSpendOnApp`                 | FLOAT       | Số giờ sử dụng ứng dụng hoặc trang web             |
| `NumberOfDeviceRegistered`       | INT         | Tổng số thiết bị đã đăng ký                        |
| `PreferedOrderCat`               | TEXT        | Danh mục đơn hàng ưa thích trong tháng gần nhất    |
| `SatisfactionScore`              | INT         | Điểm hài lòng của khách hàng                       |
| `MaritalStatus`                  | TEXT        | Tình trạng hôn nhân của khách hàng                 |
| `NumberOfAddress`                | INT         | Tổng số địa chỉ đã đăng ký                         |
| `Complain`                       | INT         | Có khiếu nại trong tháng gần nhất hay không        |
| `OrderAmountHikeFromLastYear`    | FLOAT       | Tỷ lệ tăng giá trị đơn hàng so với năm trước       |
| `CouponUsed`                     | INT         | Tổng số phiếu giảm giá đã sử dụng trong tháng gần nhất |
| `OrderCount`                     | INT         | Tổng số đơn hàng đã đặt trong tháng gần nhất       |
| `DaySinceLastOrder`              | INT         | Số ngày kể từ đơn hàng gần nhất                    |
| `CashbackAmount`                 | FLOAT       | Số tiền hoàn lại trung bình trong tháng gần nhất   |

---

## ⚒️ Main Process

### 1️⃣ **Data Cleaning & Preprocessing**
- Kiểm tra dữ liệu thiếu 
### 2️⃣ Missing Data Analysis

Để kiểm tra dữ liệu bị thiếu, chúng tôi sử dụng hàm sau:

```python
import pandas as pd

def count_NaN_values(df):
    """
    Hàm để đếm số lượng giá trị NaN trong DataFrame.

    Tham số:
    df (pandas.DataFrame): DataFrame để đếm giá trị NaN.
    """
    nan_dict = {}
    for col in df.columns:
        num_NaN = df[col].isna().sum()
        nan_dict[col] = num_NaN

    NaN_table = pd.DataFrame.from_dict(nan_dict, orient='index', columns=['Number of NaN']).reset_index()
    NaN_table.rename(columns={

- Chuyển đổi kiểu dữ liệu phù hợp.
- Xử lý outliers nếu cần.

### 2️⃣ **Exploratory Data Analysis (EDA)**
- Phân tích phân bố dữ liệu theo nhóm churn & active.
- Trực quan hóa dữ liệu bằng biểu đồ.
- Xác định mối quan hệ giữa `Churn` và các biến độc lập (`SatisfactionScore`, `OrderCount`, `DaySinceLastOrder`).

### 3️⃣ **SQL/Python Analysis & Machine Learning**
- **Chia tập dữ liệu**: Sử dụng `train_test_split` để tách tập train/test.
- **Thử nghiệm các mô hình**:
  ✔️ Logistic Regression  
  ✔️ Random Forest  
  ✔️ XGBoost  
- **Đánh giá mô hình**:
  - AUC-ROC
  - F1-score
  - XGBoost đạt độ chính xác cao nhất.

---

## 🔎 Final Conclusion & Recommendations

### 📌 Key Takeaways:
✔️ Khách hàng có **điểm hài lòng thấp** và **ít đơn hàng** có khả năng churn cao.  
✔️ **XGBoost đạt độ chính xác cao nhất** với **AUC = 0.89**.  
✔️ **Tăng cường chương trình ưu đãi** và **CSKH tốt hơn** có thể giảm churn.  

👉🏻 **Khuyến nghị:**
1. Cải thiện chất lượng dịch vụ để tăng điểm hài lòng của khách hàng.
2. Cung cấp ưu đãi đặc biệt cho khách hàng có nguy cơ churn cao.
3. Theo dõi sát hành vi của khách hàng để đưa ra chiến lược cá nhân hóa hợp lý.

---

## 🚀 Cách chạy dự án

1. **Cài đặt các thư viện cần thiết**:
   ```bash
   pip install -r requirements.txt
   ```
2. **Chạy notebook**:
   - Mở file `churn_prediction.ipynb` và chạy từng cell để xem kết quả.

---

## 📌 Liên hệ
🔗 [LinkedIn của bạn](https://www.linkedin.com/in/your-profile/)  
📧 Email: your.email@example.com

