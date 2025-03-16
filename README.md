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
- **Kích thước**: Gồm nhiều cột với thông tin về hành vi mua hàng của khách hàng.

### 📊 Data Structure & Relationships
#### 1️⃣ **Tables Used**
- **Dataset chính**: Chứa thông tin của khách hàng, đơn hàng, và các đặc điểm khác.

#### 2️⃣ **Table Schema & Data Snapshot**
| Column Name               | Data Type | Description                                           |
|---------------------------|-----------|-------------------------------------------------------|
| `CustomerID`             | INT       | Unique ID của khách hàng                             |
| `Churn`                  | INT       | Flag churn (1 = rời bỏ, 0 = hoạt động)               |
| `Tenure`                 | INT       | Số tháng khách hàng sử dụng dịch vụ                 |
| `PreferredLoginDevice`   | TEXT      | Thiết bị đăng nhập ưu tiên                           |
| `CityTier`               | INT       | Phân loại thành phố                                  |
| `SatisfactionScore`      | INT       | Điểm hài lòng của khách hàng                         |
| `OrderCount`             | INT       | Số đơn hàng đã đặt                                   |
| `DaySinceLastOrder`      | INT       | Số ngày kể từ lần đặt hàng gần nhất                 |
| `Complain`               | INT       | Phản ánh khiếu nại của khách hàng                   |
| `CashbackAmount`         | FLOAT     | Số tiền hoàn lại trung bình                         |

---

## ⚒️ Main Process

### 1️⃣ **Data Cleaning & Preprocessing**
- Kiểm tra dữ liệu thiếu (`Tenure`, `HourSpendOnApp` có giá trị NaN).
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

