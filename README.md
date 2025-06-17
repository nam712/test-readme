# Báo cáo SonarQube - Quản lý sản phẩm

## 1. Mô tả
Ứng dụng Java đơn giản dùng để quản lý sản phẩm, bao gồm các chức năng:
- Thêm sản phẩm
- Tìm kiếm sản phẩm
- Xóa sản phẩm

## 2. Công nghệ sử dụng

### 2.1. Ngôn ngữ và công cụ phát triển
- **Ngôn ngữ lập trình:** Java 
- **Trình quản lý dự án:** Apache Maven 3.9.6
- **Trình biên dịch:** Maven Compiler Plugin (target: Java 17)
- **Môi trường phát triển:** Visual Studio Code

### 2.2. Công cụ kiểm thử tĩnh
- **SonarQube:** Community Edition, chạy local trên trình duyệt Google Chrome tại địa chỉ `http://localhost:9000`
- **SonarScanner CLI:** Phiên bản 7.1.0.4889 (Windows x64)

#### Cấu hình và xác thực
- **Tài khoản mặc định SonarQube:** `admin` / `admin`
- **User Token:** Tạo thủ công tại trang `http://localhost:9000/account/security` sau khi đăng nhập.
- **Tập tin cấu hình:** `sonar-project.properties` được tạo trong thư mục gốc của dự án để cấu hình kết nối giữa SonarScanner và SonarQube server.


## 3. Phân tích ban đầu với SonarQube

### 3.1 Hướng dẫn chạy ứng dụng
1. Biên dịch dự án
   ```bash
   mvn clean compile
   ```
2. Chạy phân tích với SonarScanner:
    ```bash
    sonar-scanner
    ```
3. Xem kết quả tại địa chỉ:
    ```bash
    http://localhost:9000/dashboard?id=product-management
    ```
### 3.2  Kết quả phân tích ban đầu
SonarQube đã phát hiện một số vấn đề về khả năng bảo trì và cấu trúc mã:
| Đánh giá       | Giá trị                             |
|----------------|-----------------------------------|
| Security       | 0        |
| Reliability    | 0        |
| Maintainability| 4        | 
| Coverage| 0.0%        |
| Duplications| 0.0%        |
| Security Hotspots| 0       |

Dự án chưa tích hợp kiểm thử đơn vị nên độ phủ mã (coverage) là 0%.

### 3.3 Các vấn đề được phát hiện
| Loại          | Mô tả                                       | Gợi ý sửa                         |
|---------------|---------------------------------------------|-----------------------------------|
| Code Smell    | Sử dụng System.out.println thay vì Logger   | Kiểm tra null trước khi so sánh   |
| Code Smell    | Duplicated String Literal                   | Định nghĩa chuỗi đó thành một constant (private static final String) để tái sử dụng.     |
| Code Smell    | Không dùng break sau khi xóa              |  Thêm break để tối ưu vòng lặp   |

## 4. Cải thiện mã nguồn
### 4.1 Mã đã cải thiện - Main.java
- Dùng Logger thay cho System.out.println
- sử dụng thêm logger.isLoggable(Level.INFO) để đảm bảo phương thức findProduct() không bị gọi khi mức độ log chưa được kích hoạt
-  Định nghĩa chuỗi "Tablet" thành một constant (private static final String) để tái sử dụng.

### 4.2 Mã đã cải thiện - ProductManager.java
- Dùng Logger thay cho System.out.println
- Tối ưu vòng lặp bằng break

### 4.3 Chay lại ứng dụng
```bash
mvn clean compile
sonar-scanner
```

###  4.3 Kết quả sau cải thiện
| Đánh giá       |  Trước cải thiện                             |Sau cải thiện
|----------------|-----------------------------------|------------------------------|
| Security       | 0        |0|
| Reliability    | 0        |0|
| Maintainability| 4        | 0|
| Coverage| 0.0%        |0.0%|
| Duplications| 0.0%        |0.0%|
| Security Hotspots| 0       |0|

## Đánh giá 
SonarQube giúp phát hiện sớm các lỗi tiềm ẩn, code smell và vi phạm quy ước lập trình ngay trong quá trình phát triển. Nhờ đó, chất lượng mã nguồn được cải thiện, giảm thiểu lỗi khi triển khai, và tiết kiệm thời gian bảo trì về sau. Đây là công cụ hữu ích giúp lập trình viên viết code rõ ràng, dễ hiểu và dễ mở rộng.
