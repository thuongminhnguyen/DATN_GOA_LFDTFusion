# DATN_GOA_LFDTFusion

Chi tiết cài đặt

git clone hoặc extract foler code LFDT-Fusion and GOA.zip
cd LFDT-Fusion

conda create -n LFDT-Fusion python=3.9
conda activate LFDT-Fusion

pip install -r requiremt.txt

# Push dataset vào thư mục dataset

# Tiền xử lí dữ liệu
cd code01/code01/preprocessMRI
# Sửa đường dẫn tới dữ liệu phù hợp
python conver01.py

python clahefolder.py

python laplaceFolder.py

# Phân rã hình ảnh
# Sưa đường dẫn tới dataset của bạn
python biteralMRIFolder.py
python biteralYUVFolder.py

#Lưu lại max min pixcel ảnh trước khi chuẩn hóa
cd code01/code01/util
python saveJsonMinmax.py

# Chuẩn hóa ảnh thành phần chi tiết về miền [0 1] trước khi training
Sửa đường dẫn tới dữ liệu
cd code01/code01/std
python standardization.py

# Tiến hành training LFDT-Fusion Model
# Đọc file readme đã hướng dẫn train và test model
# Push data đã chuẩn hóa của thành phần chi tiết vào thư mục train tong thư mục dataset

# Sửa file train.json trong thư mục config tùy thuộc vào cấu hình máy và các thư mục data
# Tiến hành training

python python train.py --sample_selected ddp-solver++ --model_selected DFT --batch_size 4 --fusion_task PET-MRI --strategy MAX

#Weight sẽ được lưu ở experiments
#Sau khi training xong tiến hành Inferent
# Sửa file test_med-PET.json trong thư mục config với đường dẫn data, weight và các tham số phù hợp
python test_med-PET.py

# Tiến hành chạy thuật toán tối ưu để tổng hợp ảnh
# Sửa các đường dẫn dữ liệu phù hợp
cd code01/code01/metaherictic
python GOA.py

# Tiến hành khôi phục màu
cd code01/code01/Fusion
python khoiphucmau.py


#Một vài lưu ý khác
