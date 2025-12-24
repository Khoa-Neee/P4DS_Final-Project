# Phân tích tỉ lệ rời bỏ của khách hàng trong BankChurners Dataset

## 
- Môn học: CSC17104 – Programming for Data Science
- Trường Đại học Khoa học Tự Nhiên
- Năm học: 2025-2026

## Các thành viên của nhóm

- 23122035: Châu Văn Minh Khoa
- 23122046: Phan Ngọc Quân

## Project Overview

Dự án phân tích tỉ lệ rời bỏ (churn) của khách hàng thẻ tín dụng trong bộ dữ liệu **BankChurners**. Mục tiêu là khám phá các yếu tố ảnh hưởng đến quyết định rời bỏ của khách hàng, xây dựng mô hình dự báo và đưa ra các khuyến nghị chiến lược giữ chân khách hàng cho ngân hàng.

### Objectives
- Khám phá và phân tích đặc điểm dữ liệu khách hàng ngân hàng
- Xác định các yếu tố chính ảnh hưởng đến tỷ lệ churn
- Xây dựng mô hình dự báo khả năng khách hàng rời bỏ
- Đề xuất giải pháp thực tiễn để giảm tỷ lệ churn

## Data Information

### Dataset Description
- **Nguồn**: Kaggle - Credit Card Customers
- **URL**: https://www.kaggle.com/datasets/sakshigoyal7/credit-card-customers
- **License**: CC0-Public Domain
- **Số lượng**: 10,127 dòng × 23 cột (sau xử lý: 10,127 × 20 cột)
- **Kích thước**: ~0.87 MB

### Key Features
- **Target Variable**: `Attrition_Flag` (Attrited Customer / Existing Customer)
- **Nhân khẩu học**: Customer_Age, Gender, Dependent_count, Education_Level, Marital_Status, Income_Category
- **Thông tin tài khoản**: Card_Category, Months_on_book, Total_Relationship_Count
- **Hành vi tín dụng**: Credit_Limit, Total_Revolving_Bal, Avg_Open_To_Buy, Avg_Utilization_Ratio
- **Hành vi giao dịch**: Total_Trans_Amt, Total_Trans_Ct, Total_Amt_Chng_Q4_Q1, Total_Ct_Chng_Q4_Q1
- **Tương tác dịch vụ**: Months_Inactive_12_mon, Contacts_Count_12_mon

### Data Quality
- **Missing Values**: Không có giá trị thiếu
- **Duplicates**: Không có bản ghi trùng lặp
- **Outliers**: Được xử lý bằng phương pháp IQR cho các biến liên tục
- **Class Imbalance**: ~16% Attrited Customer vs ~84% Existing Customer

## Research Questions

Dự án tập trung vào 5 câu hỏi nghiên cứu chính:

1. **Mức độ sử dụng thẻ và Churn**: Có phải nhóm khách hàng rời đi có xu hướng phân bố ở hai thái cực (sử dụng quá ít hoặc quá mức)?

2. **Thu nhập, Học vấn và Dư nợ**: Mức thu nhập và trình độ học vấn có tác động đến dư nợ quay vòng không? Có tồn tại nhóm ngoại lệ nào?

3. **Tuổi, Thời gian sử dụng và Churn**: Có mối tương quan nào giữa tuổi khách hàng, thời gian sử dụng thẻ và tỷ lệ churn?

4. **Hạn mức cao - Sử dụng thấp**: Liệu khách hàng có hạn mức cao nhưng sử dụng thấp có phải là nhóm nguy cơ churn cao nhất?

5. **Hành vi giao dịch**: Các chỉ số tương tác và giao dịch trong 12 tháng qua có mối quan hệ thế nào với quyết định rời bỏ?

## Key Findings

### 1. Mức độ sử dụng thẻ (Avg_Utilization_Ratio)
- **Mối quan hệ chữ U**: Churn tập trung ở hai thái cực
  - Low Use (0-0.1): **26.5%** churn rate - cao nhất
  - Optimal Use (0.1-0.7): **8.38%** churn rate - thấp nhất
  - High Use (0.7-1.0): **12.89%** churn rate
- **Insight**: Thiếu giá trị sử dụng là động lực rời đi mạnh hơn căng thẳng tài chính

### 2. Thu nhập và Học vấn
- **Thu nhập**: Ảnh hưởng RẤT MẠNH đến dư nợ quay vòng (p < 0.05)
  - Thu nhập thấp → Utilization cao (~30-35%)
  - Thu nhập cao → Utilization thấp (~10-15%)
- **Học vấn**: KHÔNG có ảnh hưởng ý nghĩa thống kê (p > 0.05)
- **Kết luận**: Trình độ học vấn không phải yếu tố dự báo tốt cho rủi ro tín dụng

### 3. Tuổi và Thời gian sử dụng
- Hệ số tương quan với churn cực kỳ yếu (r < 0.02)
- Nhóm 40-59 tuổi có tỷ lệ churn cao nhất (~17%)
- Các yếu tố nhân khẩu học KHÔNG phải chỉ báo quan trọng để dự báo churn

### 4. Hạn mức cao - Sử dụng thấp
- Nhóm này KHÔNG phải nguồn churn chính
- Cần tập trung vào nhóm hạn mức thấp và giảm giao dịch

### 5. Hành vi giao dịch - Yếu tố quan trọng nhất
- **Total_Trans_Ct** (số lần giao dịch): Tương quan mạnh nhất với churn (r = -0.37)
- Mô hình dự báo:
  - Random Forest: **ROC AUC = 0.95**
  - Logistic Regression: **ROC AUC = 0.86**
- **Insight bất ngờ**: Tần suất giao dịch quan trọng hơn giá trị giao dịch

## Method Overview

### 1. Data Exploration (EDA)
- Phân tích cấu trúc dữ liệu và kiểu dữ liệu
- Thống kê mô tả cho biến số và biến phân loại
- Phân tích phân phối và outliers
- Ma trận tương quan giữa các biến

### 2. Data Preprocessing
- Loại bỏ cột không cần thiết (CLIENTNUM, Naive_Bayes_Classifier)
- Xử lý giá trị 'Unknown' trong biến phân loại
- Phát hiện và xử lý outliers bằng phương pháp IQR
- Chuyển đổi biến mục tiêu về dạng nhị phân (0/1)
- Encoding cho biến phân loại

### 3. Statistical Analysis
- **Two-Way ANOVA**: Kiểm định ảnh hưởng của Thu nhập × Học vấn
- **Correlation Analysis**: Phân tích mối liên hệ giữa các biến
- **Chi-square Test**: Kiểm định độc lập cho biến phân loại
- Phân tích theo phân khúc (Segmentation Analysis)

### 4. Machine Learning Models
- **Logistic Regression**: Baseline model, dễ giải thích
- **Random Forest**: Ensemble learning, xử lý tốt non-linear relationships
- **Evaluation Metrics**:
  - ROC-AUC Score
  - Confusion Matrix
  - Precision, Recall, F1-Score
  - Feature Importance Analysis

### 5. Visualization Techniques
- Distribution plots (histograms, boxplots)
- Heatmaps (correlation, interaction effects)
- Bar charts (churn rates by segments)
- ROC curves (model performance)
- Feature importance plots

## Project Structure

```
P4DS_Final-Project/
│
├── README.md                    # Tài liệu dự án
├── min_ds-env.yml              # Môi trường conda
│
├── data/
│   └── BankChurners.csv        # Dữ liệu gốc (10,127 × 23)
│
├── EDA.ipynb                   # Khám phá dữ liệu
│   ├── I. Data Collection
│   ├── II. Data Exploration
│   │   ├── Basic Information
│   │   ├── Data Types & Structure
│   │   ├── Missing Values
│   │   ├── Duplicates
│   │   └── Outliers
│   ├── III. Descriptive Statistics
│   │   ├── Numerical Variables
│   │   ├── Categorical Variables
│   │   └── Correlation Analysis
│   └── IV. Visualization
│
└── Questions.ipynb             # Phân tích câu hỏi
    ├── I. Data Loading
    ├── II. Preprocessing
    ├── III. Question Formulation
    ├── IV. Data Analysis (5 questions)
    │   ├── Q1: Utilization & Churn
    │   ├── Q2: Income, Education & Debt
    │   ├── Q3: Age, Tenure & Churn
    │   ├── Q4: High Limit - Low Usage
    │   └── Q5: Transaction Behavior
    ├── V. Machine Learning Models
    └── VI. Conclusions & Reflections
```

## Technical Requirements

### Software & Libraries
```yaml
# Python Version
python: 3.10+

# Core Libraries
- pandas >= 1.5.0
- numpy >= 1.23.0

# Visualization
- matplotlib >= 3.6.0
- seaborn >= 0.12.0

# Machine Learning
- scikit-learn >= 1.1.0
- scipy >= 1.9.0

# Jupyter
- jupyter >= 1.0.0
- notebook >= 6.5.0
```

### Environment Setup
```bash
# Tạo môi trường từ file yml
conda env create -f min_ds-env.yml -n min_ds-env

# Kích hoạt môi trường
conda activate min_ds-env
```

## How to Run

### Option 1: Jupyter Notebook (Command Line)

1) **Clone repository**

```bash
git clone https://github.com/Khoa-Neee/P4DS_Final-Project.git
cd P4DS_Final-Project
```

2) **Tạo môi trường** (nếu chưa có)

```bash
conda env create -f min_ds-env.yml -n min_ds-env
```

3) **Kích hoạt môi trường**

```bash
conda activate min_ds-env
```

4) **Khởi động Jupyter Notebook**

```bash
jupyter notebook
```

5) **Chạy notebooks**
   - Mở [EDA.ipynb](EDA.ipynb) → Run All để khám phá dữ liệu
   - Mở [Questions.ipynb](Questions.ipynb) → Run All để phân tích câu hỏi

### Option 2: VS Code

1) Mở thư mục dự án trong VS Code
2) Chọn kernel `min_ds-env`
3) Mở notebook và chọn **Run All**

### Notes
- Đảm bảo file [data/BankChurners.csv](data/BankChurners.csv) tồn tại trước khi chạy
- Notebooks có thể chạy độc lập nhau
- Khuyến nghị chạy EDA.ipynb trước để hiểu dữ liệu

## Limitations and Constraints

### Data Limitations
- **Snapshot data**: Dữ liệu chỉ ghi nhận 12 tháng, không theo dõi được chuỗi sự kiện dẫn tới churn
- **Missing contextual variables**: 
  - Thiếu thông tin về kênh tương tác (online/offline/mobile)
  - Không có dữ liệu về chất lượng dịch vụ khách hàng
  - Thiếu thông tin chi tiết về lịch sử ưu đãi/khuyến mãi
- **Class imbalance**: Tỷ lệ churn thấp (16%) có thể ảnh hưởng đến hiệu suất mô hình

### Analysis Constraints
- **Correlation ≠ Causation**: Các mối liên hệ được tìm thấy chưa chứng minh quan hệ nhân quả
- **Model interpretability**: Random Forest cho kết quả tốt nhưng khó giải thích cho business users
- **No A/B testing**: Chưa có thử nghiệm thực tế để xác nhận hiệu quả các đề xuất
- **Temporal dynamics**: Không phân tích được xu hướng thay đổi theo thời gian

### Technical Constraints
- **Feature engineering**: Chưa tạo ra các biến tương tác phức tạp
- **Hyperparameter tuning**: Chưa tối ưu hóa kỹ lưỡng các tham số mô hình
- **Cross-validation**: Chưa áp dụng k-fold cross-validation cho đánh giá robust
- **Ensemble methods**: Chưa thử nghiệm các phương pháp kết hợp mô hình nâng cao

## Future Directions

### 1. Expanded Research Questions
- **Temporal analysis**: Phân tích churn theo chuỗi thời gian, dự báo điểm churn trong vòng đời khách hàng
- **Campaign effectiveness**: Đánh giá tác động của các chiến dịch CSKH và ưu đãi
- **Reactivation strategies**: Nghiên cứu khả năng tái kích hoạt khách hàng đã churn
- **Segment-specific models**: Xây dựng mô hình riêng cho từng phân khúc khách hàng

### 2. Advanced Analytics
- **Early warning system**: Mô hình cảnh báo sớm dựa trên ngưỡng hành vi
- **Customer lifetime value**: Tích hợp CLV vào mô hình churn
- **Multi-channel behavior**: Phân tích hành vi cross-channel (online/offline/mobile)
- **Network analysis**: Xem xét ảnh hưởng mạng xã hội trong quyết định churn

### 3. Alternative Methods
- **Gradient Boosting (XGBoost, LightGBM)**: So sánh với Random Forest
- **Deep Learning**: Neural Networks cho pattern recognition phức tạp
- **Survival Analysis**: Cox regression để mô hình hóa thời gian đến churn
- **Causal inference**: Propensity score matching để ước lượng tác động nhân quả

### 4. Data Enhancement
- **Customer service logs**: Lịch sử liên hệ, khiếu nại, phản hồi
- **Promotion history**: Thông tin chi tiết về ưu đãi đã nhận
- **Product portfolio**: Các sản phẩm ngân hàng khác khách hàng đang sử dụng
- **External data**: Dữ liệu kinh tế vĩ mô, competitors' offerings
- **Behavioral tracking**: Click-stream, app usage patterns

### 5. Deployment & Monitoring
- **CRM Integration**: Tích hợp mô hình vào hệ thống CRM
- **Real-time scoring**: API để tính điểm churn real-time
- **Automated triggers**: Kích hoạt tự động chiến dịch giữ chân
- **A/B testing framework**: Thử nghiệm can thiệp và đo lường hiệu quả
- **KPI dashboard**: Dashboard theo dõi churn rate, retention rate, campaign ROI
- **Model retraining**: Pipeline tự động cập nhật mô hình định kỳ

## Conclusions

Dự án đã thành công trong việc phân tích và dự báo hành vi churn của khách hàng thẻ tín dụng, với những phát hiện quan trọng:

### Main Achievements
1. **Identified key drivers**: Hành vi giao dịch (đặc biệt là tần suất) là yếu tố dự báo mạnh nhất
2. **Built effective models**: Random Forest đạt ROC-AUC 0.95, sẵn sàng triển khai
3. **Debunked assumptions**: Học vấn không ảnh hưởng đến rủi ro tín dụng
4. **Actionable insights**: Đề xuất cụ thể cho từng phân khúc khách hàng

### Business Impact
- **Targeted retention**: Tập trung vào nhóm Low Use thay vì High Use
- **Resource optimization**: Không cần phân tích sâu về học vấn
- **Behavior-driven**: Chuyển hướng từ demographic sang behavioral analytics
- **Predictive capability**: Có thể cảnh báo sớm và can thiệp kịp thời

### Academic Value
- Minh họa quy trình Data Science hoàn chỉnh từ EDA đến deployment
- Kết hợp nhiều phương pháp: thống kê, machine learning, visualization
- Nhấn mạnh tầm quan trọng của việc đặt câu hỏi đúng
- Cân bằng giữa độ chính xác mô hình và khả năng giải thích

## Acknowledgments

### Course Information
- **Môn học**: CSC17104 - Lập trình cho Khoa học Dữ liệu
- **Đơn vị**: Khoa Công nghệ Thông tin, Đại học Khoa học Tự nhiên, ĐHQG-HCM
- **Học kỳ**: HKI, Năm học 2024-2025

### Data Source
- **Kaggle**: Credit Card Customers dataset
- **License**: CC0-Public Domain
- **URL**: https://www.kaggle.com/datasets/sakshigoyal7/credit-card-customers

### Tools & Libraries
Cảm ơn cộng đồng open-source đã phát triển các công cụ:
- Python Software Foundation
- Pandas, NumPy teams
- Scikit-learn developers
- Matplotlib & Seaborn communities
- Jupyter Project

### Team Reflections

#### Châu Văn Minh Khoa - 23122035
Dự án giúp em hiểu sâu hơn về quy trình Data Science thực tế. Điều quan trọng nhất là học được cách đặt câu hỏi đúng và biết rằng tiền xử lý dữ liệu kỹ lưỡng quan trọng hơn việc chọn thuật toán phức tạp. Phát hiện về tầm quan trọng của tần suất giao dịch (Total_Trans_Ct) đã thay đổi cách nhìn của em về customer loyalty.

#### Phan Ngọc Quân - 23122046
Qua dự án, em nhận ra Data Science không chỉ là về code và mô hình, mà là về việc hiểu câu chuyện đằng sau dữ liệu. Việc phát hiện ra học vấn không ảnh hưởng đến rủi ro tín dụng là bài học quý giá về việc không nên giả định mà cần để dữ liệu dẫn dắt. Dự án này đã giúp em phát triển tư duy data-driven thinking.

---

**Last Updated**: December 2024
