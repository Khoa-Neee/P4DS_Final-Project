# FinalProject-P4DS

Đồ án cuối kỳ môn học Lập trình cho Khoa học Dữ liệu.

## Các thành viên của nhóm

- 23122035: Châu Văn Minh Khoa
- 23122046: Phan Ngọc Quân

## Nội dung ngắn gọn

Phân tích và khám phá bộ dữ liệu Student Depression Dataset (Kaggle), chuẩn hóa dữ liệu, trực quan hóa và chuẩn bị cho mô hình dự báo biến mục tiêu `Depression`.

Tệp chính: `main.ipynb`

Thư mục dữ liệu: `data/Student Depression Dataset.csv`

## How to run

1) Clone repository

```pwsh
git clone <repo-url>
```

2) Tạo môi trường `min_ds-env` (nếu chưa có) từ file `min_ds-env.yml`

```pwsh
cd "P4DS_Final Project"
conda env create -f min_ds-env.yml -n min_ds-env
# Nếu môi trường đã tồn tại và muốn cập nhật gói:
# conda env update -f min_ds-env.yml -n min_ds-env
```

3) Mở terminal tại thư mục vừa clone về (đã ở trong thư mục dự án)

4) Kích hoạt môi trường

```pwsh
conda activate min_ds-env
```

5) Mở Jupyter Notebook

```pwsh
jupyter notebook
```

6) Trong giao diện Jupyter, mở `main.ipynb` và chọn Run All để thực thi toàn bộ các ô (cells).

Ghi chú:
- Nếu dùng VS Code, có thể mở trực tiếp `main.ipynb` và chọn Run All.
- Đảm bảo file dữ liệu tồn tại tại `data/Student Depression Dataset.csv` trước khi chạy.
