# [PROJECT-BASED LEARNING 1] Eulerian Circuit

## Giới thiệu
Đây là PBL1 - Dự án lập trình tính toán.

Dự án ứng dụng lí thuyết đồ thị để tìm chu trình Euler trên đồ thị vô hướng và đồ thị có hướng sử dụng thuật toán Fleury và thuật toán Hierholzer.

Ngoài ra, dự án còn mô phỏng chu trình Euler trên đồ thị với thư viện đồ họa.

## Cấu trúc dự án
```text
PBL1_EulerianCircuit/
|
├── algorithm.cpp   # Cốt lõi của thuật toán tìm chu trình Euler (Fleury & Hierholzer) 
├── algorithm.h      # Khai báo nguyên mẫu hàm cho algorithm.cpp
|
├── graphic.cpp      # Biểu diễn đồ thị và mô phỏng từng bước của chu trình Euler trên đồ thị
├── graphic.h        # Khai báo nguyên mẫu hàm cho graphic.cpp
|
├── ui.cpp           # Thiết kế giao diện người dùng
├── ui.h             # Khai báo nguyên mẫu hàm cho ui.cpp
|
|── main.cpp         # Điểm bắt đầu chương trình
|
└── README.md            
```

## Tổng quan về Thuật toán

### 1. Thuật toán Fleury

Thuật toán Fleury tìm chu trình Euler bằng cách duyệt qua các cạnh và xóa chúng trong thời gian thực, tuân theo quy tắc cốt lõi: "Không bao giờ đi qua cầu trừ khi không còn sự lựa chọn nào khác."

* **Bước 1: Kiểm tra tính hợp lệ của đồ thị & Kiểm tra tính liên thông**

  Xác minh tính liên thông tổng thể của đồ thị bằng thuật toán Tìm kiếm theo chiều sâu (DFS) phi đệ quy sử dụng stack mảng tĩnh. Kiểm tra bậc của các đỉnh để đảm bảo chu trình Euler tồn tại ($odd = 0$ đối với đồ thị vô hướng; $out_deg(v) = in_deg(v)$ với mọi $v$ trong đồ thị có hướng).
* **Bước 2: Khởi tạo & Nhân bản dữ liệu**

  Thiết lập $u = startNode$ và khởi tạo mảng đường đi. Nhân bản ma trận kề gốc sang một ma trận tạm (`temp_adj`) thông qua hàm memcpy để bảo vệ dữ liệu đồ thị ban đầu phục vụ cho việc mô phỏng.
* **Bước 3: Chọn cạnh & Kiểm tra cạnh cầu**

  Lặp qua các đỉnh kề $v$ của $u$. Đối với mỗi cạnh ứng viên $(u, v)$, thuật toán kiểm tra xem cạnh đó có phải là cầu hay không thông qua hàm `isBridge()`:
  * Đối với đồ thị vô hướng, thuật toán so sánh khả năng có thể đi tới trước và sau khi xóa cạnh (`countReachable()`).
  * Đối với đồ thị có hướng, thuật toán kiểm tra xem có đường đi quay lại từ $v$ về $u$ hay không (`isPathExist()`).
  * Nếu $(u, v)$ không phải là cầu, thuật toán sẽ chọn ngay $v$ và thoát vòng lặp. Nếu tất cả các cạnh còn lại đều là cầu, thuật toán sẽ quay lại chọn cạnh khả dụng đầu tiên.
* **Bước 4: Xóa cạnh & Cập nhật đường đi**

  Thêm $v$ vào mảng đường đi kết quả, giảm trọng số cạnh trong `temp_adj` (giảm ở cả hai chiều nếu là đồ thị vô hướng), và cập nhật $u = v$.
* **Bước 5: Hoàn thành**

  Lặp lại Bước 3–4 cho đến khi không còn cạnh nào sót lại. Trả về chuỗi chu trình Euler đã hoàn chỉnh.

---

### 2. Thuật toán Hierholzer

Thuật toán Hierholzer tìm chu trình Euler bằng cách phân rã đồ thị thành các chu trình con và ghép nối chúng lại với nhau bằng một stack.

* **Bước 1: Kiểm tra tính hợp lệ của đồ thị**

  Xác thực tính liên thông của đồ thị thông qua DFS phi đệ quy và xác nhận các yêu cầu cân bằng bậc cho sự tồn tại của đường đi Euler.
* **Bước 2: Thiết lập Stack & Bộ nhớ**

  Nhân bản ma trận kề bằng hàm memcpy. Khởi tạo một stack mảng tĩnh (`stackH`) với $startNode$ được đẩy vào stack, đồng thời tạo một mảng kết quả trống (`resH`).
* **Bước 3: Duyệt chu trình con & Quay lui**

  Kiểm tra đỉnh ở đỉnh stack $curr_v$:
  * **Trường hợp 1 (Có sẵn các cạnh chưa duyệt):**  Chọn một đỉnh kề $v$, đẩy $v$ vào stack, xóa cạnh $(curr_v, v)$ khỏi `temp_adj`, và tiếp tục quá trình duyệt để mở rộng chu trình con.
  * **Trường hợp 2 (Bị mắc kẹt / Không còn cạnh khả dụng):** Khi $curr_v$ không còn cạnh đi ra nào, một chu trình con đã được khép kín. Thuật toán lấy $curr_v$ ra khỏi stack và thêm nó vào resH (giai đoạn quay lui).
* **Bước 4: Đảo ngược đường đi & Hoàn thành**

  Lặp lại Bước 3 cho đến khi stack trống. Do các đỉnh được thu thập trong quá trình quay lui, thứ tự trong `resH` bị đảo ngược. Thuật toán tiến hành đảo ngược resH để tạo ra chu trình Euler theo đúng thứ tự thời gian cuối cùng và chính xác.
---

### 3. So sánh thuật toán

| Tiêu chí | Thuật toán Fleury | Thuật toán Hierholzer |
| :--- | :--- | :--- |
| **Nguyên lí** | Xóa cạnh dần dần, ưu tiên tránh các cạnh cầu (bridge) để giữ tính liên thông của đồ thị. | Phân rã đồ thị thành các chu trình đơn, sau đó nối chúng lại tại các đỉnh chung bằng Stack. |
| **Độ phức tạp thời gian** | $\mathcal{O}(E \times (V + E))$ | $\mathcal{O}(V \times E)$ |
| **Bộ nhớ** | Trung bình (chủ yếu lưu trữ cấu trúc đồ thị). | Cao hơn một chút do cần cấu trúc Stack để truy vết đường đi. |
| **Ưu điểm** | Trực quan, dễ hiểu dưới góc độ tư duy logic toán học. | Tốc độ thực thi rất nhanh; hiệu quả với đồ thị quy mô lớn. |
| **Nhược điểm** | Không khả thi với đồ thị lớn; hàm kiểm tra cạnh cầu phức tạp. | Đòi hỏi kỹ thuật quản lý ngăn xếp (Stack). |




---

## Hướng dẫn chạy chương trình

Thực hiện theo các bước sau để biên dịch và chạy ứng dụng trong Dev-C++:

1. **Cài đặt IDE & Thư viện đồ họa:**: 
   * **Dev-C++** IDE đã được cài đặt trên máy tính.
   * **Thư viện đồ họa WinBGIm** đã được cài đặt và liên kết chính xác với Dev-C++ để hỗ trợ tính năng trực quan hóa đồ thị. 

2. **Trình biên dịch**: GCC / G++.

3. **Mở Dự án**:
   * Khởi chạy **Dev-C++**.
   * Mở tệp dự án (`.dev`), hoặc mở đồng thời tất cả các tệp nguồn (`algorithm.cpp`, `graphic.cpp`, `ui.cpp`, `main.cpp`) và tệp tiêu đề (`.h`).
4. **Biên dịch và Chạy**:
   * Mở tệp `main.cpp`.
   * Nhấn **F11** (hoặc vào **Execute** ➔ **Compile & Run**).
   * Chương trình sẽ biên dịch mã nguồn, mở giao diện người dùng đồ họa tương tác (GUI) và cho phép quan sát các thuật toán Fleury và Hierholzer từng bước một cách trực quan.

## Thành viên dự án
* **Sinh viên thực hiện**: Hồ Tấn Phước & Phan Hữu Trí
* **Giảng viên hướng dẫn**: TS. Nguyễn Văn Hiệu
* **Môn học**: PBL1 - Dự án lập trình tính toán.
* **Trường**: Trường Đại học Bách Khoa - Đại học Đà Nẵng
