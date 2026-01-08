# Tài Liệu Lý Thuyết Công Nghệ - Hệ Thống Quản Lý Bán Hàng

## 📋 Mục Lục
1. [Backend Technologies](#1-backend-technologies)
2. [Frontend Technologies](#2-frontend-technologies)
3. [Database & Data Management](#3-database--data-management)
4. [Architecture Patterns](#4-architecture-patterns)
5. [Security & Authentication](#5-security--authentication)
6. [Cloud Services](#6-cloud-services)
7. [API Design](#7-api-design)
8. [Design Patterns & Best Practices](#8-design-patterns--best-practices)
9. [Performance Optimization](#9-performance-optimization)
10. [DevOps & Deployment](#10-devops--deployment)
11. [Testing](#11-testing)
12. [Câu Hỏi Lý Thuyết Bảo Vệ](#12-câu-hỏi-lý-thuyết-bảo-vệ)

---

## 1. Backend Technologies

### 1.1. C# (C Sharp)

**Định nghĩa:**
- Ngôn ngữ lập trình hướng đối tượng (OOP) của Microsoft
- Strongly-typed, managed language chạy trên .NET Runtime

**Đặc điểm chính:**
- **Type Safety**: Compile-time type checking
- **Garbage Collection**: Tự động quản lý memory
- **LINQ**: Language Integrated Query
- **Async/Await**: Lập trình bất đồng bộ
- **Generics**: Type parameters `List<T>`

**Tại sao chọn C#:**
- ✅ Cross-platform với .NET Core/8
- ✅ Hiệu năng cao
- ✅ Ecosystem phong phú (NuGet packages)
- ✅ Tooling tốt (Visual Studio, Rider)
- ✅ Cộng đồng lớn

---

### 1.2. ASP.NET Core 8

**Định nghĩa:**
- Framework để xây dựng web applications và APIs
- Cross-platform (Windows, Linux, macOS)
- Open-source

**Kiến trúc:**
```
HTTP Request → Middleware Pipeline → Routing → Controller → Action → Response
```

**Core Concepts:**

#### a) **Middleware Pipeline**
```csharp
app.UseAuthentication();  // Verify JWT token
app.UseAuthorization();   // Check permissions
app.MapControllers();     // Route to controllers
```

- Mỗi middleware xử lý request và pass sang middleware tiếp theo
- Có thể short-circuit pipeline (return early)

#### b) **Dependency Injection (DI)**
```csharp
// Program.cs
builder.Services.AddScoped<ICustomerService, CustomerService>();

// Controller
public CustomersController(ICustomerService service)
{
    _service = service;
}
```

- **Inversion of Control**: Framework quản lý object lifetime
- **3 Lifetimes**:
  - `Singleton`: 1 instance cho toàn app
  - `Scoped`: 1 instance per request
  - `Transient`: Mỗi lần inject tạo instance mới

#### c) **Routing**
```csharp
[Route("api/[controller]")]
[HttpGet("{id}")]
public async Task<IActionResult> GetById(int id)
```

- **Attribute Routing**: Define routes trên controller/action
- **Route Parameters**: `{id}`, `{shopId:int}`
- **Route Constraints**: `:int`, `:guid`, `:regex()`

#### d) **Model Binding**
- Tự động bind data từ request vào parameters
- Sources: URL, Query string, Headers, Body

---

### 1.3. Entity Framework Core 8

**Định nghĩa:**
- Object-Relational Mapper (ORM)
- Map C# objects ↔ database tables

**Core Concepts:**

#### a) **DbContext**
```csharp
public class ApplicationDbContext : DbContext
{
    public DbSet<Customer> Customers { get; set; }
    public DbSet<Product> Products { get; set; }
}
```

- Entry point to database
- Manages connections, transactions, change tracking

#### b) **Migrations**
```bash
dotnet ef migrations add CreateCustomersTable
dotnet ef database update
```

- **Code-First**: Define models → Generate database schema
- Version control cho database changes

#### c) **LINQ Queries**
```csharp
var customers = await _context.Customers
    .Where(c => c.ShopOwnerId == shopId)
    .Include(c => c.Orders)
    .OrderBy(c => c.CustomerName)
    .ToListAsync();
```

- **Deferred Execution**: Query chỉ execute khi cần
- **Expression Trees**: LINQ → SQL translation

#### d) **Change Tracking**
```csharp
var customer = await _context.Customers.FindAsync(id);
customer.Email = "new@email.com";  // Tracked
await _context.SaveChangesAsync(); // UPDATE SQL executed
```

- Tự động detect changes
- Generate SQL UPDATE/INSERT/DELETE

#### e) **Eager Loading vs Lazy Loading**
```csharp
// Eager: Load related data immediately
.Include(c => c.Orders)

// Lazy: Load on access (requires proxy)
customer.Orders.ToList();  // Triggers query
```

---

### 1.4. PostgreSQL

**Định nghĩa:**
- Relational Database Management System (RDBMS)
- Open-source, ACID-compliant

**Tại sao chọn PostgreSQL:**
- ✅ Advanced features (JSON, Full-Text Search, Arrays)
- ✅ ACID transactions
- ✅ Scalability
- ✅ Free & open-source
- ✅ Cross-platform

**Key Features:**

#### a) **ACID Properties**
- **Atomicity**: All or nothing (transaction)
- **Consistency**: Data integrity constraints
- **Isolation**: Concurrent transactions không conflict
- **Durability**: Committed data persists

#### b) **Indexes**
```sql
CREATE INDEX idx_customers_email ON customers(email);
CREATE INDEX idx_orders_date ON orders(order_date DESC);
```

- Speed up queries
- B-Tree (default), Hash, GiST, GIN

#### c) **Constraints**
```sql
PRIMARY KEY, FOREIGN KEY, UNIQUE, NOT NULL, CHECK
```

#### d) **Transactions**
```sql
BEGIN;
  UPDATE accounts SET balance = balance - 100 WHERE id = 1;
  UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```

---

## 2. Frontend Technologies

### 2.1. Angular 19

**Định nghĩa:**
- TypeScript-based web framework
- Component-based architecture
- Full-featured (routing, forms, HTTP client)

**Core Concepts:**

#### a) **Components**
```typescript
@Component({
  selector: 'app-customer-list',
  templateUrl: './customer-list.component.html',
  styleUrls: ['./customer-list.component.css']
})
export class CustomerListComponent implements OnInit {
  customers: Customer[] = [];
  
  ngOnInit() {
    this.loadCustomers();
  }
}
```

- **Building blocks** của UI
- **Encapsulation**: HTML + CSS + Logic

#### b) **Services**
```typescript
@Injectable({ providedIn: 'root' })
export class CustomerService {
  constructor(private http: HttpClient) {}
  
  getCustomers(): Observable<Customer[]> {
    return this.http.get<Customer[]>(`${this.apiUrl}/customers`);
  }
}
```

- **Singleton** business logic
- **Reusable** across components
- **Dependency Injection**

#### c) **Observables (RxJS)**
```typescript
this.customerService.getCustomers()
  .pipe(
    map(customers => customers.filter(c => c.active)),
    catchError(error => of([]))
  )
  .subscribe(customers => this.customers = customers);
```

- **Asynchronous data streams**
- **Operators**: map, filter, debounceTime, switchMap

#### d) **Routing**
```typescript
const routes: Routes = [
  { path: 'customers', component: CustomerListComponent },
  { path: 'customers/:id', component: CustomerDetailComponent }
];
```

- **Single Page Application (SPA)**
- Client-side navigation

#### e) **Forms**
```typescript
// Reactive Forms
this.customerForm = this.fb.group({
  customerName: ['', Validators.required],
  email: ['', [Validators.required, Validators.email]],
  phone: ['', Validators.pattern(/^\d{10}$/)]
});
```

- **Template-driven** vs **Reactive**
- Built-in validators

---

### 2.2. TypeScript

**Định nghĩa:**
- Superset của JavaScript
- Adds static typing
- Compiles to JavaScript

**Key Features:**
```typescript
// Types
let name: string = "John";
let age: number = 30;
let isActive: boolean = true;

// Interfaces
interface Customer {
  customerId: number;
  customerName: string;
  email?: string;  // Optional
}

// Generics
function getById<T>(id: number): T {
  // ...
}

// Classes
class CustomerService {
  private apiUrl: string;
  
  constructor(apiUrl: string) {
    this.apiUrl = apiUrl;
  }
}
```

**Lợi ích:**
- ✅ Early error detection (compile-time)
- ✅ Better IDE support (IntelliSense)
- ✅ Refactoring safety
- ✅ Self-documenting code

---

### 2.3. Tailwind CSS

**Định nghĩa:**
- Utility-first CSS framework
- Pre-built classes: `flex`, `p-4`, `bg-blue-500`

**Example:**
```html
<div class="flex items-center justify-between p-4 bg-white rounded-lg shadow-md">
  <h2 class="text-xl font-bold text-gray-800">Customer List</h2>
  <button class="px-4 py-2 text-white bg-blue-500 rounded hover:bg-blue-600">
    Add Customer
  </button>
</div>
```

**Ưu điểm:**
- ✅ Rapid development
- ✅ Consistent design
- ✅ No CSS naming conflicts
- ✅ Tree-shaking (remove unused CSS)

---

## 3. Database & Data Management

### 3.1. Relational Database Concepts

#### a) **Normalization**
- **1NF**: Atomic values (không có arrays)
- **2NF**: No partial dependencies
- **3NF**: No transitive dependencies

**Ví dụ:**
```sql
-- Before (denormalized)
orders: order_id, customer_name, customer_email, product_name

-- After (normalized)
customers: customer_id, customer_name, email
products: product_id, product_name
orders: order_id, customer_id, product_id
```

#### b) **Relationships**
- **One-to-Many**: Customer → Orders
- **Many-to-Many**: Products ↔ Shops (via ProductShop)
- **One-to-One**: User → Profile

#### c) **Foreign Keys**
```sql
FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
  ON DELETE CASCADE
  ON UPDATE CASCADE
```

#### d) **Indexes**
```sql
-- B-Tree (default)
CREATE INDEX idx_email ON customers(email);

-- Composite index
CREATE INDEX idx_shop_product ON product_shop(shop_id, product_id);

-- Partial index
CREATE INDEX idx_active_customers ON customers(email) WHERE status = 'active';
```

**Trade-offs:**
- ✅ Faster SELECT
- ❌ Slower INSERT/UPDATE/DELETE
- ❌ More storage

---

### 3.2. Transactions

**ACID Properties:**

```sql
BEGIN TRANSACTION;

  -- Deduct stock
  UPDATE products SET stock = stock - 5 WHERE product_id = 1;
  
  -- Create order
  INSERT INTO orders (customer_id, total) VALUES (123, 500000);

COMMIT;  -- Success
ROLLBACK; -- On error
```

**Isolation Levels:**
- **Read Uncommitted**: Dirty reads possible
- **Read Committed**: Default in PostgreSQL
- **Repeatable Read**: Prevents non-repeatable reads
- **Serializable**: Full isolation

---

### 3.3. Stored Procedures & Functions

```sql
CREATE FUNCTION calculate_customer_lifetime_value(customer_id INT)
RETURNS DECIMAL(18,2)
AS $$
BEGIN
  RETURN (
    SELECT COALESCE(SUM(total), 0)
    FROM orders
    WHERE customer_id = $1 AND status = 'completed'
  );
END;
$$ LANGUAGE plpgsql;
```

**Lợi ích:**
- Reusable logic
- Better performance (compiled)
- Security (limited access)

---

## 4. Architecture Patterns

### 4.1. Layered Architecture (N-Tier)

```
┌──────────────────────────┐
│  Presentation Layer      │ ← Controllers, DTOs
├──────────────────────────┤
│  Business Logic Layer    │ ← Services
├──────────────────────────┤
│  Data Access Layer       │ ← Repositories
├──────────────────────────┤
│  Database                │ ← PostgreSQL
└──────────────────────────┘
```

**Nguyên tắc:**
- **Separation of Concerns**: Mỗi layer một trách nhiệm
- **Dependency Flow**: Top → Down
- **Loose Coupling**: Interfaces giữa layers

---

### 4.2. Repository Pattern

**Định nghĩa:** Abstraction layer cho data access

```csharp
public interface ICustomerRepository
{
    Task<Customer?> GetByIdAsync(int id);
    Task<IEnumerable<Customer>> GetAllAsync();
    Task<Customer> AddAsync(Customer customer);
    Task UpdateAsync(Customer customer);
    Task DeleteAsync(int id);
}

public class CustomerRepository : ICustomerRepository
{
    private readonly ApplicationDbContext _context;
    
    public async Task<Customer?> GetByIdAsync(int id)
    {
        return await _context.Customers.FindAsync(id);
    }
}
```

**Lợi ích:**
- ✅ Testability (mock repositories)
- ✅ Centralized data access logic
- ✅ Easier to switch data sources

---

### 4.3. Service Layer Pattern

```csharp
public interface ICustomerService
{
    Task<CustomerDto> GetByIdAsync(int id);
    Task<CustomerDto> CreateAsync(CreateCustomerDto dto);
}

public class CustomerService : ICustomerService
{
    private readonly ICustomerRepository _repo;
    private readonly IMapper _mapper;
    
    public async Task<CustomerDto> CreateAsync(CreateCustomerDto dto)
    {
        // Business logic
        var customer = _mapper.Map<Customer>(dto);
        await _repo.AddAsync(customer);
        return _mapper.Map<CustomerDto>(customer);
    }
}
```

**Trách nhiệm:**
- Business logic
- Validation
- Orchestrate repositories
- DTO mapping

---

### 4.4. DTO Pattern (Data Transfer Object)

**Định nghĩa:** Objects để transfer data giữa layers

```csharp
// Entity (Database)
public class Customer
{
    public int CustomerId { get; set; }
    public string CustomerName { get; set; }
    public string Password { get; set; }  // Sensitive!
}

// DTO (API Response)
public class CustomerDto
{
    public int CustomerId { get; set; }
    public string CustomerName { get; set; }
    // NO Password!
}
```

**Lý do:**
- ✅ Security (hide sensitive fields)
- ✅ API versioning (decouple from DB schema)
- ✅ Validation (Data Annotations)
- ✅ Performance (select only needed fields)

---

### 4.5. Dependency Injection (DI)

**Định nghĩa:** IoC pattern để inject dependencies

```csharp
// Registration
services.AddScoped<ICustomerService, CustomerService>();
services.AddScoped<ICustomerRepository, CustomerRepository>();

// Injection
public class CustomersController : ControllerBase
{
    private readonly ICustomerService _service;
    
    public CustomersController(ICustomerService service)
    {
        _service = service;  // Injected by framework
    }
}
```

**Lợi ích:**
- ✅ Loose coupling
- ✅ Testability
- ✅ Centralized configuration

---

## 5. Security & Authentication

### 5.1. JWT (JSON Web Token)

**Định nghĩa:** Token-based authentication standard

**Structure:**
```
Header.Payload.Signature
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

**Parts:**
1. **Header**: Algorithm + Token type
2. **Payload**: Claims (user data)
3. **Signature**: Verify integrity

**Flow:**
```
1. Client → POST /login (username, password)
2. Server validates credentials
3. Server generates JWT
4. Client stores token (localStorage/cookie)
5. Client → GET /api/customers (Authorization: Bearer <token>)
6. Server validates token
7. Server returns data
```

**Claims:**
```json
{
  "sub": "12345",           // User ID
  "name": "John Doe",       // User name
  "role": "ShopOwner",      // Role
  "shopOwnerId": "100",     // Custom claim
  "exp": 1736332800,        // Expiration
  "iat": 1736246400         // Issued at
}
```

**Implementation:**
```csharp
// Generate
var tokenHandler = new JwtSecurityTokenHandler();
var key = Encoding.ASCII.GetBytes(_config["JwtSettings:Secret"]);
var tokenDescriptor = new SecurityTokenDescriptor
{
    Subject = new ClaimsIdentity(new[]
    {
        new Claim(ClaimTypes.NameIdentifier, user.UserId.ToString()),
        new Claim(ClaimTypes.Role, user.Role)
    }),
    Expires = DateTime.UtcNow.AddHours(24),
    SigningCredentials = new SigningCredentials(
        new SymmetricSecurityKey(key), 
        SecurityAlgorithms.HmacSha256Signature
    )
};
var token = tokenHandler.CreateToken(tokenDescriptor);
return tokenHandler.WriteToken(token);

// Validate
services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(options =>
    {
        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuerSigningKey = true,
            IssuerSigningKey = new SymmetricSecurityKey(key),
            ValidateIssuer = false,
            ValidateAudience = false
        };
    });
```

---

### 5.2. Password Hashing (Bcrypt)

**Định nghĩa:** One-way encryption for passwords

```csharp
// Hash password
string hashedPassword = BCrypt.Net.BCrypt.HashPassword(plainPassword);

// Verify
bool isValid = BCrypt.Net.BCrypt.Verify(plainPassword, hashedPassword);
```

**Features:**
- **Salt**: Random data added to password
- **Work Factor**: CPU cost (higher = slower)
- **One-way**: Cannot decrypt

**Tại sao không MD5/SHA1:**
- MD5/SHA1 quá nhanh → Brute-force dễ
- Bcrypt slow by design
- Built-in salt

---

### 5.3. Authorization

```csharp
// Role-based
[Authorize(Roles = "ShopOwner,Admin")]
public async Task<IActionResult> GetCustomers()

// Policy-based
[Authorize(Policy = "MustOwnShop")]
public async Task<IActionResult> GetCustomers(int shopId)

// Implementation
services.AddAuthorization(options =>
{
    options.AddPolicy("MustOwnShop", policy =>
        policy.Requirements.Add(new ShopOwnerRequirement()));
});
```

---

### 5.4. CORS (Cross-Origin Resource Sharing)

**Vấn đề:** Browser block requests từ origin khác

**Giải pháp:**
```csharp
services.AddCors(options =>
{
    options.AddPolicy("AllowAngular", builder =>
    {
        builder.WithOrigins("http://localhost:4200")
               .AllowAnyMethod()
               .AllowAnyHeader()
               .AllowCredentials();
    });
});

app.UseCors("AllowAngular");
```

---

### 5.5. SQL Injection Prevention

**Bad:**
```csharp
var query = $"SELECT * FROM users WHERE username = '{username}'";
// username = "admin' OR '1'='1" → SQL injection!
```

**Good:**
```csharp
// EF Core (parameterized)
var user = await _context.Users
    .FirstOrDefaultAsync(u => u.Username == username);

// Raw SQL with parameters
var user = await _context.Users
    .FromSqlRaw("SELECT * FROM users WHERE username = {0}", username)
    .FirstOrDefaultAsync();
```

---

## 6. Cloud Services

### 6.1. Google Cloud Storage (GCS)

**Định nghĩa:** Object storage service

**Use Cases:**
- Store product images
- Backup files
- Static assets

**Key Concepts:**

#### a) **Buckets**
- Top-level containers
- Globally unique names
- Access control (public/private)

#### b) **Objects**
- Files stored in buckets
- Identified by keys: `products/laptop_123.jpg`

#### c) **Storage Classes**
- **Standard**: Frequent access
- **Nearline**: Monthly access (backups)
- **Coldline**: Quarterly access (archives)
- **Archive**: Yearly access (compliance)

#### d) **Authentication**
```csharp
// Service Account JSON
Environment.SetEnvironmentVariable(
    "GOOGLE_APPLICATION_CREDENTIALS",
    "path/to/service-account.json"
);

var storage = StorageClient.Create();
```

#### e) **Upload**
```csharp
await storage.UploadObjectAsync(
    bucket: "my-bucket",
    objectName: "products/image.jpg",
    contentType: "image/jpeg",
    source: fileStream
);
```

**Pricing:**
- Storage: $0.020/GB/month
- Network egress: $0.12/GB
- Operations: $0.05/10k requests

---

## 7. API Design

### 7.1. REST API

**Định nghĩa:** REpresentational State Transfer

**Principles:**
1. **Stateless**: No session on server
2. **Resource-based**: URL = Resource
3. **HTTP Methods**: GET, POST, PUT, DELETE
4. **HTTP Status Codes**: 200, 201, 400, 404, 500

**Best Practices:**

#### a) **URL Design**
```
✅ Good:
GET    /api/customers
GET    /api/customers/123
POST   /api/customers
PUT    /api/customers/123
DELETE /api/customers/123

❌ Bad:
GET    /api/getCustomers
POST   /api/createCustomer
GET    /api/customer?id=123
```

#### b) **Status Codes**
| Code | Meaning | Use Case |
|------|---------|----------|
| 200 | OK | Successful GET, PUT |
| 201 | Created | Successful POST |
| 204 | No Content | Successful DELETE |
| 400 | Bad Request | Validation error |
| 401 | Unauthorized | No/invalid token |
| 403 | Forbidden | Valid token, no permission |
| 404 | Not Found | Resource not exists |
| 500 | Internal Server Error | Server error |

#### c) **Pagination**
```
GET /api/customers?page=2&pageSize=20

Response:
{
  "data": [...],
  "totalCount": 150,
  "page": 2,
  "pageSize": 20,
  "totalPages": 8
}
```

#### d) **Filtering & Sorting**
```
GET /api/customers?status=active&sortBy=customerName&sortOrder=asc
```

#### e) **Versioning**
```
/api/v1/customers
/api/v2/customers
```

---

### 7.2. Swagger/OpenAPI

**Định nghĩa:** API documentation standard

```csharp
// Setup
services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1", new OpenApiInfo 
    { 
        Title = "Shop Management API", 
        Version = "v1" 
    });
});

app.UseSwagger();
app.UseSwaggerUI();
```

**Attributes:**
```csharp
/// <summary>
/// Get customer by ID
/// </summary>
/// <param name="id">Customer ID</param>
/// <returns>Customer details</returns>
[HttpGet("{id}")]
[ProducesResponseType(typeof(CustomerDto), 200)]
[ProducesResponseType(404)]
public async Task<IActionResult> GetById(int id)
```

**URL:** `https://localhost:5001/swagger`

---

## 8. Design Patterns & Best Practices

### 8.1. Async/Await

**Định nghĩa:** Asynchronous programming pattern

```csharp
public async Task<Customer> GetCustomerAsync(int id)
{
    // Non-blocking I/O
    return await _context.Customers.FindAsync(id);
}
```

**Lợi ích:**
- ✅ Better scalability (free threads during I/O)
- ✅ Responsive UI (Angular)
- ✅ Handle more concurrent requests

**Rule:** Async all the way (controller → service → repository)

---

### 8.2. AutoMapper

**Định nghĩa:** Object-to-object mapping library

```csharp
// Profile
public class CustomerProfile : Profile
{
    public CustomerProfile()
    {
        CreateMap<Customer, CustomerDto>();
        CreateMap<CreateCustomerDto, Customer>();
    }
}

// Usage
var customerDto = _mapper.Map<CustomerDto>(customer);
```

**Lợi ích:**
- ✅ Reduce boilerplate code
- ✅ Centralized mapping logic
- ✅ Convention-based mapping

---

### 8.3. Exception Handling

```csharp
// Global exception handler
public class GlobalExceptionMiddleware
{
    public async Task InvokeAsync(HttpContext context, RequestDelegate next)
    {
        try
        {
            await next(context);
        }
        catch (NotFoundException ex)
        {
            context.Response.StatusCode = 404;
            await context.Response.WriteAsJsonAsync(new { error = ex.Message });
        }
        catch (Exception ex)
        {
            context.Response.StatusCode = 500;
            await context.Response.WriteAsJsonAsync(new { error = "Internal server error" });
        }
    }
}
```

---

### 8.4. Logging

```csharp
public class CustomerService
{
    private readonly ILogger<CustomerService> _logger;
    
    public async Task<Customer> GetByIdAsync(int id)
    {
        _logger.LogInformation("Getting customer {CustomerId}", id);
        
        try
        {
            return await _repo.GetByIdAsync(id);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Error getting customer {CustomerId}", id);
            throw;
        }
    }
}
```

**Levels:** Trace, Debug, Information, Warning, Error, Critical

---

## 9. Performance Optimization

### 9.1. Database Indexing

```sql
-- Single column
CREATE INDEX idx_customers_email ON customers(email);

-- Composite
CREATE INDEX idx_orders_customer_date ON orders(customer_id, order_date DESC);

-- Covering index
CREATE INDEX idx_customers_name_email ON customers(customer_name, email);
```

**Khi nào cần index:**
- ✅ Foreign keys
- ✅ WHERE clauses
- ✅ ORDER BY columns
- ✅ JOIN columns

---

### 9.2. Caching

```csharp
// In-memory cache
services.AddMemoryCache();

public class CustomerService
{
    private readonly IMemoryCache _cache;
    
    public async Task<Customer> GetByIdAsync(int id)
    {
        var cacheKey = $"customer_{id}";
        
        if (_cache.TryGetValue(cacheKey, out Customer customer))
            return customer;
        
        customer = await _repo.GetByIdAsync(id);
        
        _cache.Set(cacheKey, customer, TimeSpan.FromMinutes(10));
        
        return customer;
    }
}
```

**Cache Strategies:**
- **Cache-Aside**: App checks cache first
- **Write-Through**: Write to cache + DB
- **Write-Behind**: Write to cache, async to DB

---

### 9.3. Pagination

```csharp
public async Task<PagedResult<CustomerDto>> GetPagedAsync(int page, int pageSize)
{
    var totalCount = await _context.Customers.CountAsync();
    
    var customers = await _context.Customers
        .Skip((page - 1) * pageSize)
        .Take(pageSize)
        .ToListAsync();
    
    return new PagedResult<CustomerDto>
    {
        Data = _mapper.Map<List<CustomerDto>>(customers),
        TotalCount = totalCount,
        Page = page,
        PageSize = pageSize
    };
}
```

---

### 9.4. Select Only Needed Columns

```csharp
// Bad: SELECT *
var customers = await _context.Customers.ToListAsync();

// Good: SELECT id, name, email
var customers = await _context.Customers
    .Select(c => new CustomerDto
    {
        CustomerId = c.CustomerId,
        CustomerName = c.CustomerName,
        Email = c.Email
    })
    .ToListAsync();
```

---

### 9.5. Asynchronous Processing

```csharp
// Fire-and-forget background job
public async Task<IActionResult> CreateOrder(CreateOrderDto dto)
{
    var order = await _orderService.CreateAsync(dto);
    
    // Send email asynchronously (don't wait)
    _ = Task.Run(async () => 
    {
        await _emailService.SendOrderConfirmationAsync(order);
    });
    
    return CreatedAtAction(nameof(GetById), new { id = order.OrderId }, order);
}
```

---

## 10. DevOps & Deployment

### 10.1. Docker

**Định nghĩa:** Containerization platform

**Dockerfile:**
```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["Backend.csproj", "./"]
RUN dotnet restore
COPY . .
RUN dotnet build -c Release -o /app/build

FROM build AS publish
RUN dotnet publish -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Backend.dll"]
```

**Build & Run:**
```bash
docker build -t shop-api .
docker run -p 5000:80 shop-api
```

---

### 10.2. Environment Variables

```csharp
// appsettings.json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=shopdb;..."
  }
}

// appsettings.Production.json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=prod-server;Database=shopdb;..."
  }
}

// Read
var connectionString = _configuration.GetConnectionString("DefaultConnection");
```

---

### 10.3. CI/CD

**Continuous Integration/Continuous Deployment**

```yaml
# Example: GitHub Actions
name: Build and Deploy

on:
  push:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Setup .NET
        uses: actions/setup-dotnet@v1
        with:
          dotnet-version: 8.0.x
      - name: Restore dependencies
        run: dotnet restore
      - name: Build
        run: dotnet build --no-restore
      - name: Test
        run: dotnet test --no-build
      - name: Publish
        run: dotnet publish -c Release -o ./publish
```

---

## 11. Testing

### 11.1. Unit Testing

```csharp
[Fact]
public async Task GetById_ReturnsCustomer_WhenExists()
{
    // Arrange
    var mockRepo = new Mock<ICustomerRepository>();
    mockRepo.Setup(r => r.GetByIdAsync(1))
            .ReturnsAsync(new Customer { CustomerId = 1, CustomerName = "John" });
    
    var service = new CustomerService(mockRepo.Object);
    
    // Act
    var result = await service.GetByIdAsync(1);
    
    // Assert
    Assert.NotNull(result);
    Assert.Equal("John", result.CustomerName);
}
```

**Framework:** xUnit, NUnit, MSTest

---

### 11.2. Integration Testing

```csharp
public class CustomersControllerTests : IClassFixture<WebApplicationFactory<Program>>
{
    private readonly HttpClient _client;
    
    public CustomersControllerTests(WebApplicationFactory<Program> factory)
    {
        _client = factory.CreateClient();
    }
    
    [Fact]
    public async Task GetCustomers_ReturnsOkWithData()
    {
        var response = await _client.GetAsync("/api/customers");
        
        response.EnsureSuccessStatusCode();
        var customers = await response.Content.ReadFromJsonAsync<List<CustomerDto>>();
        Assert.NotEmpty(customers);
    }
}
```

---

## 12. Câu Hỏi Lý Thuyết Bảo Vệ

### Câu 1: **Tại sao em chọn ASP.NET Core thay vì Node.js/Java?**

**Trả lời:**
> Em chọn **ASP.NET Core** vì:
> 1. **Performance**: Benchmarks cho thấy ASP.NET Core nhanh hơn Node.js (TechEmpower)
> 2. **Type Safety**: C# strongly-typed giúp catch errors sớm
> 3. **Ecosystem**: NuGet packages phong phú (EF Core, AutoMapper, JWT)
> 4. **Tooling**: Visual Studio/Rider có IntelliSense tốt
> 5. **Cross-platform**: Chạy trên Windows/Linux/macOS
> 6. **Free & Open-source**: Không có licensing cost
> 
> **So với Node.js:** Node.js tốt cho real-time apps, nhưng ASP.NET Core có type safety tốt hơn cho business apps
> 
> **So với Java Spring:** Tương đương về features, nhưng ASP.NET Core có cú pháp ngắn gọn hơn và built-in DI

---

### Câu 2: **Giải thích Dependency Injection hoạt động như thế nào?**

**Trả lời:**
> **Dependency Injection** là design pattern để achieve **Inversion of Control**:
> 
> **Truyền thống:**
> ```csharp
> public class CustomerService
> {
>     private CustomerRepository _repo = new CustomerRepository(); // Tight coupling
> }
> ```
> 
> **Với DI:**
> ```csharp
> public class CustomerService
> {
>     private readonly ICustomerRepository _repo;
>     
>     public CustomerService(ICustomerRepository repo) // Injected
>     {
>         _repo = repo;
>     }
> }
> ```
> 
> **Workflow:**
> 1. Register: `services.AddScoped<ICustomerRepository, CustomerRepository>()`
> 2. Framework creates **DI Container**
> 3. When `CustomerService` needed → Container resolves dependencies
> 4. Container injects `CustomerRepository` instance
> 
> **3 Lifetimes:**
> - `Singleton`: 1 instance toàn app
> - `Scoped`: 1 instance per HTTP request
> - `Transient`: New instance mỗi lần inject

---

### Câu 3: **ORM là gì? Lợi ích của Entity Framework Core?**

**Trả lời:**
> **ORM (Object-Relational Mapper)** map C# objects ↔ database tables:
> 
> **Without ORM:**
> ```csharp
> var command = new SqlCommand("SELECT * FROM customers WHERE id = @id");
> command.Parameters.AddWithValue("@id", id);
> var reader = command.ExecuteReader();
> // Manual mapping...
> ```
> 
> **With EF Core:**
> ```csharp
> var customer = await _context.Customers.FindAsync(id);
> ```
> 
> **Lợi ích:**
> 1. **Productivity**: Ít code hơn
> 2. **Type Safety**: Compile-time checks
> 3. **SQL Injection Prevention**: Parameterized queries
> 4. **Database Agnostic**: Dễ switch PostgreSQL → SQL Server
> 5. **Migrations**: Version control DB schema
> 6. **Change Tracking**: Tự động generate UPDATE/DELETE SQL
> 
> **Trade-off:** Performance overhead (so với raw SQL), nhưng chấp nhận được cho hầu hết apps

---

### Câu 4: **JWT hoạt động như thế nào? Tại sao không dùng Session?**

**Trả lời:**
> **JWT (JSON Web Token):**
> 
> **Flow:**
> 1. Client login → Server validate credentials
> 2. Server generate JWT (chứa user info)
> 3. Client lưu token (localStorage/cookie)
> 4. Mỗi request, client gửi token trong `Authorization: Bearer <token>`
> 5. Server verify signature → Extract user info
> 
> **Structure:** `Header.Payload.Signature`
> - Header: Algorithm (HS256)
> - Payload: Claims (userId, role, exp)
> - Signature: HMAC(Header + Payload, secret)
> 
> **JWT vs Session:**
> | Tiêu chí | JWT | Session |
> |----------|-----|---------|
> | Stateless | ✅ Yes | ❌ No (server stores session) |
> | Scalability | ✅ Easy (no shared session store) | ❌ Needs Redis/sticky sessions |
> | Revocation | ❌ Hard (blacklist needed) | ✅ Easy (delete session) |
> | Size | ❌ Large (sent every request) | ✅ Small (only session ID) |
> 
> **Khi nào dùng JWT:** Microservices, mobile apps, stateless APIs
> 
> **Khi nào dùng Session:** Monolith apps, cần revoke tokens ngay lập tức

---

### Câu 5: **Async/Await hoạt động như thế nào?**

**Trả lời:**
> **Async/Await** cho phép lập trình bất đồng bộ:
> 
> **Without async:**
> ```csharp
> public Customer GetCustomer(int id)
> {
>     var customer = _context.Customers.Find(id); // Thread blocks here (I/O wait)
>     return customer;
> }
> ```
> 
> **With async:**
> ```csharp
> public async Task<Customer> GetCustomerAsync(int id)
> {
>     var customer = await _context.Customers.FindAsync(id); // Thread freed during I/O
>     return customer;
> }
> ```
> 
> **Workflow:**
> 1. Method hits `await` → Thread released back to thread pool
> 2. I/O operation continues (database query)
> 3. When I/O completes → Thread pool picks a thread to continue
> 4. Execution resumes after `await`
> 
> **Lợi ích:**
> - ✅ **Scalability**: Handle more concurrent requests (ít threads hơn)
> - ✅ **Responsive UI**: Không block UI thread (Angular)
> - ✅ **Better resource utilization**
> 
> **Rule:** "Async all the way" (controller → service → repository)

---

### Câu 6: **ACID properties của transactions là gì?**

**Trả lời:**
> **ACID** đảm bảo data integrity trong transactions:
> 
> **A - Atomicity (Nguyên tử):**
> - Tất cả operations trong transaction thành công HOẶC tất cả fail
> - Ví dụ: Chuyển tiền → Trừ A và Cộng B phải cùng xảy ra
> 
> **C - Consistency (Nhất quán):**
> - Database luôn ở trạng thái hợp lệ (constraints không bị vi phạm)
> - Ví dụ: Foreign key constraints, check constraints
> 
> **I - Isolation (Cô lập):**
> - Concurrent transactions không ảnh hưởng lẫn nhau
> - Levels: Read Uncommitted, Read Committed, Repeatable Read, Serializable
> 
> **D - Durability (Bền vững):**
> - Committed data không bị mất (persist to disk)
> - Ví dụ: Server crash → Data đã commit vẫn còn
> 
> **Implementation:**
> ```csharp
> using var transaction = await _context.Database.BeginTransactionAsync();
> try
> {
>     await _orderService.CreateOrderAsync(order);
>     await _inventoryService.DeductStockAsync(productId, quantity);
>     await transaction.CommitAsync(); // All or nothing
> }
> catch
> {
>     await transaction.RollbackAsync();
> }
> ```

---

### Câu 7: **Normalization là gì? Tại sao cần normalize database?**

**Trả lời:**
> **Normalization** là quá trình organize data để giảm redundancy:
> 
> **1NF (First Normal Form):**
> - Mỗi cell chứa atomic value (không có arrays/lists)
> - Có primary key
> 
> **2NF:**
> - Thỏa 1NF
> - Không có partial dependencies (mọi non-key column phụ thuộc toàn bộ PK)
> 
> **3NF:**
> - Thỏa 2NF
> - Không có transitive dependencies (non-key columns không phụ thuộc non-key columns khác)
> 
> **Ví dụ Denormalized (Bad):**
> ```
> orders: order_id, customer_name, customer_email, customer_phone, product_name, product_price
> ```
> 
> **Normalized (Good):**
> ```
> customers: customer_id, name, email, phone
> products: product_id, name, price
> orders: order_id, customer_id (FK), product_id (FK)
> ```
> 
> **Lợi ích:**
> - ✅ Ít data duplication
> - ✅ Dễ update (chỉ update 1 chỗ)
> - ✅ Data integrity tốt hơn
> 
> **Trade-off:** Nhiều JOINs → Có thể chậm hơn (nhưng indexes giúp)

---

### Câu 8: **Repository Pattern vs Direct DbContext access?**

**Trả lời:**
> **Repository Pattern:**
> ```csharp
> public interface ICustomerRepository
> {
>     Task<Customer> GetByIdAsync(int id);
> }
> 
> // Controller
> var customer = await _customerRepo.GetByIdAsync(id);
> ```
> 
> **Direct DbContext:**
> ```csharp
> // Controller
> var customer = await _context.Customers.FindAsync(id);
> ```
> 
> **So sánh:**
> | Tiêu chí | Repository | Direct DbContext |
> |----------|------------|------------------|
> | Testability | ✅ Dễ mock | ❌ Khó mock DbContext |
> | Separation of Concerns | ✅ Clear layers | ❌ Controller biết về DB |
> | Reusability | ✅ Centralized queries | ❌ Duplicate code |
> | Flexibility | ✅ Dễ switch data source | ❌ Tight coupling |
> | Learning Curve | ❌ More code | ✅ Simple |
> 
> **Kết luận:** Repository Pattern tốt cho:
> - Large projects
> - Team collaboration
> - Need testability
> 
> Direct DbContext ok cho: Simple apps, prototypes

---

### Câu 9: **REST API best practices em áp dụng?**

**Trả lời:**
> **1. Resource-based URLs:**
> ```
> ✅ /api/customers
> ✅ /api/customers/123
> ❌ /api/getCustomer?id=123
> ```
> 
> **2. HTTP Methods semantics:**
> - GET: Read (idempotent)
> - POST: Create (not idempotent)
> - PUT: Update (idempotent)
> - DELETE: Delete (idempotent)
> 
> **3. Status Codes:**
> - 200: Success
> - 201: Created
> - 400: Bad request (validation)
> - 401: Unauthorized
> - 404: Not found
> - 500: Server error
> 
> **4. Pagination:**
> ```
> GET /api/customers?page=1&pageSize=20
> ```
> 
> **5. Filtering/Sorting:**
> ```
> GET /api/customers?status=active&sortBy=name&sortOrder=asc
> ```
> 
> **6. Versioning:**
> ```
> /api/v1/customers
> ```
> 
> **7. Consistent response format:**
> ```json
> {
>   "success": true,
>   "data": {...},
>   "message": "Success"
> }
> ```
> 
> **8. Security:**
> - JWT authentication
> - HTTPS only
> - Rate limiting

---

### Câu 10: **Caching strategies em sử dụng?**

**Trả lời:**
> **1. In-Memory Cache (IMemoryCache):**
> ```csharp
> if (_cache.TryGetValue(cacheKey, out Customer customer))
>     return customer;
> 
> customer = await _repo.GetByIdAsync(id);
> _cache.Set(cacheKey, customer, TimeSpan.FromMinutes(10));
> ```
> 
> **Use case:** Frequently accessed data (products, categories)
> 
> **2. Distributed Cache (Redis):**
> ```csharp
> services.AddStackExchangeRedisCache(options =>
> {
>     options.Configuration = "localhost:6379";
> });
> ```
> 
> **Use case:** Multi-server deployments (shared cache)
> 
> **3. Response Caching:**
> ```csharp
> [ResponseCache(Duration = 60)]
> public IActionResult GetProducts()
> ```
> 
> **Cache Invalidation:**
> ```csharp
> // Update product
> await _repo.UpdateAsync(product);
> _cache.Remove($"product_{id}");
> ```
> 
> **Cache Strategies:**
> - **Cache-Aside**: App checks cache first, load from DB if miss
> - **Write-Through**: Write to cache + DB simultaneously
> - **Write-Behind**: Write to cache, async write to DB
> 
> **Khi nào dùng cache:**
> - ✅ Read-heavy data
> - ✅ Expensive queries
> - ✅ Rarely updated data
> 
> **Khi không nên cache:**
> - ❌ Frequently updated data
> - ❌ User-specific sensitive data
> - ❌ Large datasets (memory limit)

---

## 📝 Tổng Kết Tất Cả Công Nghệ

### Backend Stack:
- ✅ C# 12 - OOP language
- ✅ ASP.NET Core 8 - Web framework
- ✅ Entity Framework Core 8 - ORM
- ✅ PostgreSQL - RDBMS
- ✅ JWT - Authentication
- ✅ Bcrypt - Password hashing
- ✅ AutoMapper - Object mapping
- ✅ Swagger - API documentation

### Frontend Stack:
- ✅ Angular 19 - SPA framework
- ✅ TypeScript - Typed JavaScript
- ✅ RxJS - Reactive programming
- ✅ Tailwind CSS - Utility-first CSS
- ✅ html5-qrcode - Barcode scanning

### Architecture:
- ✅ Layered Architecture (N-Tier)
- ✅ Repository Pattern
- ✅ Service Layer Pattern
- ✅ DTO Pattern
- ✅ Dependency Injection

### Cloud & DevOps:
- ✅ Google Cloud Storage - File storage
- ✅ Docker - Containerization
- ✅ CI/CD - Automated deployment

### Database Concepts:
- ✅ Normalization (1NF, 2NF, 3NF)
- ✅ Indexes (B-Tree, Composite)
- ✅ Transactions (ACID)
- ✅ Constraints (FK, PK, UNIQUE)

### Security:
- ✅ JWT Authentication
- ✅ Role-based Authorization
- ✅ Password Hashing (Bcrypt)
- ✅ CORS
- ✅ SQL Injection Prevention

### Performance:
- ✅ Caching (Memory, Redis)
- ✅ Pagination
- ✅ Async/Await
- ✅ Database Indexing

---

**Tài liệu này cover tất cả lý thuyết công nghệ cho hội đồng bảo vệ! 🎓**
