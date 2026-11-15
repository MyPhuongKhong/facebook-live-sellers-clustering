# 📊 Phân cụm Hành vi Tương tác của Nhà Bán Hàng Facebook Live tại Thái Lan

Dự án phân tích và phân cụm hành vi tương tác bài đăng của các nhà bán hàng thời trang, mỹ phẩm trên Facebook tại Thái Lan, dựa trên bộ dữ liệu **7.050 bài đăng** từ giai đoạn 2012–2018, sử dụng thuật toán học không giám sát **K-Means Clustering**.

---

## 🗂️ Giới Thiệu Bộ Dữ Liệu

Bộ dữ liệu [**Facebook Live Sellers in Thailand**](https://archive.ics.uci.edu/dataset/488/facebook+live+sellers+in+thailand) từ **UCI Machine Learning Repository** (Dataset ID: 488), được giới thiệu trong bài báo của tác giả **Nassim Dehouche** (2020).

| Thuộc tính | Giá trị |
| :--- | :--- |
| **Số lượng bài đăng** | 7.050 |
| **Số đặc trưng** | 12 cột |
| **Thời gian thu thập** | Tháng 3/2012 – Tháng 6/2018 |
| **Đối tượng** | 10 nhà bán hàng thời trang & mỹ phẩm tại Thái Lan |
| **Loại bài đăng** | `photo`, `video` (video quay sẵn & livestream), `link`, `status` |

**Hai cột mốc quan trọng trong dữ liệu:**
- 📅 **24/02/2016**: Facebook ra mắt các biểu tượng cảm xúc mới (`Love`, `Haha`, `Wow`, `Sad`, `Angry`) cho toàn cầu.
- 📅 **06/04/2016**: Facebook Live (Livestream) ra mắt chính thức trên toàn cầu, bao gồm Thái Lan. Đây là cột mốc then chốt lý giải sự xuất hiện của **Cluster 1 (Viral)** trong kết quả phân cụm.

---

## 🎯 Mục Tiêu Dự Án

| # | Mục tiêu | Mô tả |
| :---: | :--- | :--- |
| 1 | **Kiểm tra và Làm sạch Dữ liệu** | Phát hiện 54 bản ghi trùng lặp ẩn (khi bỏ cột `status_id`) và sửa 9 bản ghi `num_reactions` không khớp với tổng các cảm xúc con. |
| 2 | **Phân tích EDA** | Đánh giá phân bố loại bài đăng, khung giờ tương tác cao nhất, và ma trận tương quan giữa các chỉ số. |
| 3 | **Phân cụm K-Means** | Chuẩn hóa đặc trưng bằng `StandardScaler`, xác định số cụm tối ưu `k=3` qua **Elbow Method**, **Silhouette Score** (Silhouette Score = 0.7969) và mục tiêu phân nhóm bài đăng theo mức độ hiệu quả. |
| 4 | **Phân tích Chân dung Cụm & Đề xuất Hành động** | Trực quan hóa bằng Scatter Plot 2D (`num_reactions` vs `num_likes` và `num_comments` vs `num_shares`) và Heatmap chuẩn hóa Min-Max để xây dựng chân dung từng cụm, đề xuất chiến lược nội dung. |

---

## 🛠️ Công Nghệ Sử Dụng

| Thành phần | Công cụ / Thư viện |
| :--- | :--- |
| **Ngôn ngữ** | Python 3.10 |
| **Xử lý dữ liệu** | `pandas`, `numpy` |
| **Trực quan hóa** | `matplotlib`, `seaborn` |
| **Machine Learning** | `scikit-learn` — `StandardScaler`, `KMeans`, `silhouette_score` |
| **Nguồn dữ liệu** | `ucimlrepo` (UCI Dataset ID: 488) |

---

## 🔍 Quy Trình Thực Hiện

```
A. Thiết lập Môi trường & Thu nạp Dữ liệu
   └── Tải dữ liệu trực tiếp từ UCI qua ucimlrepo (7050 dòng, 12 cột)

B. Kiểm tra & Làm sạch Dữ liệu
   ├── B.1 Kiểm tra kiểu dữ liệu (dtypes)
   ├── B.2 Kiểm tra giá trị Null / Missing
   ├── B.3 Phát hiện & loại bỏ 54 dòng trùng lặp ẩn → còn 6.996 dòng
   ├── B.4 Kiểm tra tính nhất quán: num_reactions == sum(cảm xúc con) → sửa 9 bản ghi sai
   └── B.5 Trích xuất thuộc tính thời gian: year, month, day, hour, day_of_week

C. Phân tích Khám phá Dữ liệu (EDA)
   ├── C.1 Phân bố loại bài đăng (Photo / Video / Link / Status)
   ├── C.2 Phân tích khung giờ & ngày đăng bài có tương tác cao
   └── C.3 Ma trận tương quan giữa các chỉ số tương tác

D. Phân cụm K-Means
   ├── Lựa chọn 3 đặc trưng: num_reactions, num_comments, num_shares
   ├── Chuẩn hóa: StandardScaler
   ├── Elbow Method (WCSS) & Silhouette Score (k = 2..8) → chọn k = 3
   └── Huấn luyện mô hình K-Means với k = 3

E. Phân tích Chân dung Cụm
   ├── Scatter Plot 2D: num_comments × num_shares và num_reactions × num_likes theo màu cụm
   └── Heatmap chuẩn hóa Min-Max: chân dung cụm thông qua qua giá trị trung bình của các chỉ số tương tác tương tác và cảm xúc con

F. Kết luận & Đề xuất Hành động
   └── Xây dựng Chân dung Cụm và Đề xuất Hành động
```

---

## 📈 Kết Quả Phân Cụm (Key Findings)

Mô hình phân tách **6.996 bài đăng** thành **3 nhóm đặc thù**:

| Cụm | Tên nhóm | Đặc trưng Tương tác | Định dạng Chủ đạo | Đề xuất Hành động |
| :---: | :--- | :--- | :---: | :--- |
| **0** | Standard / Low Engagement | Tương tác ở mức cơ bản, ít comment và share | `Photo` | Nhóm bài duy trì nhận diện thương hiệu. Nên chèn **Call-to-Action (CTA)** để kích thích bình luận. |
| **1** | Viral / High Engagement | Comments & shares vượt trội (trung bình **3282,78** lượt comments và **507,37** lượt shares). Chỉ xuất hiện sau **06/04/2016** (ngày Facebook Live ra mắt) | `Video / Live` | **Nhóm nội dung chủ lực tạo chuyển đổi.** Tăng tần suất livestream & xây dựng kịch bản bán hàng tương tác. |
| **2** | Medium Engagement | Likes & reactions cao, nhưng comments/shares ở mức trung bình | `Photo & Video` | Phù hợp cho chiến dịch ra mắt sản phẩm, minigame hoặc thông báo khuyến mãi lớn. |

> **Phát hiện quan trọng**: **Cluster 1 (Viral)** tập trung ở giai đoạn trùng thời điểm Facebook ra mắt Live vào 06/04/2016. Điều này có thể cho thấy tính năng **livestream** đã tạo ra bước nhảy vọt về lượt tương tác nhờ sự kết hợp giữa yếu tố giải trí, giao lưu xã hội và bán hàng thời gian thực.

---

## 🚀 Hướng Dẫn Cài Đặt & Chạy

### 1. Clone repository
```bash
git clone https://github.com/<your-username>/facebook-live-sellers-clustering.git
cd facebook-live-sellers-clustering
```

### 2. Cài đặt thư viện
```bash
pip install -r requirements.txt
```

### 3. Chạy Notebook
```bash
jupyter notebook notebooks/clustering.ipynb
```

---

## 📁 Cấu Trúc Thư Mục

```
facebook-live-sellers-clustering/
├── README.md                     # Tài liệu tổng quan dự án
├── requirements.txt              # Danh sách thư viện cần thiết
├── .gitignore
└── notebooks/
    └── clustering.ipynb          # Notebook phân tích & mô hình hóa chính
```