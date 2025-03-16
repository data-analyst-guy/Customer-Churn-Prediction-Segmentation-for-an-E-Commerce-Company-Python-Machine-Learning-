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
```
- Chuyển đổi kiểu dữ liệu phù hợp.
Sau khi kiểm tra dữ liệu thiếu, chúng tôi tiến hành xử lý như sau:

#### 📌 Xử lý cột `Tenure`
- Vẽ biểu đồ phân bố **Tenure** theo trạng thái Churn:

```python
import seaborn as sns
import matplotlib.pyplot as plt

# Lọc dữ liệu cho churn = 0 và churn = 1
churn_0 = df[df['Churn'] == 0]
churn_1 = df[df['Churn'] == 1]

column_to_plot = 'Tenure'

plt.figure(figsize=(10, 6))
sns.histplot(churn_0[column_to_plot], color='green', label='Churn = 0', kde=True, bins=30)
sns.histplot(churn_1[column_to_plot], color='red', label='Churn = 1', kde=True, bins=30)

plt.title(f'Histogram of {column_to_plot} by Churn')
plt.xlabel(column_to_plot)
plt.ylabel('Frequency')
plt.legend()
plt.show()
```

- **Xử lý dữ liệu trống**: Thay thế các giá trị NaN bằng `0`:
```python
df['Tenure'] = df['Tenure'].fillna(0)
```
#### 📌 Xử lý dữ liệu Tenure: Vì sao thay NaN bằng 0?

#### 1️⃣ Giả định dữ liệu bị thiếu do khách hàng mới  
- Tenure là số tháng khách hàng đã gắn bó với công ty.  
- Nếu giá trị bị thiếu, có thể do khách hàng mới đăng ký nhưng chưa có đủ dữ liệu.  
- **Thay thế bằng 0 phản ánh chính xác tình trạng khách hàng mới.**  
🔹 *Ví dụ:* Khách hàng vừa đăng ký nhưng dữ liệu chưa cập nhật → Tenure bị NaN → Gán **0**.  

#### 2️⃣ Tránh sai lệch khi dùng mean hoặc median  
- Nếu thay NaN bằng **mean** hoặc **median**, dữ liệu có thể bị lệch.  
- Khách hàng mới có thể bị gán Tenure lớn hơn thực tế → Dự đoán sai trong mô hình Churn.  
🔹 *Ví dụ:* Nếu mean = 15 tháng, ta đang giả định khách hàng chưa có dữ liệu đã gắn bó trung bình 15 tháng → **Không chính xác!**  

#### 3️⃣ Tương thích với mô hình phân tích Churn  
- **Nhóm khách hàng mới dễ dàng nhận diện** khi thay NaN → 0.  
- Nếu thay bằng trung bình, có thể mất thông tin quan trọng về hành vi của khách hàng mới.  

✅ **Chọn `fillna(0)` giúp dữ liệu chính xác hơn, tránh sai lệch và hỗ trợ phân tích tốt hơn.**
---
#### 📌 Xử lý cột `WarehouseToHome`
- Vẽ biểu đồ phân bố **WarehouseToHome** theo trạng thái Churn:

```python
column_to_plot = 'WarehouseToHome'

plt.figure(figsize=(10, 6))
sns.histplot(churn_0[column_to_plot], color='green', label='Churn = 0', kde=True)
sns.histplot(churn_1[column_to_plot], color='red', label='Churn = 1', kde=True)

plt.title(f'Histogram of {column_to_plot} by Churn')
plt.xlabel(column_to_plot)
plt.ylabel('Frequency')
plt.legend()
plt.show()
```
- **Xử lý dữ liệu trống**: Thay thế các giá trị NaN bằng giá trị trung vị:
```python
df['WarehouseToHome'] = df['WarehouseToHome'].fillna(df['WarehouseToHome'].median())
```
---
#### 📌 Xử lý cột `HourSpendOnApp`
- Vẽ biểu đồ phân bố **HourSpendOnApp** theo trạng thái Churn:

```python
column_to_plot = 'HourSpendOnApp'

plt.figure(figsize=(10, 6))
sns.histplot(churn_0[column_to_plot], color='green', label='Churn = 0', kde=True, bins=30)
sns.histplot(churn_1[column_to_plot], color='red', label='Churn = 1', kde=True, bins=30)

plt.title(f'Histogram of {column_to_plot} by Churn')
plt.xlabel(column_to_plot)
plt.ylabel('Frequency')
plt.legend()
plt.show()
```

- **Xử lý dữ liệu trống**: Do giá trị trung bình là **2.93**, làm tròn thành `3`:
```python
df['HourSpendOnApp'] = df['HourSpendOnApp'].fillna(3)
```
---
#### 📌 Xử lý các cột còn lại (`OrderAmountHikeFromlastYear`, `CouponUsed`, `OrderCount`, `DaySinceLastOrder`)
- Vẽ biểu đồ phân bố:

```python
list_nan_col = ['OrderAmountHikeFromlastYear', 'CouponUsed', 'OrderCount', 'DaySinceLastOrder']

g = sns.FacetGrid(pd.melt(df, id_vars='Churn', value_vars=list_nan_col), col="variable", col_wrap=2, height=4, sharex=False, sharey=False)
g.map_dataframe(sns.histplot, x='value', hue='Churn', kde=True, stat='density', common_norm=False, palette='Set1')

for ax, column in zip(g.axes.flat, list_nan_col):
    ax.set_title(f'Histogram of {column} by Churn')
    ax.set_xlabel(column)
    ax.set_ylabel('Density')

g.add_legend(title='Churn')

plt.tight_layout()
plt.show()
```

- **Xử lý dữ liệu trống**:
- Thay thế **NaN** bằng trung bình (`mean`):  
```python
df['OrderAmountHikeFromlastYear'] = df['OrderAmountHikeFromlastYear'].fillna(df['OrderAmountHikeFromlastYear'].median())
df['OrderCount'] = df['OrderCount'].fillna(df['OrderCount'].median())
```
🔍 Xử lý NaN cho `CouponUsed` và `DaySinceLastOrder`  
📌 Lý do chọn `0` thay vì mean/median:  
✅ **Dữ liệu có phân bố lệch (skewed distribution)** → Mean không phản ánh trung thực.   
✅ **Nhiều giá trị `0` trong dữ liệu** → NaN có khả năng đại diện cho **"không có hoạt động"** thay vì giá trị bị mất.  
✅ **Điền bằng `0` giúp giữ nguyên ý nghĩa thực tế của dữ liệu**:  
   - **`CouponUsed = 0`** → Khách hàng **chưa sử dụng** phiếu giảm giá.  
   - **`DaySinceLastOrder = 0`** → Khách hàng **chưa từng đặt hàng**.  
  - Thay thế **NaN** bằng `0`:
    ```python
    df['CouponUsed'] = df['CouponUsed'].fillna(0)
    df['DaySinceLastOrder'] = df['DaySinceLastOrder'].fillna(0)
    ```
### 2️⃣ **Exploratory Data Analysis (EDA) and Hypothesis Testing**
### 🔍 Kiểm định thống kê giữa các biến và Churn

Để kiểm tra xem các biến có ảnh hưởng đáng kể đến Churn hay không, chúng tôi thực hiện kiểm định thống kê:

### 📊 1️⃣ Kiểm định biến định lượng  
- **Phương pháp:**  
  - **T-test**: Dùng nếu dữ liệu có phân phối chuẩn.  
  - **Mann-Whitney U test**: Dùng nếu dữ liệu không có phân phối chuẩn.  
- **Ý nghĩa:** Nếu `p-value < 0.05`, có sự khác biệt đáng kể giữa nhóm Churn và Non-Churn.  

### 🏷️ 2️⃣ Kiểm định biến định tính  
- **Phương pháp:**  
  - **Chi-Square Test**: Kiểm tra sự khác biệt về tần suất xuất hiện giữa hai nhóm.  
  - **Cramer’s V**: Đo lường mức độ ảnh hưởng của biến định tính lên Churn.  
- **Ý nghĩa:** Nếu `p-value < 0.05`, biến này có ảnh hưởng đáng kể đến Churn.

### 🔍 Kết quả kiểm định thống kê biến định lượng và Churn  

| **Biến**                     | **p-value**         | **Effect Size** |
|------------------------------|--------------------|----------------|
| **Tenure**                   | 2.70 × 10⁻¹⁹⁴     | 0.92 (Rất lớn) |
| **Complain**                 | 1.31 × 10⁻⁷⁸     | 0.67 (Lớn)     |
| **CashbackAmount**           | 2.52 × 10⁻³⁸     | 0.41 (Trung bình - lớn) |
| **DaySinceLastOrder**        | 2.85 × 10⁻³⁸     | 0.40 (Trung bình - lớn) |
| **SatisfactionScore**        | 3.75 × 10⁻¹⁵     | 0.28 (Trung bình) |
| **NumberOfDeviceRegistered** | 3.05 × 10⁻¹⁴     | 0.29 (Trung bình) |
| **CityTier**                 | 1.62 × 10⁻¹⁰     | 0.23 (Nhỏ - Trung bình) |
| **WarehouseToHome**          | 2.04 × 10⁻⁹      | 0.18 (Nhỏ) |
| **OrderCount**               | 2.21 × 10⁻³      | 0.076 (Rất nhỏ) |
| **NumberOfAddress**          | 3.05 × 10⁻²      | 0.12 (Rất nhỏ) |
| **OrderAmountHikeFromLastYear** | 7.54 × 10⁻²  | 0.027 (Không đáng kể) |
| **CustomerID**               | 1.52 × 10⁻¹      | 0.051 (Không đáng kể) |
| **HourSpendOnApp**           | 2.09 × 10⁻¹      | 0.050 (Không đáng kể) |
| **CouponUsed**               | 4.49 × 10⁻¹      | 0.020 (Không đáng kể) |

### 🔍 Kết luận kiểm định biến định lượng và Churn  

### 📌 1️⃣ Biến có ảnh hưởng mạnh đến Churn (p-value rất nhỏ, effect size lớn > 0.4)  
✅ **Tenure** (p = 2.7 × 10⁻¹⁹⁴, effect size = 0.92) → Ảnh hưởng rất lớn đến Churn.  
✅ **Complain** (p = 1.3 × 10⁻⁷⁸, effect size = 0.67) → Khách hàng có khiếu nại có xu hướng rời bỏ cao.  
✅ **CashbackAmount** (p = 2.5 × 10⁻³⁸, effect size = 0.41) → Số tiền hoàn lại ảnh hưởng đến Churn.  
✅ **DaySinceLastOrder** (p = 2.8 × 10⁻³⁸, effect size = 0.40) → Khách hàng càng lâu không mua hàng càng dễ rời bỏ.  

### 📌 2️⃣ Biến có ảnh hưởng trung bình đến Churn (p-value nhỏ, effect size từ 0.2 - 0.4)  
🔹 **SatisfactionScore** (p = 3.75 × 10⁻¹⁵, effect size = 0.28) → Điểm hài lòng thấp làm tăng tỷ lệ Churn.  
🔹 **NumberOfDeviceRegistered** (p = 3.05 × 10⁻¹⁴, effect size = 0.29) → Khách hàng đăng ký nhiều thiết bị có tỷ lệ Churn thấp hơn.  
🔹 **CityTier** (p = 1.61 × 10⁻¹⁰, effect size = 0.22) → Tầng lớp thành phố ảnh hưởng đến Churn.  
🔹 **WarehouseToHome** (p = 2.04 × 10⁻⁹, effect size = 0.18) → Khoảng cách từ kho đến nhà có ảnh hưởng nhỏ đến Churn.  

### 📌 3️⃣ Biến có ảnh hưởng yếu đến Churn (p-value > 0.01, effect size < 0.2)  
🔹 **OrderCount** (p = 0.002, effect size = 0.075) → Số lượng đơn hàng có ảnh hưởng nhỏ đến Churn.  
🔹 **NumberOfAddress** (p = 0.03, effect size = 0.11) → Số địa chỉ được lưu có ảnh hưởng nhỏ.  
🔹 **OrderAmountHikeFromLastYear** (p = 0.075, effect size = 0.02) → Không có ý nghĩa thống kê mạnh.  

### 📌 4️⃣ Biến không có ảnh hưởng đáng kể đến Churn (p-value > 0.05)  
❌ **CustomerID** (p = 0.152) → Không có ý nghĩa trong dự đoán Churn.  
❌ **HourSpendOnApp** (p = 0.209) → Thời gian sử dụng app không ảnh hưởng nhiều đến Churn.  
❌ **CouponUsed** (p = 0.449) → Sử dụng coupon không có mối quan hệ đáng kể với Churn.  
---
### 📌 Kết luận chung về biến định lượng  
✅ **Các biến ảnh hưởng mạnh đến Churn:** `Tenure`, `Complain`, `CashbackAmount`, `DaySinceLastOrder`.  
✅ **Các biến có ảnh hưởng trung bình:** `SatisfactionScore`, `NumberOfDeviceRegistered`, `CityTier`, `WarehouseToHome`.  
✅ **Các biến có ảnh hưởng yếu hoặc không đáng kể:** `OrderCount`, `NumberOfAddress`, `OrderAmountHikeFromLastYear`, `HourSpendOnApp`, `CouponUsed`.  

### 🔍 Kết quả kiểm định thống kê biến định tính và Churn  

| **Biến**                     | **p-value**         | **Cramér's V** | **Mức độ ảnh hưởng** |
|------------------------------|--------------------|---------------|---------------------|
| **PreferedOrderCat**         | 2.77 × 10⁻⁶⁰      | 0.226         | Ảnh hưởng trung bình |
| **MaritalStatus**            | 1.07 × 10⁻⁴¹      | 0.183         | Ảnh hưởng nhỏ - trung bình |
| **PreferredLoginDevice**     | 1.08 × 10⁻¹⁶      | 0.114         | Ảnh hưởng nhỏ |
| **PreferredPaymentMode**     | 9.71 × 10⁻¹⁵      | 0.118         | Ảnh hưởng nhỏ |
| **Gender**                   | 3.08 × 10⁻²       | 0.029         | Không đáng kể |
---
### 📌 Kết luận:
- **Biến có ảnh hưởng trung bình đến Churn:** `PreferedOrderCat`, `MaritalStatus`.  
- **Biến có ảnh hưởng nhỏ đến Churn:** `PreferredLoginDevice`, `PreferredPaymentMode`.  
- **Biến không có ảnh hưởng đáng kể:** `Gender`.  
![image](https://github.com/user-attachments/assets/e7c2eb4e-f8f2-4d3e-bd5b-4951acbf8f32)
# Thống kê mô tả Violin Plot (Tenure vs Churn)

## Churn = 0 (Không rời bỏ):
- Tenure trải rộng từ 0 đến hơn 60 tháng.
- Phần lớn khách hàng có Tenure dưới 30 tháng.
- Phân bố rộng và có nhiều giá trị cao.

## Churn = 1 (Rời bỏ):
- Tenure chủ yếu tập trung dưới 10 tháng.
- Ít khách hàng có Tenure cao.
- Phân bố hẹp, lệch về phía giá trị thấp.  


# 📊 Ma trận Churn vs Complain  

|                     | Không khiếu nại (0) | Có khiếu nại (1) |
|---------------------|-----------------|-----------------|
| **Không rời bỏ (0)** | 3586            | 1096            |
| **Rời bỏ (1)**       | 440             | 508             |

### 📌 Nhận xét  
- **Phần lớn khách hàng không rời bỏ (Churn = 0)** thuộc nhóm **không khiếu nại (3586 người)**.  
- **Khách hàng có khiếu nại (Complain = 1)** có tỷ lệ rời bỏ cao hơn:  
  - **508 khách hàng** rời bỏ sau khi khiếu nại.  
  - So với chỉ **440 khách hàng** rời bỏ mà không khiếu nại.  
- **Tóm lại**, khiếu nại có thể là một dấu hiệu báo trước về churn, nhưng không phải tất cả khách hàng khiếu nại đều rời bỏ.

![image](https://github.com/user-attachments/assets/bc593a14-f9bc-44dd-b09b-880498acb57f)
![image](https://github.com/user-attachments/assets/d4be2b44-b30d-4472-b716-563a2dbf575f)

# Phân tích Boxplot: CashbackAmount vs Churn

## 1. Tổng quan
Biểu đồ hộp (boxplot) dưới đây thể hiện phân phối của **CashbackAmount** theo trạng thái **Churn** (rời bỏ hoặc không rời bỏ).

![Boxplot CashbackAmount vs Churn](image.png)

## 2. Nhận xét
- **Churn = 0 (Không rời bỏ)**  
  - Khoảng phân vị 50% (IQR) của CashbackAmount nằm trong khoảng **~120 đến ~200**.  
  - Có một số giá trị ngoại lệ rất cao, lên đến **hơn 300**.  
  - Trung vị (median) cao hơn so với nhóm Churn = 1.  

- **Churn = 1 (Rời bỏ)**  
  - Khoảng IQR thấp hơn, nằm trong khoảng **~100 đến ~160**.  
  - Trung vị của nhóm này thấp hơn nhóm không rời bỏ.  
  - Có nhiều giá trị ngoại lệ nhưng thấp hơn so với nhóm Churn = 0.  

## 3. Kết luận
- Nhìn chung, khách hàng không rời bỏ (**Churn = 0**) có xu hướng nhận **CashbackAmount cao hơn**.  
- Nhóm khách hàng rời bỏ (**Churn = 1**) nhận cashback thấp hơn, nhưng vẫn có một số trường hợp ngoại lệ.  
- Điều này có thể gợi ý rằng **việc hoàn tiền nhiều có thể giúp giữ chân khách hàng**, nhưng cần thêm phân tích để xác định tác động thực sự của cashback đến churn.  

![image](https://github.com/user-attachments/assets/398e2e74-af7c-45ac-8932-0d542c35b7a9)

## 🔎 Phân tích Violin Plot: DaySinceLastOrder vs Churn

### 1️⃣ Nhóm Không Rời Bỏ (Churn = 0)
- Phần lớn khách hàng có `DaySinceLastOrder` thấp (gần 0), nghĩa là họ thường xuyên đặt hàng.
- Một số ít khách hàng có khoảng thời gian dài giữa các lần đặt hàng, nhưng họ vẫn tiếp tục mua hàng.

### 2️⃣ Nhóm Rời Bỏ (Churn = 1)
Có hai nhóm khách hàng rõ rệt:

#### 🟢 Nhóm có `DaySinceLastOrder` thấp:
- Họ vừa đặt hàng gần đây nhưng vẫn rời bỏ. Điều này có thể do:
  - Trải nghiệm kém với lần mua cuối cùng.
  - Khuyến mãi hoặc ưu đãi không còn hấp dẫn.

#### 🔴 Nhóm có `DaySinceLastOrder` cao:
- Họ đã lâu không đặt hàng trước khi rời bỏ.
- Điều này thường gặp ở khách hàng mất hứng thú hoặc chuyển sang dịch vụ khác.

![image](https://github.com/user-attachments/assets/58f61245-99bf-40bc-90cc-7612f9469824)

## 📊 Biểu đồ Thanh Phân Kỳ (Diverging Bar Chart) cho Các Biến Định Tính

### 🔹 Mô tả:
- **Biểu đồ thanh phân kỳ** giúp trực quan hóa mối quan hệ giữa các biến định tính và biến mục tiêu `Churn`.
- Các thanh màu **xanh lá cây** đại diện cho khách hàng **không rời bỏ** (`Churn = 0`).
- Các thanh màu **đỏ** đại diện cho khách hàng **rời bỏ** (`Churn = 1`).
- Biểu đồ giúp xác định nhóm khách hàng nào có tỷ lệ rời bỏ cao hơn dựa trên từng đặc điểm cụ thể.

### 🔹 Code:
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Function to plot diverging bar chart
def plot_diverging_bar_chart(df, categorical_cols, target):
    for col in categorical_cols:
        # Create a cross-tabulation of counts normalized by columns to get percentages
        cross_tab = pd.crosstab(df[col], df[target], normalize='columns') * 100

        # Ensure both churn values (0 and 1) are present in the columns
        if 0 not in cross_tab.columns:
            cross_tab[0] = 0
        if 1 not in cross_tab.columns:
            cross_tab[1] = 0

        cross_tab = cross_tab[[0, 1]]  # Ensure the order of columns is [0, 1]

        # Plotting
        fig, ax = plt.subplots(figsize=(10, 6))

        # Plot bars for churn = 0
        bars0 = ax.barh(cross_tab.index, cross_tab[0], color='green', alpha=0.6, label='Churn = 0')

        # Plot bars for churn = 1
        bars1 = ax.barh(cross_tab.index, -cross_tab[1], color='red', alpha=0.6, label='Churn = 1')

        ax.set_title(f'Diverging Bar Chart for {col}')
        ax.set_xlabel('Percentage')
        ax.axvline(0, color='grey', linewidth=0.8)
        ax.legend()
        plt.show()

# Plot the charts
plot_diverging_bar_chart(df, categorical_cols, 'Churn')

![image](https://github.com/user-attachments/assets/885b4963-7361-44d4-bbe0-ca1ee88ab4b7)  
![image](https://github.com/user-attachments/assets/e551dbb8-a41e-4576-9408-0a887f139a31)  
![image](https://github.com/user-attachments/assets/42805c46-d446-4139-a408-ca54cf0d3856)  
![image](https://github.com/user-attachments/assets/19261fc2-d897-4c9c-9301-ae482d14c870)  
![image](https://github.com/user-attachments/assets/3c57f2c8-6b8a-4591-a9fe-54c21122d597)  



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

