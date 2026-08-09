# Phát hiện làn đường (Lane Detection)

---

## Giới thiệu

Dự án xử lý ảnh và thị giác máy tính (Computer Vision) sử dụng **Python** và **OpenCV** nhằm phát hiện làn đường tự động trên video giao thông, tính toán bán kính độ cong của đường và khoảng cách lệch tâm của xe.

---

## Các bước tiến hành

1. **Xử lý dữ liệu đầu vào:** Đọc và phân tách video thành từng khung hình (`frame`).
2. **Hiệu chỉnh Camera (Camera Calibration):** Sử dụng các ảnh chụp bàn cờ (chessboard) từ thư mục dữ liệu để loại bỏ hiện tượng méo hình do thấu kính camera.
3. **Biến đổi phối cảnh (Perspective Transform):** Chuyển đổi góc nhìn từ camera trước thành góc nhìn từ trên xuống (`Bird's Eye View`).
4. **Xử lý ngưỡng nhị phân (Binary Thresholding):** Kết hợp các phương pháp lọc màu (HLS - Saturation, Hue), ngưỡng cường độ sáng (White pixels) và toán tử đạo hàm Sobel (Sobel X) để làm nổi bật vạch kẻ đường.
5. **Tìm kiếm & Fit Đa thức (Lane Finding & Polynomial Fitting):**
* Sử dụng biểu đồ tần số (Histogram) kết hợp **Sliding Windows** để phát hiện vị trí vạch kẻ làn đường ban đầu.
* Dùng phương pháp xấp xỉ đa thức bậc hai (`np.polyfit`) để vẽ đường cong bám sát làn đường.
* Tối ưu hóa hiệu suất bằng cách sử dụng thông tin từ các khung hình trước đó cho video liên tục.


6. **Tính toán thông số xe:** Đo bán kính độ cong của mặt đường (Radius of Curvature) và tính độ lệch vị trí tâm xe so với tâm làn đường.
7. **Trình chiếu kết quả (Project Back):** Đổ màu vùng làn đường đã nhận diện, đưa trở lại phối cảnh gốc và hiển thị trực tiếp các thông số lên video xuất ra.

---

## Ngôn ngữ & Thư Viện Sử Dụng

* **Python**
* **OpenCV (`cv2`):** Xử lý ảnh, biến đổi hình học, cấu hình camera.
* **NumPy:** Xử lý ma trận, tính toán tọa độ và mảng pixel.
* **Matplotlib:** Trực quan hóa hình ảnh trong quá trình kiểm thử.
* **MoviePy:** Đọc, xử lý từng khung hình và xuất file video kết quả.

---

## Cấu Trúc Dự Án

```text
📦 project_root
 ┣ 📂 code
 ┃  ┗ 📜 Lane_detection.ipynb      # Notebook chứa toàn bộ mã nguồn thực thi
 ┣ 📂 data
 ┃  ┣ 📜 camera_cal.rar            # File nén chứa ảnh hiệu chỉnh camera (chessboard)
 ┃  ┣ 📜 project_video.mp4         # Video thử nghiệm chính đầu vào
 ┃  ┗ 📜 test_images.rar           # File nén chứa các ảnh kiểm thử
 ┣ 📂 output                       # Thư mục lưu trữ video/ảnh kết quả sau khi xử lý
 ┗ 📜 README.md                    # Tài liệu mô tả dự án

```

---

## Hướng Dẫn Chạy Dự Án

1. **Chuẩn bị môi trường:** Đảm bảo bạn đã cài đặt các thư viện cần thiết:
```bash
pip install opencv-python numpy matplotlib moviepy
```
2. **Giải nén dữ liệu:** Tiến hành giải nén các file `camera_cal.rar` và `test_images.rar` trong thư mục `data/` nếu cần sử dụng ảnh gốc.
3. **Thực thi mã nguồn:**
* Mở Jupyter Notebook tại đường dẫn `code/Lane_detection.ipynb`.
* Chạy lần lượt các khối lệnh từ trên xuống dưới.
4. **Kết quả:** Video sau khi xử lý sẽ được tự động xuất và lưu vào thư mục `output/`.
