# Tài Liệu API - Module Quản Lý Khách Hàng (Customers)

## 📋 Mục Lục

1. [Tổng Quan Module](#tổng-quan-module)
2. [Công Nghệ Sử Dụng](#công-nghệ-sử-dụng)
3. [Kiến Trúc & Thiết Kế](#kiến-trúc--thiết-kế)
4. [Data Transfer Objects (DTOs)](#data-transfer-objects-dtos)
5. [API Endpoints Chi Tiết](#api-endpoints-chi-tiết)
6. [Luồng Nghiệp Vụ](#luồng-nghiệp-vụ)
7. [Thuật Toán & Logic](#thuật-toán--logic)
8. [Bảo Mật & Phân Quyền](#bảo-mật--phân-quyền)
9. [Tối Ưu Hiệu Năng](#tối-ưu-hiệu-năng)
10. [Câu Hỏi Thường Gặp](#câu-hỏi-thường-gặp)

---

## 1. Tổng Quan Module

### 1.1. Mục Đích

Module **Customers** quản lý toàn bộ thông tin khách hàng trong hệ thống bán hàng đa nền tảng, bao gồm:

- **CRUD** (Create, Read, Update, Delete) khách hàng
- **Tìm kiếm** theo tên, số điện thoại, email
- **Phân trang** (Pagination) cho danh sách lớn
- **Phân đoạn khách hàng** (Segmentation): VIP, thường, mới
- **Quản lý công nợ** (Debt Management)
- **Điểm thành viên** (Loyalty Points)
- **Kiểm tra giới hạn subscription** (SaaS multi-tenant)

### 1.2. Đặc Điểm Chính

✅ **Multi-Tenant**: Mỗi shop owner quản lý khách hàng riêng  
✅ **Subscription Limit**: Kiểm tra giới hạn số lượng khách hàng theo gói  
✅ **Search & Filter**: Tìm kiếm nhanh theo nhiều tiêu chí  
✅ **Pagination**: Hỗ trợ phân trang cho hiệu năng  
✅ **Authorization**: JWT-based, role-based access control

---

## 2. Công Nghệ Sử Dụng

### 2.1. Backend Framework

| Công Nghệ                 | Phiên Bản | Mục Đích                       |
| ------------------------- | --------- | ------------------------------ |
| **ASP.NET Core**          | 8.0       | Web API framework              |
| **Entity Framework Core** | 8.0       | ORM, database access           |
| **PostgreSQL**            | 15+       | Relational database            |
| **AutoMapper**            | 12.0      | DTO ↔ Entity mapping           |
| **JWT (JSON Web Token)**  | -         | Authentication & Authorization |
| **Swagger/OpenAPI**       | 3.0       | API documentation              |

### 2.2. Design Patterns

- **Repository Pattern**: Tách biệt data access logic
- **Service Layer Pattern**: Business logic layer
- **DTO Pattern**: Data transfer between layers
- **Dependency Injection**: IoC container trong ASP.NET Core
- **Async/Await**: Non-blocking I/O operations

### 2.3. Authentication & Authorization

- **JWT Token**: Bearer token trong HTTP Header
- **Claims-Based**: `shop_owner_id` claim để phân quyền
- **[Authorize] Attribute**: Bảo vệ endpoints

---

## 3. Kiến Trúc & Thiết Kế

### 3.1. Layered Architecture

```
┌─────────────────────────────────────────┐
│         CustomersController             │  ← API Layer
│  (HTTP Endpoints, Request Validation)   │
└─────────────────┬───────────────────────┘
                  │
                  ↓
┌─────────────────────────────────────────┐
│         ICustomerService                │  ← Service Layer
│  (Business Logic, Validation)           │
└─────────────────┬───────────────────────┘
                  │
                  ↓
┌─────────────────────────────────────────┐
│       ICustomerRepository               │  ← Data Access Layer
│  (CRUD, Database Queries)               │
└─────────────────┬───────────────────────┘
                  │
                  ↓
┌─────────────────────────────────────────┐
│       ApplicationDbContext              │  ← EF Core DbContext
│  (PostgreSQL Connection)                │
└─────────────────────────────────────────┘
```

### 3.2. Dependency Injection Setup

```csharp
// Program.cs
services.AddScoped<ICustomerService, CustomerService>();
services.AddScoped<ICustomerRepository, CustomerRepository>();
services.AddScoped<ISubscriptionLimitService, SubscriptionLimitService>();
```

### 3.3. Database Schema

**Bảng: `customers`**

| Column                  | Type          | Constraints                 | Mô Tả                                       |
| ----------------------- | ------------- | --------------------------- | ------------------------------------------- |
| `customer_id`           | INT           | PRIMARY KEY, AUTO_INCREMENT | ID khách hàng                               |
| `customer_code`         | VARCHAR(50)   | UNIQUE, NOT NULL            | Mã khách hàng (tự động hoặc thủ công)       |
| `customer_name`         | VARCHAR(255)  | NOT NULL                    | Tên khách hàng                              |
| `phone`                 | VARCHAR(20)   | INDEX                       | Số điện thoại                               |
| `email`                 | VARCHAR(100)  | INDEX                       | Email                                       |
| `address`               | TEXT          | -                           | Địa chỉ                                     |
| `tax_code`              | VARCHAR(50)   | -                           | Mã số thuế (doanh nghiệp)                   |
| `customer_type`         | VARCHAR(20)   | -                           | Loại: `individual`, `corporate`             |
| `date_of_birth`         | DATE          | -                           | Ngày sinh                                   |
| `gender`                | VARCHAR(10)   | -                           | Giới tính                                   |
| `id_card`               | VARCHAR(20)   | -                           | CMND/CCCD                                   |
| `bank_account`          | VARCHAR(50)   | -                           | Số tài khoản                                |
| `bank_name`             | VARCHAR(100)  | -                           | Tên ngân hàng                               |
| `total_debt`            | DECIMAL(18,2) | DEFAULT 0                   | Tổng công nợ                                |
| `total_purchase_amount` | DECIMAL(18,2) | DEFAULT 0                   | Tổng giá trị mua                            |
| `total_purchase_count`  | INT           | DEFAULT 0                   | Số lần mua                                  |
| `loyalty_points`        | INT           | DEFAULT 0                   | Điểm thành viên                             |
| `segment`               | VARCHAR(50)   | -                           | Phân đoạn: `VIP`, `Regular`, `New`          |
| `source`                | VARCHAR(50)   | -                           | Nguồn: `Walk-in`, `Online`, `Referral`      |
| `avatar_url`            | VARCHAR(500)  | -                           | URL ảnh đại diện                            |
| `status`                | VARCHAR(20)   | DEFAULT 'active'            | Trạng thái: `active`, `inactive`, `blocked` |
| `notes`                 | TEXT          | -                           | Ghi chú                                     |
| `shop_owner_id`         | INT           | FOREIGN KEY, NOT NULL       | ID chủ shop (multi-tenant)                  |
| `created_at`            | TIMESTAMP     | DEFAULT NOW()               | Ngày tạo                                    |
| `updated_at`            | TIMESTAMP     | AUTO UPDATE                 | Ngày cập nhật                               |

**Indexes:**

```sql
CREATE INDEX idx_customers_phone ON customers(phone);
CREATE INDEX idx_customers_email ON customers(email);
CREATE INDEX idx_customers_shop_owner ON customers(shop_owner_id);
CREATE INDEX idx_customers_segment ON customers(segment);
CREATE INDEX idx_customers_status ON customers(status);
```

---

## 4. Data Transfer Objects (DTOs)

### 4.1. CustomerDto

```csharp
public class CustomerDto
{
    public int CustomerId { get; set; }

    [Required(ErrorMessage = "Mã khách hàng không được để trống")]
    [MaxLength(50)]
    public string CustomerCode { get; set; } = string.Empty;

    [Required(ErrorMessage = "Tên khách hàng không được để trống")]
    [MaxLength(255)]
    public string CustomerName { get; set; } = string.Empty;

    [MaxLength(20)]
    public string Phone { get; set; } = string.Empty;

    [EmailAddress]
    [MaxLength(100)]
    public string? Email { get; set; }

    public string? Address { get; set; }
    public string? TaxCode { get; set; }
    public string? CustomerType { get; set; }
    public DateTime? DateOfBirth { get; set; }
    public string? Gender { get; set; }
    public string? IdCard { get; set; }
    public string? BankAccount { get; set; }
    public string? BankName { get; set; }
    public decimal? TotalDebt { get; set; }
    public decimal? TotalPurchaseAmount { get; set; }
    public int? TotalPurchaseCount { get; set; }
    public int? LoyaltyPoints { get; set; }
    public string? Segment { get; set; }
    public string? Source { get; set; }
    public string? AvatarUrl { get; set; }
    public string? Status { get; set; }
    public string? Notes { get; set; }
    public DateTime CreatedAt { get; set; }
    public DateTime UpdatedAt { get; set; }
}
```

### 4.2. PaginationRequest

```csharp
public class PaginationRequest
{
    public int Page { get; set; } = 1;
    public int PageSize { get; set; } = 10;
    public string? SortBy { get; set; }
    public string? SortOrder { get; set; } = "asc";
}
```

### 4.3. PaginatedResponse<T>

```csharp
public class PaginatedResponse<T>
{
    public IEnumerable<T> Data { get; set; }
    public int TotalCount { get; set; }
    public int Page { get; set; }
    public int PageSize { get; set; }
    public int TotalPages => (int)Math.Ceiling((double)TotalCount / PageSize);
    public bool HasPreviousPage => Page > 1;
    public bool HasNextPage => Page < TotalPages;
}
```

---

## 5. API Endpoints Chi Tiết

### 5.1. GET /api/customers

**Mục đích:** Lấy toàn bộ danh sách khách hàng (không phân trang)

**Request:**

```http
GET /api/customers HTTP/1.1
Host: api.example.com
Authorization: Bearer {jwt_token}
```

**Response Success (200 OK):**

```json
[
  {
    "customerId": 1,
    "customerCode": "KH001",
    "customerName": "Nguyễn Văn A",
    "phone": "0901234567",
    "email": "nguyenvana@example.com",
    "address": "123 Lê Lợi, Q1, TP.HCM",
    "customerType": "individual",
    "totalDebt": 0,
    "totalPurchaseAmount": 15000000,
    "totalPurchaseCount": 25,
    "loyaltyPoints": 1500,
    "segment": "VIP",
    "status": "active",
    "createdAt": "2024-01-15T10:30:00Z",
    "updatedAt": "2024-12-20T14:45:00Z"
  }
]
```

**Luồng Xử Lý:**

```
1. Client gửi GET request với JWT token
2. Middleware kiểm tra JWT token hợp lệ
3. Controller extract shop_owner_id từ token claims
4. Kiểm tra shop_owner_id có tồn tại không
5. Gọi _service.GetAllAsync()
6. Service gọi Repository để query database
7. Repository filter theo shop_owner_id
8. EF Core thực thi SQL:
   SELECT * FROM customers WHERE shop_owner_id = {id} AND status != 'deleted'
9. AutoMapper map Entity → DTO
10. Return danh sách CustomerDto[]
```

---

### 5.2. GET /api/customers/paginated

**Mục đích:** Lấy danh sách khách hàng có phân trang

**Request:**

```http
GET /api/customers/paginated?page=1&pageSize=20&sortBy=customerName&sortOrder=asc HTTP/1.1
Host: api.example.com
Authorization: Bearer {jwt_token}
```

**Query Parameters:**

| Parameter   | Type   | Required | Default | Mô Tả                 |
| ----------- | ------ | -------- | ------- | --------------------- |
| `page`      | int    | No       | 1       | Trang hiện tại        |
| `pageSize`  | int    | No       | 10      | Số bản ghi mỗi trang  |
| `sortBy`    | string | No       | -       | Trường sắp xếp        |
| `sortOrder` | string | No       | asc     | Thứ tự: `asc`, `desc` |

**Response Success (200 OK):**

```json
{
  "data": [
    {
      "customerId": 1,
      "customerCode": "KH001",
      "customerName": "Nguyễn Văn A",
      "phone": "0901234567",
      "email": "nguyenvana@example.com",
      "segment": "VIP",
      "status": "active"
    }
  ],
  "totalCount": 1250,
  "page": 1,
  "pageSize": 20,
  "totalPages": 63,
  "hasPreviousPage": false,
  "hasNextPage": true
}
```

**SQL Query Generated:**

```sql
SELECT * FROM customers
WHERE shop_owner_id = @shopOwnerId
ORDER BY customer_name ASC
LIMIT 20 OFFSET 0;

SELECT COUNT(*) FROM customers
WHERE shop_owner_id = @shopOwnerId;
```

**Lợi Ích Pagination:**

- Giảm tải database khi có hàng nghìn khách hàng
- Tăng tốc độ response time (chỉ trả 20 bản ghi thay vì 1000+)
- Tiết kiệm băng thông mạng
- Cải thiện UX (người dùng không phải đợi load hết dữ liệu)

---

### 5.3. GET /api/customers/{id}

**Mục đích:** Lấy thông tin chi tiết một khách hàng

**Request:**

```http
GET /api/customers/123 HTTP/1.1
Host: api.example.com
Authorization: Bearer {jwt_token}
```

**Response Success (200 OK):**

```json
{
  "customerId": 123,
  "customerCode": "KH123",
  "customerName": "Trần Thị B",
  "phone": "0912345678",
  "email": "tranthib@example.com",
  "address": "456 Nguyễn Huệ, Q1, TP.HCM",
  "taxCode": null,
  "customerType": "individual",
  "dateOfBirth": "1990-05-15",
  "gender": "female",
  "idCard": "079090012345",
  "bankAccount": "0011223344",
  "bankName": "Vietcombank",
  "totalDebt": 500000,
  "totalPurchaseAmount": 25000000,
  "totalPurchaseCount": 45,
  "loyaltyPoints": 2500,
  "segment": "VIP",
  "source": "Online",
  "avatarUrl": "https://storage.googleapis.com/bucket/avatars/123.jpg",
  "status": "active",
  "notes": "Khách hàng VIP, ưu tiên phục vụ",
  "createdAt": "2023-03-10T08:20:00Z",
  "updatedAt": "2024-12-25T16:30:00Z"
}
```

**Response Error (404 Not Found):**

```json
{
  "message": "Không tìm thấy khách hàng với ID = 123"
}
```

**Response Error (401 Unauthorized):**

```json
{
  "success": false,
  "message": "Không tìm thấy thông tin chủ shop"
}
```

---

### 5.4. GET /api/customers/search

**Mục đích:** Tìm kiếm khách hàng theo tên

**Request:**

```http
GET /api/customers/search?name=Nguyễn HTTP/1.1
Host: api.example.com
Authorization: Bearer {jwt_token}
```

**Query Parameters:**

| Parameter | Type   | Required | Mô Tả                                 |
| --------- | ------ | -------- | ------------------------------------- |
| `name`    | string | No       | Từ khóa tìm kiếm trong tên khách hàng |

**Response Success (200 OK):**

```json
[
  {
    "customerId": 1,
    "customerCode": "KH001",
    "customerName": "Nguyễn Văn A",
    "phone": "0901234567",
    "email": "nguyenvana@example.com",
    "segment": "VIP",
    "status": "active"
  },
  {
    "customerId": 25,
    "customerCode": "KH025",
    "customerName": "Nguyễn Thị C",
    "phone": "0923456789",
    "email": "nguyenthic@example.com",
    "segment": "Regular",
    "status": "active"
  }
]
```

**SQL Query:**

```sql
SELECT * FROM customers
WHERE shop_owner_id = @shopOwnerId
  AND customer_name ILIKE '%Nguyễn%'
  AND status != 'deleted'
ORDER BY customer_name ASC;
```

**Thuật Toán Tìm Kiếm:**

1. **ILIKE Operator** (PostgreSQL): Case-insensitive search
2. **Wildcard Pattern**: `%keyword%` để tìm substring
3. **Index**: Sử dụng GIN index cho tìm kiếm full-text nếu cần
4. **Optimization**: Limit kết quả nếu quá nhiều (top 100)

---

### 5.5. POST /api/customers

**Mục đích:** Tạo khách hàng mới

**Request:**

```http
POST /api/customers HTTP/1.1
Host: api.example.com
Authorization: Bearer {jwt_token}
Content-Type: application/json

{
  "customerCode": "KH999",
  "customerName": "Lê Văn D",
  "phone": "0934567890",
  "email": "levand@example.com",
  "address": "789 Trần Hưng Đạo, Q5, TP.HCM",
  "customerType": "individual",
  "dateOfBirth": "1995-08-20",
  "gender": "male",
  "source": "Walk-in",
  "status": "active"
}
```

**Response Success (201 Created):**

```json
{
  "customerId": 999,
  "customerCode": "KH999",
  "customerName": "Lê Văn D",
  "phone": "0934567890",
  "email": "levand@example.com",
  "totalDebt": 0,
  "totalPurchaseAmount": 0,
  "totalPurchaseCount": 0,
  "loyaltyPoints": 0,
  "segment": "New",
  "status": "active",
  "createdAt": "2025-01-08T10:00:00Z",
  "updatedAt": "2025-01-08T10:00:00Z"
}
```

**Response Error (400 Bad Request) - Vượt Giới Hạn Subscription:**

```json
{
  "success": false,
  "message": "Đã đạt giới hạn số lượng khách hàng cho gói hiện tại. Vui lòng nâng cấp gói.",
  "limitReached": true,
  "currentCount": 1000,
  "maxLimit": 1000
}
```

**Validation Rules:**

- `customerCode`: Required, unique trong shop_owner_id
- `customerName`: Required, max 255 ký tự
- `phone`: Max 20 ký tự
- `email`: Valid email format
- `status`: Default 'active'

**Luồng Kiểm Tra Subscription Limit:**

```
1. Extract shop_owner_id từ JWT token
2. Gọi _limitService.CheckCustomerLimitAsync(shopOwnerId)
3. Service query bảng `subscriptions`:
   SELECT max_customers FROM subscriptions WHERE shop_owner_id = @id AND status = 'active'
4. Query count hiện tại:
   SELECT COUNT(*) FROM customers WHERE shop_owner_id = @id AND status != 'deleted'
5. So sánh: currentCount < maxLimit
6. Nếu vượt → Return error 400
7. Nếu OK → Tiếp tục tạo customer
```

---

### 5.6. PUT /api/customers/{id}

**Mục đích:** Cập nhật thông tin khách hàng

**Request:**

```http
PUT /api/customers/999 HTTP/1.1
Host: api.example.com
Authorization: Bearer {jwt_token}
Content-Type: application/json

{
  "customerId": 999,
  "customerCode": "KH999",
  "customerName": "Lê Văn D (Updated)",
  "phone": "0934567890",
  "email": "levand_new@example.com",
  "address": "789 Trần Hưng Đạo, Q5, TP.HCM",
  "segment": "Regular",
  "loyaltyPoints": 150,
  "notes": "Khách hàng đã mua 3 lần",
  "status": "active"
}
```

**Response Success (200 OK):**

```json
{
  "message": "Cập nhật khách hàng thành công",
  "customerId": 999,
  "customerCode": "KH999",
  "customerName": "Lê Văn D (Updated)"
}
```

**Response Error (400 Bad Request):**

```json
{
  "message": "ID trong URL và ID trong dữ liệu không khớp"
}
```

**Business Logic:**

- Kiểm tra `customerId` trong URL và body phải khớp
- Chỉ update các fields được gửi lên (partial update)
- Tự động update `updated_at` timestamp
- Không cho phép update `shop_owner_id` (security)

---

### 5.7. DELETE /api/customers/{id}

**Mục đích:** Xóa khách hàng

**Request:**

```http
DELETE /api/customers/999 HTTP/1.1
Host: api.example.com
Authorization: Bearer {jwt_token}
```

**Response Success (200 OK):**

```json
{
  "message": "Xóa khách hàng thành công",
  "customerId": 999,
  "customerCode": "KH999",
  "customerName": "Lê Văn D"
}
```

**Response Error (404 Not Found):**

```json
{
  "message": "Không tìm thấy khách hàng với ID = 999"
}
```

**Cơ Chế Xóa:**

1. **Soft Delete (Khuyến nghị):**
   - Không xóa vật lý khỏi database
   - Update `status = 'deleted'`
   - Giữ lại dữ liệu để audit, báo cáo
2. **Hard Delete:**
   - Xóa vật lý khỏi database
   - Kiểm tra ràng buộc foreign key (invoices, orders)
   - Cascade delete hoặc prevent delete nếu có dữ liệu liên quan

**SQL (Soft Delete):**

```sql
UPDATE customers
SET status = 'deleted', updated_at = NOW()
WHERE customer_id = @id AND shop_owner_id = @shopOwnerId;
```

---

## 6. Luồng Nghiệp Vụ

### 6.1. Luồng Tạo Khách Hàng Mới

```
┌────────────┐
│   Client   │
└─────┬──────┘
      │ POST /api/customers
      │ + JWT Token
      │ + CustomerDto
      ↓
┌─────────────────────────┐
│   CustomersController   │
└─────────┬───────────────┘
          │ 1. Validate JWT
          │ 2. Extract shop_owner_id
          ↓
┌──────────────────────────────┐
│  ISubscriptionLimitService   │
└─────────┬────────────────────┘
          │ 3. CheckCustomerLimitAsync()
          │ Query subscriptions table
          │ Compare current vs max
          ↓
      ┌───┴────┐
      │ Limit? │
      └───┬────┘
         NO│    YES→ Return 400 Error
          ↓
┌─────────────────────┐
│  ICustomerService   │
└─────────┬───────────┘
          │ 4. AddAsync(customerDto)
          │ Business validation
          │ Auto-generate fields
          ↓
┌─────────────────────────┐
│  ICustomerRepository    │
└─────────┬───────────────┘
          │ 5. Insert into database
          │ SET shop_owner_id
          │ SET created_at, updated_at
          │ SET segment = 'New'
          ↓
┌─────────────────────────┐
│   PostgreSQL Database   │
└─────────┬───────────────┘
          │ 6. Return Customer entity
          ↓
┌─────────────────────┐
│    AutoMapper       │
└─────────┬───────────┘
          │ 7. Map Entity → DTO
          ↓
┌─────────────────────────┐
│   Return 201 Created    │
│   Location: /api/       │
│   customers/{id}        │
└─────────────────────────┘
```

### 6.2. Luồng Tìm Kiếm Khách Hàng

```
Client Request
     ↓
GET /api/customers/search?name=Nguyễn
     ↓
[CustomersController]
  1. Validate JWT token
  2. Extract shop_owner_id claim
  3. Validate shop_owner_id not null
     ↓
[ICustomerService]
  4. SearchAsync(name)
  5. Sanitize search keyword (trim, lowercase)
  6. Call repository
     ↓
[ICustomerRepository]
  7. Query database với ILIKE operator
  8. Filter by shop_owner_id
  9. Filter status != 'deleted'
  10. Apply LIMIT (e.g., 100 results)
     ↓
[PostgreSQL]
  11. Execute query với index scan
  12. Return matching rows
     ↓
[AutoMapper]
  13. Map List<Customer> → List<CustomerDto>
     ↓
Return 200 OK với danh sách kết quả
```

### 6.3. Luồng Phân Trang (Pagination)

```
Client Request: page=3, pageSize=20
     ↓
Tính toán OFFSET:
  OFFSET = (page - 1) × pageSize
  OFFSET = (3 - 1) × 20 = 40
     ↓
Query 1: Lấy dữ liệu
  SELECT * FROM customers
  WHERE shop_owner_id = @id
  ORDER BY customer_name ASC
  LIMIT 20 OFFSET 40;
     ↓
Query 2: Đếm tổng số
  SELECT COUNT(*) FROM customers
  WHERE shop_owner_id = @id;
     ↓
Kết quả:
  - Data: 20 bản ghi (từ 41-60)
  - TotalCount: 1250
  - TotalPages: CEIL(1250 / 20) = 63
  - HasNextPage: page < totalPages → true
  - HasPreviousPage: page > 1 → true
```

---

## 7. Thuật Toán & Logic

### 7.1. Thuật Toán Tìm Kiếm (Search Algorithm)

**Yêu Cầu:**

- Tìm kiếm không phân biệt hoa thường
- Hỗ trợ tìm substring (partial match)
- Tìm kiếm đa trường (name, phone, email)

**Cách Tiếp Cận 1: ILIKE Operator (PostgreSQL)**

```sql
SELECT * FROM customers
WHERE shop_owner_id = @shopOwnerId
  AND (
    customer_name ILIKE '%keyword%'
    OR phone ILIKE '%keyword%'
    OR email ILIKE '%keyword%'
  )
  AND status != 'deleted'
ORDER BY customer_name ASC
LIMIT 100;
```

**Ưu Điểm:**

- Đơn giản, dễ implement
- Không cần setup thêm

**Nhược Điểm:**

- Chậm với bảng lớn (không dùng được index B-tree)
- Không hỗ trợ ranked search
- Không handle typo, synonym

**Cách Tiếp Cận 2: Full-Text Search (FTS)**

```sql
-- Tạo tsvector column
ALTER TABLE customers ADD COLUMN search_vector tsvector;

-- Tạo trigger update search_vector
CREATE OR REPLACE FUNCTION customers_search_trigger() RETURNS trigger AS $$
BEGIN
  NEW.search_vector :=
    setweight(to_tsvector('english', COALESCE(NEW.customer_name, '')), 'A') ||
    setweight(to_tsvector('english', COALESCE(NEW.phone, '')), 'B') ||
    setweight(to_tsvector('english', COALESCE(NEW.email, '')), 'C');
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Query với FTS
SELECT * FROM customers
WHERE shop_owner_id = @shopOwnerId
  AND search_vector @@ to_tsquery('english', 'keyword:*')
  AND status != 'deleted'
ORDER BY ts_rank(search_vector, to_tsquery('english', 'keyword:*')) DESC
LIMIT 100;
```

**Ưu Điểm:**

- Nhanh hơn ILIKE (dùng GIN index)
- Hỗ trợ ranked results
- Xử lý stemming (run → running)

**Nhược Điểm:**

- Phức tạp hơn
- Tốn storage cho search_vector column

### 7.2. Thuật Toán Phân Đoạn Khách Hàng (Segmentation)

**Mục Tiêu:** Tự động phân loại khách hàng thành VIP, Regular, New

**Logic:**

```csharp
public string CalculateSegment(Customer customer)
{
    // New customer: ít hơn 3 đơn hàng
    if (customer.TotalPurchaseCount < 3)
        return "New";

    // VIP: Mua trên 50 triệu hoặc >50 đơn
    if (customer.TotalPurchaseAmount >= 50_000_000 ||
        customer.TotalPurchaseCount >= 50)
        return "VIP";

    // Regular: Còn lại
    return "Regular";
}
```

**Trigger Update Segment:**

```sql
CREATE OR REPLACE FUNCTION update_customer_segment() RETURNS trigger AS $$
BEGIN
  IF NEW.total_purchase_count < 3 THEN
    NEW.segment := 'New';
  ELSIF NEW.total_purchase_amount >= 50000000 OR NEW.total_purchase_count >= 50 THEN
    NEW.segment := 'VIP';
  ELSE
    NEW.segment := 'Regular';
  END IF;
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER customers_segment_trigger
BEFORE INSERT OR UPDATE ON customers
FOR EACH ROW EXECUTE FUNCTION update_customer_segment();
```

### 7.3. Thuật Toán Tính Điểm Thành Viên (Loyalty Points)

**Quy Tắc:**

- Tích 1 điểm cho mỗi 10,000đ chi tiêu
- Bonus 50 điểm cho đơn hàng đầu tiên
- Bonus 100 điểm vào sinh nhật

**Code:**

```csharp
public int CalculateLoyaltyPoints(decimal purchaseAmount, bool isFirstOrder, bool isBirthday)
{
    int basePoints = (int)(purchaseAmount / 10000);
    int bonusPoints = 0;

    if (isFirstOrder)
        bonusPoints += 50;

    if (isBirthday)
        bonusPoints += 100;

    return basePoints + bonusPoints;
}
```

---

## 8. Bảo Mật & Phân Quyền

### 8.1. JWT Authentication

**Token Structure:**

```
Header:
{
  "alg": "HS256",
  "typ": "JWT"
}

Payload:
{
  "sub": "shopowner@example.com",
  "shop_owner_id": "123",
  "role": "ShopOwner",
  "exp": 1736342400,
  "iat": 1736256000
}

Signature:
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret
)
```

**Validation Flow:**

```
1. Client gửi request với Header:
   Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

2. ASP.NET Core Middleware validate:
   - Token signature hợp lệ?
   - Token chưa hết hạn?
   - Issuer & Audience đúng?

3. Nếu hợp lệ → Parse claims
4. Set HttpContext.User với claims
5. Controller truy cập: User.FindFirst("shop_owner_id")
```

### 8.2. Authorization Logic

**Kiểm Tra Shop Owner:**

```csharp
var shopOwnerIdClaim = User.FindFirst("shop_owner_id")?.Value;
if (string.IsNullOrEmpty(shopOwnerIdClaim) ||
    !int.TryParse(shopOwnerIdClaim, out int shopOwnerId))
{
    return Unauthorized(new {
        success = false,
        message = "Không tìm thấy thông tin chủ shop"
    });
}
```

**Multi-Tenant Isolation:**

- Mọi query đều filter theo `shop_owner_id`
- Một shop owner không thể truy cập dữ liệu của shop khác
- Database-level: Row-Level Security (RLS) có thể enable

**SQL Injection Prevention:**

- Sử dụng **Parameterized Queries** (EF Core tự động)
- Không concatenate string vào SQL
- Validate & sanitize input

**XSS Prevention:**

- Encode output khi render HTML
- Content-Security-Policy headers
- Validate input (không cho phép `<script>` tags)

---

## 9. Tối Ưu Hiệu Năng

### 9.1. Database Indexing

**Index Strategy:**

```sql
-- Primary lookups
CREATE INDEX idx_customers_id ON customers(customer_id);

-- Search queries
CREATE INDEX idx_customers_name ON customers(customer_name);
CREATE INDEX idx_customers_phone ON customers(phone);
CREATE INDEX idx_customers_email ON customers(email);

-- Multi-tenant filtering
CREATE INDEX idx_customers_shop_owner ON customers(shop_owner_id);

-- Composite index for common filter
CREATE INDEX idx_customers_shop_status ON customers(shop_owner_id, status);

-- Full-text search
CREATE INDEX idx_customers_fts ON customers USING GIN(search_vector);
```

**Explain Analyze:**

```sql
EXPLAIN ANALYZE
SELECT * FROM customers
WHERE shop_owner_id = 123 AND status = 'active';

-- Result:
-- Index Scan using idx_customers_shop_status (cost=0.29..8.31 rows=1 width=500)
-- Index Cond: ((shop_owner_id = 123) AND (status = 'active'))
-- Execution Time: 0.045 ms
```

### 9.2. Caching Strategy

**Memory Cache (In-Process):**

```csharp
public class CustomerService : ICustomerService
{
    private readonly IMemoryCache _cache;

    public async Task<CustomerDto> GetByIdAsync(int id)
    {
        string cacheKey = $"customer_{id}";

        if (!_cache.TryGetValue(cacheKey, out CustomerDto customer))
        {
            customer = await _repository.GetByIdAsync(id);

            _cache.Set(cacheKey, customer, new MemoryCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(10),
                SlidingExpiration = TimeSpan.FromMinutes(5)
            });
        }

        return customer;
    }
}
```

**Redis Cache (Distributed):**

```csharp
public async Task<CustomerDto> GetByIdAsync(int id)
{
    string cacheKey = $"customer_{id}";

    var cachedValue = await _redis.StringGetAsync(cacheKey);
    if (!cachedValue.IsNullOrEmpty)
    {
        return JsonSerializer.Deserialize<CustomerDto>(cachedValue);
    }

    var customer = await _repository.GetByIdAsync(id);

    await _redis.StringSetAsync(
        cacheKey,
        JsonSerializer.Serialize(customer),
        TimeSpan.FromMinutes(10)
    );

    return customer;
}
```

**Cache Invalidation:**

```csharp
public async Task UpdateAsync(CustomerDto dto)
{
    await _repository.UpdateAsync(dto);

    // Invalidate cache
    await _redis.KeyDeleteAsync($"customer_{dto.CustomerId}");
    await _redis.KeyDeleteAsync($"customers_list_{dto.ShopOwnerId}");
}
```

### 9.3. Query Optimization

**N+1 Problem:**

```csharp
// ❌ BAD: N+1 queries
var customers = await _context.Customers.ToListAsync();
foreach (var customer in customers)
{
    // Mỗi lần loop query thêm 1 lần!
    customer.Invoices = await _context.Invoices
        .Where(i => i.CustomerId == customer.CustomerId)
        .ToListAsync();
}

// ✅ GOOD: Eager loading
var customers = await _context.Customers
    .Include(c => c.Invoices)
    .ToListAsync();
```

**Select Only Needed Columns:**

```csharp
// ❌ BAD: Select *
var customers = await _context.Customers.ToListAsync();

// ✅ GOOD: Select specific fields
var customers = await _context.Customers
    .Select(c => new CustomerSummaryDto
    {
        CustomerId = c.CustomerId,
        CustomerName = c.CustomerName,
        Phone = c.Phone,
        Segment = c.Segment
    })
    .ToListAsync();
```

### 9.4. Async/Await Pattern

**Non-Blocking I/O:**

```csharp
// ❌ BAD: Blocking
public CustomerDto GetById(int id)
{
    return _repository.GetById(id); // Blocks thread
}

// ✅ GOOD: Async
public async Task<CustomerDto> GetByIdAsync(int id)
{
    return await _repository.GetByIdAsync(id); // Non-blocking
}
```

**Parallel Queries:**

```csharp
public async Task<DashboardDto> GetDashboardAsync(int shopOwnerId)
{
    // Execute 3 queries in parallel
    var customersTask = _customerRepo.GetCountAsync(shopOwnerId);
    var revenueTask = _invoiceRepo.GetTotalRevenueAsync(shopOwnerId);
    var ordersTask = _orderRepo.GetCountAsync(shopOwnerId);

    await Task.WhenAll(customersTask, revenueTask, ordersTask);

    return new DashboardDto
    {
        TotalCustomers = customersTask.Result,
        TotalRevenue = revenueTask.Result,
        TotalOrders = ordersTask.Result
    };
}
```

---

## 10. Câu Hỏi Thường Gặp (FAQ)

### Câu 1: **Em giải thích về kiến trúc của module Customers?**

**Trả lời:**

> Em sử dụng **Layered Architecture** với 4 layers:
>
> 1. **API Layer (Controller)**: Nhận HTTP requests, validate input, trả response
> 2. **Service Layer**: Chứa business logic, validation, orchestration
> 3. **Repository Layer**: Data access, CRUD operations với database
> 4. **Database Layer**: PostgreSQL, EF Core DbContext
>
> Lợi ích:
>
> - **Separation of Concerns**: Mỗi layer có trách nhiệm riêng
> - **Testability**: Có thể unit test từng layer độc lập
> - **Maintainability**: Dễ sửa đổi, mở rộng
> - **Reusability**: Service/Repository có thể dùng chung cho nhiều controllers

### Câu 2: **Tại sao em sử dụng DTO (Data Transfer Object)?**

**Trả lời:**

> **DTO** giúp:
>
> 1. **Tách biệt Entity và API response**: Entity có thể có nhiều fields nhạy cảm (password, internal IDs) không nên expose ra ngoài
> 2. **Customize output**: Có thể thêm computed fields (TotalPages, HasNextPage) mà Entity không có
> 3. **Versioning**: Khi thay đổi Entity, DTO không bị ảnh hưởng (backward compatibility)
> 4. **Validation**: Áp dụng validation attributes ([Required], [MaxLength]) riêng cho API input
> 5. **Performance**: Chỉ serialize fields cần thiết, giảm payload size

### Câu 3: **Em giải thích luồng xử lý khi tạo khách hàng mới?**

**Trả lời:**

> 1. Client gửi POST request với JWT token và CustomerDto
> 2. Middleware validate JWT token hợp lệ
> 3. Controller extract `shop_owner_id` từ token claims
> 4. **Kiểm tra giới hạn subscription**: Gọi `ISubscriptionLimitService`
>    - Query bảng `subscriptions` lấy `max_customers`
>    - Query bảng `customers` đếm số lượng hiện tại
>    - So sánh: nếu vượt → Return 400 Error
> 5. Nếu OK, gọi `ICustomerService.AddAsync()`
>    - Validate business rules (unique customerCode, valid email, etc.)
>    - Auto-generate fields (segment = 'New', loyalty_points = 0)
> 6. Service gọi `ICustomerRepository.AddAsync()`
>    - EF Core insert vào database
>    - Set `shop_owner_id`, `created_at`, `updated_at`
> 7. AutoMapper map Entity → DTO
> 8. Return 201 Created với Location header

### Câu 4: **Tại sao em sử dụng Pagination? Lợi ích là gì?**

**Trả lời:**

> **Lợi ích của Pagination:**
>
> 1. **Hiệu năng**: Chỉ query 20 bản ghi thay vì 1000+ → Giảm tải database
> 2. **Bandwidth**: Giảm kích thước response → Tiết kiệm băng thông mạng
> 3. **User Experience**: Load nhanh hơn, không phải đợi cả list
> 4. **Memory**: Giảm memory usage trên server và client
> 5. **Scalability**: Hệ thống scale tốt hơn khi có nhiều user
>
> **Cơ chế:**
>
> - SQL: `LIMIT {pageSize} OFFSET {(page-1) * pageSize}`
> - Trả về metadata: TotalPages, HasNextPage, HasPreviousPage
> - Frontend render pagination UI (1, 2, 3, ..., Next)

### Câu 5: **Em xử lý bảo mật như thế nào trong module này?**

**Trả lời:**

> 1. **Authentication**: JWT token với `[Authorize]` attribute
>
>    - Token signature verification
>    - Expiration check
>    - Claims extraction
>
> 2. **Authorization**: Multi-tenant isolation
>
>    - Mọi query filter theo `shop_owner_id`
>    - Shop A không thể truy cập data của Shop B
>
> 3. **SQL Injection Prevention**: Parameterized queries (EF Core)
>
> 4. **XSS Prevention**: Sanitize input, encode output
>
> 5. **HTTPS Only**: Encrypt data in transit
>
> 6. **Rate Limiting**: Giới hạn số request/phút (có thể thêm middleware)

### Câu 6: **Em giải thích về thuật toán tìm kiếm khách hàng?**

**Trả lời:**

> Em sử dụng **ILIKE operator** của PostgreSQL:
>
> ```sql
> SELECT * FROM customers
> WHERE shop_owner_id = @id
>   AND customer_name ILIKE '%keyword%'
>   AND status != 'deleted'
> ORDER BY customer_name ASC
> LIMIT 100;
> ```
>
> **Đặc điểm:**
>
> - **ILIKE**: Case-insensitive (không phân biệt hoa thường)
> - **Wildcard %**: Tìm substring (partial match)
> - **Multi-field search**: Có thể OR với phone, email
> - **Limit 100**: Giới hạn kết quả để tránh quá tải
>
> **Optimization:**
>
> - Sử dụng **GIN index** với Full-Text Search cho bảng lớn
> - Hoặc **trigram index** (pg_trgm extension) cho fuzzy search

### Câu 7: **Repository Pattern là gì? Tại sao em dùng nó?**

**Trả lời:**

> **Repository Pattern** là design pattern tách biệt data access logic khỏi business logic.
>
> **Interface:**
>
> ```csharp
> public interface ICustomerRepository
> {
>     Task<IEnumerable<Customer>> GetAllAsync();
>     Task<Customer> GetByIdAsync(int id);
>     Task<Customer> AddAsync(Customer customer);
>     Task UpdateAsync(Customer customer);
>     Task DeleteAsync(int id);
>     Task<IEnumerable<Customer>> SearchAsync(string keyword);
> }
> ```
>
> **Lợi ích:**
>
> 1. **Abstraction**: Service layer không cần biết database implementation
> 2. **Testability**: Dễ mock repository trong unit tests
> 3. **Flexibility**: Dễ thay đổi database (SQL → NoSQL) mà không ảnh hưởng service layer
> 4. **Reusability**: Có thể dùng chung cho nhiều services
> 5. **Maintainability**: Code rõ ràng, dễ bảo trì

### Câu 8: **Dependency Injection hoạt động như thế nào?**

**Trả lời:**

> **Dependency Injection (DI)** là IoC (Inversion of Control) pattern.
>
> **Setup trong Program.cs:**
>
> ```csharp
> builder.Services.AddScoped<ICustomerService, CustomerService>();
> builder.Services.AddScoped<ICustomerRepository, CustomerRepository>();
> ```
>
> **Injection vào Controller:**
>
> ```csharp
> public CustomersController(
>     ICustomerService service,
>     ISubscriptionLimitService limitService)
> {
>     _service = service;
>     _limitService = limitService;
> }
> ```
>
> **Cách hoạt động:**
>
> 1. ASP.NET Core IoC container tạo instance của `CustomerService`
> 2. Container inject `ICustomerRepository` vào constructor của `CustomerService`
> 3. Container inject `CustomerService` vào constructor của `CustomersController`
> 4. **Scoped lifetime**: Mỗi HTTP request tạo 1 instance mới
>
> **Lợi ích:**
>
> - **Loose coupling**: Controller không phụ thuộc vào implementation cụ thể
> - **Testability**: Dễ mock dependencies
> - **Lifetime management**: Container tự động dispose

### Câu 9: **AutoMapper là gì? Tại sao dùng nó?**

**Trả lời:**

> **AutoMapper** là thư viện mapping object ↔ object tự động.
>
> **Mapping Profile:**
>
> ```csharp
> public class CustomerMappingProfile : Profile
> {
>     public CustomerMappingProfile()
>     {
>         CreateMap<Customer, CustomerDto>();
>         CreateMap<CustomerDto, Customer>();
>     }
> }
> ```
>
> **Usage:**
>
> ```csharp
> var customerDto = _mapper.Map<CustomerDto>(customer);
> ```
>
> **Lợi ích:**
>
> 1. **Reduce boilerplate**: Không phải viết code mapping thủ công
> 2. **Convention-based**: Tự động map properties cùng tên
> 3. **Custom mapping**: Có thể config custom logic (ForMember)
> 4. **Maintainability**: Thay đổi DTO không cần sửa nhiều chỗ
>
> **Trước khi dùng AutoMapper (manual):**
>
> ```csharp
> var dto = new CustomerDto
> {
>     CustomerId = customer.CustomerId,
>     CustomerCode = customer.CustomerCode,
>     CustomerName = customer.CustomerName,
>     // ... 30 fields nữa
> };
> ```

### Câu 10: **Em giải thích về Async/Await pattern?**

**Trả lời:**

> **Async/Await** cho phép non-blocking I/O operations.
>
> **Synchronous (Blocking):**
>
> ```csharp
> public CustomerDto GetById(int id)
> {
>     var customer = _repository.GetById(id); // Thread bị block
>     return customer;
> }
> ```
>
> - Thread bị block khi chờ database response
> - Giảm throughput (số requests/second xử lý được)
>
> **Asynchronous (Non-Blocking):**
>
> ```csharp
> public async Task<CustomerDto> GetByIdAsync(int id)
> {
>     var customer = await _repository.GetByIdAsync(id); // Thread được giải phóng
>     return customer;
> }
> ```
>
> - Thread được giải phóng khi chờ I/O
> - Thread pool có thể xử lý requests khác
> - Tăng throughput, scalability
>
> **Lợi ích:**
>
> - **Scalability**: Xử lý được nhiều concurrent requests hơn
> - **Responsiveness**: UI không bị freeze
> - **Resource efficiency**: Sử dụng ít threads hơn

---

## 📝 Tóm Tắt Cho Bảo Vệ

**Khi hội đồng hỏi về Module Customers, em cần nhớ:**

1. **Công nghệ**: ASP.NET Core 8, EF Core, PostgreSQL, JWT, AutoMapper
2. **Kiến trúc**: Layered (Controller → Service → Repository → Database)
3. **Patterns**: Repository, DTO, Dependency Injection, Async/Await
4. **Endpoints**: CRUD + Search + Pagination (7 endpoints)
5. **Bảo mật**: JWT authentication, multi-tenant isolation, parameterized queries
6. **Tối ưu**: Indexing, caching, pagination, async I/O
7. **Business logic**: Subscription limit check, segmentation, loyalty points

**Câu trả lời mẫu ngắn gọn:**

> "Module Customers sử dụng ASP.NET Core 8 với kiến trúc Layered Architecture, bao gồm Controller, Service và Repository layers. Em áp dụng Repository Pattern để tách biệt data access, DTO Pattern để tách biệt Entity và API response, và Dependency Injection để loose coupling. Bảo mật được đảm bảo qua JWT authentication và multi-tenant isolation. Tối ưu hiệu năng bằng indexing, pagination, và async/await pattern."

---

**Tài liệu này chuẩn bị đầy đủ để trả lời cả câu hỏi lý thuyết và giải thích luồng nghiệp vụ khi bảo vệ đồ án tốt nghiệp! 🎓**
