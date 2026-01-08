# Tài Liệu API - Module Upload File (Uploads)

## 📋 Mục Lục

1. [Tổng Quan](#tổng-quan)
2. [Công Nghệ & Kiến Trúc](#công-nghệ--kiến-trúc)
3. [API Endpoints](#api-endpoints)
4. [Upload Flow](#upload-flow)
5. [Security & Validation](#security--validation)
6. [Câu Hỏi Bảo Vệ](#câu-hỏi-bảo-vệ)

---

## 1. Tổng Quan

### 1.1. Mục Đích

Module **Uploads** quản lý việc upload file (chủ yếu hình ảnh sản phẩm) lên **Google Cloud Storage (GCS)**:

- Upload hình ảnh sản phẩm
- Generate URL public để truy cập
- Validate file type, size
- Unique file naming để tránh conflict

### 1.2. Use Cases

| Use Case                | Mô Tả                                |
| ----------------------- | ------------------------------------ |
| **Product Images**      | Upload ảnh sản phẩm khi tạo/cập nhật |
| **Profile Avatars**     | Ảnh đại diện nhân viên/khách hàng    |
| **Invoice Attachments** | File đính kèm hóa đơn (PDF, images)  |
| **Category Icons**      | Icon danh mục sản phẩm               |

---

## 2. Công Nghệ & Kiến Trúc

### 2.1. Tech Stack

| Technology                  | Version  | Purpose                      |
| --------------------------- | -------- | ---------------------------- |
| **ASP.NET Core**            | 8.0      | Web API Framework            |
| **Google Cloud Storage**    | V1 API   | Cloud file storage           |
| **Google.Cloud.Storage.V1** | 4.x      | .NET SDK for GCS             |
| **IFormFile**               | Built-in | ASP.NET file upload handling |

### 2.2. Architecture

```
Client (Browser/Mobile)
    ↓ [Multipart/Form-Data]
Controller (UploadsController)
    ↓
Google Cloud Storage SDK
    ↓ [HTTPS]
Google Cloud Storage Bucket
    ↓
Public URL
```

### 2.3. Google Cloud Storage Structure

```
gs://your-bucket-name/
├── products/
│   ├── laptop_a1b2c3d4.jpg
│   ├── phone_e5f6g7h8.png
│   └── ...
├── avatars/
│   ├── user_12345.jpg
│   └── ...
└── invoices/
    ├── invoice_001.pdf
    └── ...
```

---

## 3. API Endpoints

### 3.1. POST /api/Uploads

**Mục đích:** Upload file lên Google Cloud Storage

**Content-Type:** `multipart/form-data`

**Request:**

```http
POST /api/Uploads HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5...
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="file"; filename="laptop.jpg"
Content-Type: image/jpeg

<binary data>
------WebKitFormBoundary7MA4YWxkTrZu0gW--
```

**Response (200 OK):**

```json
{
  "success": true,
  "url": "https://storage.googleapis.com/your-bucket/products/laptop_a1b2c3d4.jpg"
}
```

**Response (400 Bad Request):**

```json
{
  "success": false,
  "message": "No file provided"
}
```

**Response (500 Internal Server Error):**

```json
{
  "success": false,
  "message": "Upload failed",
  "error": "The service is unavailable"
}
```

---

### 3.2. Frontend Upload Example (Angular)

```typescript
// product.service.ts
uploadProductImage(file: File): Observable<any> {
  const formData = new FormData();
  formData.append('file', file);

  return this.http.post<any>(`${this.apiUrl}/api/Uploads`, formData, {
    headers: {
      'Authorization': `Bearer ${this.authService.getToken()}`
    }
  });
}

// product-form.component.ts
onFileSelected(event: any) {
  const file = event.target.files[0];
  if (file) {
    this.productService.uploadProductImage(file).subscribe(
      (res) => {
        if (res.success) {
          this.productForm.patchValue({ imageUrl: res.url });
          console.log('Upload success:', res.url);
        }
      },
      (error) => console.error('Upload failed:', error)
    );
  }
}
```

---

## 4. Upload Flow

### 4.1. Detailed Workflow

```
┌─────────────┐
│   Client    │
│  Select File│
└──────┬──────┘
       │ [multipart/form-data]
       ↓
┌──────────────────────────┐
│   UploadsController      │
│  1. Validate file        │
│  2. Generate unique name │
└──────┬───────────────────┘
       │ [IFormFile]
       ↓
┌──────────────────────────┐
│  Google.Cloud.Storage.V1 │
│  StorageClient.Create()  │
│  UploadObjectAsync()     │
└──────┬───────────────────┘
       │ [HTTPS Upload]
       ↓
┌──────────────────────────┐
│  Google Cloud Storage    │
│  gs://bucket/products/   │
└──────┬───────────────────┘
       │ [Public URL]
       ↓
┌──────────────────────────┐
│  Response to Client      │
│  { success, url }        │
└──────────────────────────┘
```

### 4.2. Code Implementation

```csharp
[HttpPost]
[Consumes("multipart/form-data")]
public async Task<IActionResult> Upload([FromForm] IFormFile file)
{
    // Step 1: Validate file
    if (file == null || file.Length == 0)
        return BadRequest(new { success = false, message = "No file provided" });

    try
    {
        // Step 2: Get GCS config
        var bucketName = _configuration["GoogleCloud:BucketName"];
        var projectId = _configuration["GoogleCloud:ProjectId"];

        // Step 3: Initialize GCS client
        var storage = StorageClient.Create();

        // Step 4: Generate unique file name
        var safeFileName = Path.GetFileNameWithoutExtension(file.FileName);
        var ext = Path.GetExtension(file.FileName);
        var newFileName = $"products/{safeFileName}_{Guid.NewGuid().ToString().Substring(0,8)}{ext}";

        // Step 5: Upload to GCS
        using (var stream = file.OpenReadStream())
        {
            await storage.UploadObjectAsync(
                bucketName,
                newFileName,
                file.ContentType,
                stream
            );
        }

        // Step 6: Generate public URL
        var publicUrl = $"https://storage.googleapis.com/{bucketName}/{newFileName}";

        return Ok(new { success = true, url = publicUrl });
    }
    catch (Exception ex)
    {
        return StatusCode(500, new { success = false, message = "Upload failed", error = ex.Message });
    }
}
```

---

## 5. Security & Validation

### 5.1. File Type Validation

```csharp
private readonly string[] _allowedExtensions = { ".jpg", ".jpeg", ".png", ".gif", ".webp" };
private readonly string[] _allowedMimeTypes = { "image/jpeg", "image/png", "image/gif", "image/webp" };

private bool IsValidFileType(IFormFile file)
{
    var ext = Path.GetExtension(file.FileName).ToLower();

    // Check extension
    if (!_allowedExtensions.Contains(ext))
        return false;

    // Check MIME type
    if (!_allowedMimeTypes.Contains(file.ContentType))
        return false;

    return true;
}
```

**Usage:**

```csharp
if (!IsValidFileType(file))
    return BadRequest(new { success = false, message = "Invalid file type. Only images allowed." });
```

---

### 5.2. File Size Validation

```csharp
private const long MaxFileSize = 5 * 1024 * 1024; // 5MB

private bool IsValidFileSize(IFormFile file)
{
    return file.Length <= MaxFileSize;
}
```

**appsettings.json:**

```json
{
  "FileSizeLimit": {
    "MaxFileSizeMB": 5
  }
}
```

**Usage:**

```csharp
if (!IsValidFileSize(file))
    return BadRequest(new { success = false, message = "File size exceeds 5MB limit." });
```

---

### 5.3. Authentication & Authorization

```csharp
[Authorize] // Require JWT token
[Authorize(Roles = "ShopOwner,Employee")] // Role-based
```

**Middleware Check:**

```csharp
var shopOwnerId = User.FindFirst("ShopOwnerId")?.Value;
if (string.IsNullOrEmpty(shopOwnerId))
    return Unauthorized();
```

---

### 5.4. Rate Limiting

```csharp
// Startup.cs / Program.cs
services.AddRateLimiter(options =>
{
    options.AddFixedWindowLimiter("uploads", opt =>
    {
        opt.Window = TimeSpan.FromMinutes(1);
        opt.PermitLimit = 10; // Max 10 uploads/minute
    });
});

// Controller
[EnableRateLimiting("uploads")]
[HttpPost]
public async Task<IActionResult> Upload([FromForm] IFormFile file)
{
    // ...
}
```

---

## 6. Câu Hỏi Bảo Vệ

### Câu 1: **Tại sao em chọn Google Cloud Storage thay vì lưu file trên server?**

**Trả lời:**

> **Lý do:**
>
> 1. **Scalability**: GCS tự động scale, không lo hết dung lượng
> 2. **Availability**: 99.95% uptime SLA, redundancy across regions
> 3. **CDN Integration**: Serve file nhanh từ edge locations
> 4. **Cost-effective**: Pay-as-you-go, rẻ hơn maintain storage server
> 5. **No Maintenance**: Google quản lý backup, security, updates
>
> **So sánh lưu local:**
> | Tiêu chí | Local Storage | GCS |
> |----------|---------------|-----|
> | Scalability | Limited | Unlimited |
> | Backup | Manual | Automatic |
> | CDN | Manual setup | Built-in |
> | Cost | High (hardware) | Low (pay-per-use) |

### Câu 2: **Giải thích cách generate unique file name?**

**Trả lời:**

> Em dùng công thức:
>
> ```csharp
> var newFileName = $"products/{safeFileName}_{Guid.NewGuid().ToString().Substring(0,8)}{ext}";
> ```
>
> **Ví dụ:**
>
> - Original: `laptop.jpg`
> - Generated: `products/laptop_a1b2c3d4.jpg`
>
> **Các thành phần:**
>
> 1. **Folder prefix** (`products/`): Phân loại files
> 2. **safeFileName**: Tên gốc (stripped extension)
> 3. **GUID suffix** (8 chars): Đảm bảo unique
> 4. **Extension** (`.jpg`): Giữ nguyên file type
>
> **Lý do:**
>
> - Tránh overwrite file cùng tên
> - SEO-friendly (giữ tên gốc)
> - Easy to debug (nhìn tên biết file gốc)

### Câu 3: **IFormFile hoạt động như thế nào?**

**Trả lời:**

> **IFormFile** là interface của ASP.NET Core để handle uploaded files:
>
> **Properties:**
>
> ```csharp
> string FileName       // Tên file gốc
> long Length           // Size (bytes)
> string ContentType    // MIME type (image/jpeg)
> Stream OpenReadStream() // Đọc binary data
> ```
>
> **Workflow:**
>
> 1. Client gửi multipart/form-data
> 2. ASP.NET Core parse form data
> 3. Bind file vào IFormFile parameter
> 4. Developer dùng OpenReadStream() để đọc
>
> **Lưu ý:** IFormFile chỉ available trong request scope, không thể pass ra ngoài method

### Câu 4: **StorageClient.Create() làm gì?**

**Trả lời:**

> **StorageClient.Create()** tạo client để giao tiếp với GCS API:
>
> ```csharp
> var storage = StorageClient.Create();
> ```
>
> **Authentication:**
>
> - Tự động tìm credentials từ:
>   1. **Environment variable**: `GOOGLE_APPLICATION_CREDENTIALS`
>   2. **Service account JSON file**
>   3. **Default credentials** (nếu chạy trên GCP)
>
> **Setup Example:**
>
> ```json
> // appsettings.json
> {
>   "GoogleCloud": {
>     "ProjectId": "my-project-123",
>     "BucketName": "my-app-storage",
>     "CredentialsPath": "path/to/service-account.json"
>   }
> }
> ```
>
> ```csharp
> Environment.SetEnvironmentVariable(
>     "GOOGLE_APPLICATION_CREDENTIALS",
>     _configuration["GoogleCloud:CredentialsPath"]
> );
> var storage = StorageClient.Create();
> ```

### Câu 5: **UploadObjectAsync có các parameters gì?**

**Trả lời:**

> **Method signature:**
>
> ```csharp
> await storage.UploadObjectAsync(
>     bucket: "my-bucket",           // Bucket name
>     objectName: "products/abc.jpg", // File path in bucket
>     contentType: "image/jpeg",      // MIME type
>     source: stream                  // File stream
> );
> ```
>
> **Parameters:**
>
> 1. **bucket**: Tên bucket (configured trong GCS)
> 2. **objectName**: Path + filename trong bucket
> 3. **contentType**: MIME type (để GCS serve đúng header)
> 4. **source**: Stream data (from IFormFile.OpenReadStream())
>
> **Return:** `Google.Apis.Storage.v1.Data.Object` (metadata)

### Câu 6: **Public URL được generate như thế nào?**

**Trả lời:**

> Em dùng format cố định của GCS:
>
> ```csharp
> var publicUrl = $"https://storage.googleapis.com/{bucketName}/{objectName}";
> ```
>
> **Ví dụ:**
>
> - Bucket: `my-app-storage`
> - Object: `products/laptop_a1b2c3d4.jpg`
> - URL: `https://storage.googleapis.com/my-app-storage/products/laptop_a1b2c3d4.jpg`
>
> **Điều kiện:**
>
> - Bucket phải **public** hoặc file có **public ACL**
> - Cấu hình trong GCS Console: "Make public"

### Câu 7: **File size limit xử lý ở đâu?**

**Trả lời:**

> **2 levels:**
>
> **1. ASP.NET Core Kestrel:**
>
> ```csharp
> // Program.cs
> builder.Services.Configure<FormOptions>(options =>
> {
>     options.MultipartBodyLengthLimit = 10 * 1024 * 1024; // 10MB
> });
>
> builder.WebHost.ConfigureKestrel(options =>
> {
>     options.Limits.MaxRequestBodySize = 10 * 1024 * 1024;
> });
> ```
>
> **2. Application Logic:**
>
> ```csharp
> if (file.Length > 5 * 1024 * 1024)
>     return BadRequest("File too large");
> ```
>
> **Recommendation:** Validate ở cả 2 levels để security tốt hơn

### Câu 8: **Xử lý concurrent uploads như thế nào?**

**Trả lời:**

> **Không có vấn đề concurrency** vì:
>
> 1. **Unique file names**: Mỗi upload có GUID unique → không overwrite
> 2. **GCS is thread-safe**: StorageClient handle concurrent requests
> 3. **Stateless**: Không có shared state giữa requests
>
> **Nếu cần track upload progress:**
>
> ```csharp
> var uploadProgress = new Progress<IUploadProgress>(p =>
> {
>     Console.WriteLine($"{p.BytesSent} bytes uploaded");
> });
>
> await storage.UploadObjectAsync(
>     bucket, objectName, contentType, stream,
>     new UploadObjectOptions { ModifySessionRequest = req => req.ProgressChanged += uploadProgress }
> );
> ```

### Câu 9: **Delete file hoạt động như thế nào?**

**Trả lời:**

> Em implement endpoint DELETE:
>
> ```csharp
> [HttpDelete("{fileName}")]
> public async Task<IActionResult> Delete(string fileName)
> {
>     try
>     {
>         var storage = StorageClient.Create();
>         await storage.DeleteObjectAsync(bucketName, fileName);
>
>         return Ok(new { success = true, message = "File deleted" });
>     }
>     catch (Exception ex)
>     {
>         return StatusCode(500, new { success = false, error = ex.Message });
>     }
> }
> ```
>
> **Lưu ý:**
>
> - Validate ownership (chỉ xóa file của shop mình)
> - Check file có đang được dùng không (FK trong products table)

### Câu 10: **Cost optimization cho GCS?**

**Trả lời:**

> **Strategies:**
>
> 1. **Storage Classes:**
>
>    - **Standard**: Hot data (frequently accessed)
>    - **Nearline**: 30 days (backups)
>    - **Coldline**: 90 days (archives)
>    - **Archive**: Long-term storage
>
> 2. **Lifecycle Policies:**
>
>    ```json
>    {
>      "rule": [
>        {
>          "action": { "type": "SetStorageClass", "storageClass": "NEARLINE" },
>          "condition": { "age": 30 }
>        }
>      ]
>    }
>    ```
>
> 3. **Compression:**
>
>    - Gzip files before upload
>    - GCS auto-decompress on serve
>
> 4. **CDN Caching:**
>    - Set Cache-Control headers
>    - Reduce egress costs

---

## 📝 Tóm Tắt

**Key Points:**

1. **GCS**: Scalable, reliable, cost-effective cloud storage
2. **IFormFile**: ASP.NET Core interface for file uploads
3. **StorageClient**: Google SDK for GCS operations
4. **Unique Naming**: `{prefix}/{name}_{guid}{ext}`
5. **Security**: File type/size validation, JWT auth, rate limiting
6. **Public URL**: `https://storage.googleapis.com/{bucket}/{object}`
7. **Concurrency**: No issues with unique file names
8. **Cost Optimization**: Storage classes, lifecycle policies, CDN

---

**Tài liệu sẵn sàng cho bảo vệ module Uploads! 🎓**
