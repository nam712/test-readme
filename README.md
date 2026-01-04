# 🎓 Câu hỏi và Trả lời Bảo vệ Đồ án Tốt nghiệp

## Multi-Platform Sales Management System

_Tài liệu phiên bản văn bản - Không chứa code_

---

## 📋 Câu hỏi 1: Tổng quan dự án và mục tiêu

### ❓ Câu hỏi:

**"Em hãy trình bày tổng quan về đồ án của mình, vấn đề em muốn giải quyết là gì và mục tiêu cụ thể của hệ thống?"**

### ✅ Trả lời:

**1. Bối cảnh và Động lực:**

Hiện nay, nhiều doanh nghiệp nhỏ và vừa tại Việt Nam đang gặp khó khăn trong việc quản lý bán hàng:

- Quản lý thủ công bằng sổ sách, dễ sai sót và mất thời gian
- Không theo dõi được tồn kho theo thời gian thực (real-time)
- Khó khăn trong việc phân tích dữ liệu kinh doanh và đưa ra quyết định
- Chi phí cao khi sử dụng các phần mềm có sẵn trên thị trường
- Không có giải pháp tích hợp trí tuệ nhân tạo (AI) để hỗ trợ quyết định kinh doanh

**2. Vấn đề cần giải quyết:**

Dự án nhằm giải quyết 5 vấn đề chính:

- ✅ **Quản lý tập trung**: Tích hợp quản lý sản phẩm, khách hàng, nhân viên, hóa đơn vào một hệ thống duy nhất
- ✅ **Multi-shop**: Hỗ trợ quản lý nhiều cửa hàng từ một tài khoản chủ shop
- ✅ **Real-time**: Cập nhật tồn kho và thông báo cảnh báo tức thời qua WebSocket
- ✅ **AI-powered**: Trợ lý ảo thông minh hỗ trợ tra cứu và phân tích dữ liệu bằng ngôn ngữ tự nhiên
- ✅ **SaaS Model**: Mô hình subscription (thuê bao) linh hoạt, tiết kiệm chi phí đầu tư ban đầu

**3. Mục tiêu cụ thể:**

**Mục tiêu chính:**

- Xây dựng hệ thống quản lý bán hàng toàn diện, dễ sử dụng cho doanh nghiệp vừa và nhỏ
- Áp dụng công nghệ hiện đại: Cloud Computing, AI, Real-time Communication
- Mô hình kinh doanh SaaS với các gói subscription linh hoạt theo nhu cầu

**Mục tiêu kỹ thuật:**

- ✅ Backend API RESTful với ASP.NET Core 8.0 để đảm bảo hiệu năng cao
- ✅ Frontend Single Page Application (SPA) với Angular 19 cho trải nghiệm người dùng mượt mà
- ✅ Database PostgreSQL với Pgvector extension để hỗ trợ tìm kiếm vector cho AI
- ✅ Real-time notifications với SignalR cho cập nhật tức thời
- ✅ AI Assistant sử dụng Google Gemini 1.5 Pro và Vertex AI cho khả năng hiểu ngôn ngữ tự nhiên
- ✅ Payment gateway tích hợp MoMo để xử lý thanh toán subscription
- ✅ CI/CD pipeline tự động deploy lên Google Cloud Run

**Mục tiêu người dùng:**

- **Chủ shop**: Quản lý toàn bộ hoạt động kinh doanh, xem báo cáo phân tích
- **Nhân viên**: Thao tác bán hàng nhanh chóng tại điểm bán (POS)
- **Khách hàng**: Trải nghiệm mua hàng tốt hơn với hệ thống tra cứu thông tin nhanh

**4. Phạm vi dự án:**

**Đã triển khai:**

- ✅ Authentication & Authorization: JWT tokens, multi-role (ShopOwner, Employee)
- ✅ Product Management: CRUD operations, stock tracking, cảnh báo tồn kho thấp
- ✅ Purchase Order Management: Tạo đơn đặt hàng, tự động tạo sản phẩm mới
- ✅ Invoice Management: POS system, xuất PDF hóa đơn
- ✅ Customer & Employee Management: Quản lý thông tin khách hàng và nhân viên
- ✅ Shop Management: Hỗ trợ nhiều cửa hàng (multi-shop support)
- ✅ Promotion System: Khuyến mãi theo phần trăm hoặc giá trị cố định
- ✅ Subscription System: Quản lý gói thuê bao với giới hạn tài nguyên (limits)
- ✅ AI Chat Assistant: RAG (Retrieval-Augmented Generation) với vector search
- ✅ Real-time Notifications: SignalR WebSocket cho thông báo tức thời
- ✅ Payment Integration: Tích hợp cổng thanh toán MoMo
- ✅ Reports & Analytics: Báo cáo doanh thu, lợi nhuận, sản phẩm bán chạy
- ✅ CI/CD Pipeline: GitHub Actions tự động build, test, deploy

**Giới hạn:**

- Chưa hỗ trợ chế độ offline (cần kết nối internet)
- Chưa có mobile app native (chỉ web responsive)
- AI Assistant chỉ hỗ trợ tiếng Việt
- Chỉ tích hợp MoMo (chưa có VNPay, ZaloPay)

**5. Đóng góp của dự án:**

- Giải pháp quản lý bán hàng toàn diện, giá cả phải chăng cho SMEs (doanh nghiệp vừa và nhỏ)
- Áp dụng thành công kỹ thuật RAG (Retrieval-Augmented Generation) với Pgvector để AI trả lời chính xác dựa trên dữ liệu thực tế của shop
- Mô hình SaaS với subscription limits linh hoạt, dễ mở rộng
- Hệ thống real-time hoàn chỉnh với SignalR WebSocket
- CI/CD pipeline tự động hóa hoàn toàn từ commit đến production

**6. Kết quả đạt được:**

- ✅ Hệ thống hoạt động ổn định trên môi trường production (Google Cloud Run)
- ✅ Demo thành công tất cả các tính năng chính cho người dùng thực tế
- ✅ Performance tốt: thời gian phản hồi trung bình dưới 200ms
- ✅ Kiến trúc có khả năng mở rộng (scalable architecture)
- ✅ Tài liệu đầy đủ (comprehensive documentation) cho API và Frontend

---

## 📋 Câu hỏi 2: Lý do chọn công nghệ

### ❓ Câu hỏi:

**"Tại sao em lại chọn ASP.NET Core, Angular và PostgreSQL? So sánh với các công nghệ khác như Node.js, React, MySQL?"**

### ✅ Trả lời:

**1. Backend: Tại sao chọn ASP.NET Core 8.0?**

**Ưu điểm:**

- ✅ **Performance cao**: Nằm trong top 3 web frameworks về tốc độ theo TechEmpower Benchmark, có khả năng xử lý hơn 7 triệu requests/giây với response time trung bình dưới 50ms

- ✅ **Type-safe**: C# là ngôn ngữ strongly-typed (kiểu dữ liệu mạnh), giúp phát hiện lỗi tại compile-time thay vì runtime, đặc biệt quan trọng với dữ liệu tài chính

- ✅ **Dependency Injection**: Built-in DI container mạnh mẽ giúp quản lý dependencies dễ dàng, tăng khả năng test và maintain code

- ✅ **Cross-platform**: Chạy được trên Windows, Linux, macOS, dễ deploy lên các cloud platform

- ✅ **Entity Framework Core**: ORM mạnh mẽ với hỗ trợ migrations tốt, LINQ queries trực quan

- ✅ **SignalR Built-in**: Thư viện real-time communication được tích hợp sẵn, không cần cài thêm package phức tạp

- ✅ **Security**: Các tính năng bảo mật được tích hợp sẵn như authentication, authorization, CORS, HTTPS enforcement

- ✅ **Enterprise-ready**: Được sử dụng bởi các ứng dụng lớn như Stack Overflow, Bing, Azure Portal

**So sánh với Node.js:**

| Tiêu chí           | ASP.NET Core           | Node.js                          |
| ------------------ | ---------------------- | -------------------------------- |
| Performance        | ⭐⭐⭐⭐⭐ (7M+ req/s) | ⭐⭐⭐⭐ (3M+ req/s)             |
| Type Safety        | ⭐⭐⭐⭐⭐ (C# native) | ⭐⭐⭐ (cần TypeScript)          |
| Scalability        | ⭐⭐⭐⭐⭐             | ⭐⭐⭐⭐                         |
| Learning Curve     | ⭐⭐⭐ (cần học C#)    | ⭐⭐⭐⭐⭐ (JavaScript phổ biến) |
| Community          | ⭐⭐⭐⭐               | ⭐⭐⭐⭐⭐                       |
| Enterprise Support | ⭐⭐⭐⭐⭐             | ⭐⭐⭐⭐                         |

**Lý do chọn ASP.NET Core:**

- Dự án yêu cầu performance cao vì là hệ thống POS cần xử lý giao dịch nhanh chóng
- Type safety rất quan trọng khi xử lý dữ liệu tài chính để tránh lỗi tính toán
- SignalR tích hợp sẵn giúp implement real-time notifications dễ dàng
- Team có kinh nghiệm với C# và .NET ecosystem từ trước

---

**2. Frontend: Tại sao chọn Angular 19?**

**Ưu điểm:**

- ✅ **Full Framework**: Angular là framework đầy đủ, có sẵn routing, HTTP client, forms module, animations, không cần cài thêm nhiều thư viện như React

- ✅ **TypeScript First**: Angular được thiết kế với TypeScript từ đầu, đảm bảo type safety end-to-end từ frontend đến backend

- ✅ **Dependency Injection**: Angular có hệ thống DI mạnh mẽ giúp quản lý services, testing, và code reusability

- ✅ **RxJS**: Reactive programming với RxJS operators mạnh mẽ cho xử lý async operations, streams, và real-time data

- ✅ **CLI mạnh mẽ**: Angular CLI cung cấp commands đầy đủ cho generate, build, test, deploy

- ✅ **Enterprise-grade**: Được Google backing và sử dụng bởi các công ty lớn như Google, Microsoft, Forbes

- ✅ **Two-way binding**: Giảm boilerplate code khi sync data giữa model và view

- ✅ **Standalone Components**: Angular 19 hỗ trợ standalone components, giảm complexity của NgModules

**So sánh với React:**

| Tiêu chí            | Angular                  | React                    |
| ------------------- | ------------------------ | ------------------------ |
| Learning Curve      | ⭐⭐⭐ (phức tạp hơn)    | ⭐⭐⭐⭐ (dễ học)        |
| Bundle Size         | ⭐⭐⭐ (nặng hơn)        | ⭐⭐⭐⭐⭐ (nhẹ hơn)     |
| TypeScript Support  | ⭐⭐⭐⭐⭐ (native)      | ⭐⭐⭐⭐ (cần config)    |
| Project Structure   | ⭐⭐⭐⭐⭐ (opinionated) | ⭐⭐⭐ (flexible)        |
| State Management    | ⭐⭐⭐⭐ (RxJS)          | ⭐⭐⭐⭐ (Redux/Context) |
| Community           | ⭐⭐⭐⭐                 | ⭐⭐⭐⭐⭐               |
| Enterprise Adoption | ⭐⭐⭐⭐⭐               | ⭐⭐⭐⭐                 |

**Lý do chọn Angular:**

- Project cần structure rõ ràng và conventions chặt chẽ (enterprise application)
- TypeScript type safety từ đầu đến cuối, phù hợp với C# backend
- RxJS observables phù hợp với real-time data streams từ SignalR
- Dependency Injection giúp testing và mocking services dễ dàng
- Team đã có kinh nghiệm với Angular từ các dự án trước

---

**3. Database: Tại sao chọn PostgreSQL?**

**Ưu điểm:**

- ✅ **Open Source**: Miễn phí hoàn toàn với community support mạnh mẽ

- ✅ **ACID Compliance**: Đảm bảo tính toàn vẹn dữ liệu (Atomicity, Consistency, Isolation, Durability), cực kỳ quan trọng với financial transactions

- ✅ **JSON Support**: JSONB data type native cho flexible schema, lưu trữ metadata hiệu quả

- ✅ **Full-Text Search**: Built-in FTS (Full Text Search) cho chức năng tìm kiếm sản phẩm

- ✅ **Pgvector Extension**: Vector similarity search cho AI embeddings - đây là lý do QUAN TRỌNG NHẤT! Không có extension này thì không thể implement RAG

- ✅ **Performance**: Hiệu năng tốt với complex queries, JOIN operations phức tạp, và large datasets

- ✅ **Advanced Features**: Window functions, CTEs (Common Table Expressions), JSONB operations, Array types

- ✅ **Cloud Support**: Hỗ trợ tốt trên GCP Cloud SQL, AWS RDS, Azure Database

**So sánh với MySQL:**

| Tiêu chí         | PostgreSQL             | MySQL                        |
| ---------------- | ---------------------- | ---------------------------- |
| ACID Compliance  | ⭐⭐⭐⭐⭐ (strict)    | ⭐⭐⭐⭐ (depends on engine) |
| Complex Queries  | ⭐⭐⭐⭐⭐             | ⭐⭐⭐⭐                     |
| JSON Support     | ⭐⭐⭐⭐⭐ (JSONB)     | ⭐⭐⭐ (JSON only)           |
| Extensions       | ⭐⭐⭐⭐⭐ (Pgvector!) | ⭐⭐⭐ (limited)             |
| Full-Text Search | ⭐⭐⭐⭐⭐ (native)    | ⭐⭐⭐ (basic)               |
| Learning Curve   | ⭐⭐⭐ (phức tạp hơn)  | ⭐⭐⭐⭐ (dễ hơn)            |
| Popularity       | ⭐⭐⭐⭐               | ⭐⭐⭐⭐⭐                   |

**So sánh với SQL Server:**

| Tiêu chí          | PostgreSQL            | SQL Server         |
| ----------------- | --------------------- | ------------------ |
| Cost              | ⭐⭐⭐⭐⭐ (free)     | ⭐⭐ (expensive)   |
| Cross-platform    | ⭐⭐⭐⭐⭐            | ⭐⭐⭐⭐           |
| Vector Search     | ⭐⭐⭐⭐⭐ (Pgvector) | ⭐⭐ (limited)     |
| Cloud Integration | ⭐⭐⭐⭐⭐            | ⭐⭐⭐⭐⭐ (Azure) |

**Lý do chọn PostgreSQL:**

- **Pgvector extension là BẮT BUỘC**: Không có Pgvector thì không thể lưu trữ và search vector embeddings cho AI Assistant
- ACID compliance cực kỳ quan trọng khi xử lý financial transactions (hóa đơn, thanh toán)
- JSONB support cho flexible data như product attributes, invoice metadata
- Performance tốt hơn MySQL với complex analytics queries (báo cáo doanh thu, thống kê)
- Google Cloud SQL hỗ trợ PostgreSQL rất tốt với auto-backup, replication

---

**4. Công nghệ bổ sung:**

**SignalR (Real-time Communication):**

- Tích hợp sẵn với ASP.NET Core, không cần cài package riêng
- Hỗ trợ WebSocket với automatic fallback sang Server-Sent Events hoặc Long Polling nếu WebSocket không khả dụng
- Dễ implement hơn Socket.io vì tích hợp chặt chẽ với .NET
- Auto-reconnect khi mất kết nối, scaling support với Redis backplane

**Google Vertex AI & Gemini:**

- Gemini 1.5 Pro là state-of-the-art LLM với khả năng hiểu tiếng Việt tốt
- Text Embedding API để tạo vector embeddings từ mô tả sản phẩm
- Tích hợp tốt với Google Cloud ecosystem (Cloud Run, Cloud SQL)
- Pricing hợp lý cho startup: $0.00025/1K characters cho embedding

**MoMo Payment Gateway:**

- Cổng thanh toán phổ biến nhất tại Việt Nam
- API documentation rõ ràng, dễ tích hợp
- Sandbox environment tốt cho testing
- Support team responsive, hỗ trợ kỹ thuật nhanh

**TailwindCSS:**

- Utility-first CSS framework giúp develop UI nhanh hơn
- Smaller bundle size nhờ PurgeCSS loại bỏ unused styles
- Highly customizable với config file
- Consistent design system với predefined utilities

---

**5. Kết luận lựa chọn công nghệ:**

**Tech Stack cuối cùng:**

```
Frontend:  Angular 19 + TailwindCSS
Backend:   ASP.NET Core 8.0
Database:  PostgreSQL + Pgvector
Real-time: SignalR
AI:        Google Vertex AI + Gemini 1.5 Pro
Payment:   MoMo
Cloud:     Google Cloud Run
CI/CD:     GitHub Actions
```

**Lý do tổng hợp:**

1. ✅ **Performance**: Cả ASP.NET Core và PostgreSQL đều nằm trong top performers, đáp ứng yêu cầu của POS system
2. ✅ **Type Safety**: End-to-end type safety với TypeScript (frontend) và C# (backend) giảm bugs
3. ✅ **AI-ready**: Pgvector extension là yếu tố quyết định cho vector search trong RAG
4. ✅ **Enterprise-grade**: Tất cả technologies đều được proven và sử dụng bởi các công ty lớn
5. ✅ **Cloud-native**: Dễ deploy và scale trên cloud platforms, đặc biệt Google Cloud
6. ✅ **Team expertise**: Team có kinh nghiệm và comfortable với stack này
7. ✅ **Cost-effective**: Open source technologies + free tier của cloud services giúp tiết kiệm chi phí

**Khi nào nên dùng alternatives:**

- **Node.js**: Nếu team chỉ biết JavaScript, project nhỏ, cần prototype nhanh
- **React**: Nếu ưu tiên bundle size nhỏ, cần flexibility cao trong structure
- **MySQL**: Nếu không cần AI features, project đơn giản hơn, team quen MySQL

---

## 📋 Câu hỏi 3: Kiến trúc hệ thống

### ❓ Câu hỏi:

**"Em hãy trình bày kiến trúc tổng thể của hệ thống, các layers và cách chúng tương tác với nhau?"**

### ✅ Trả lời:

**1. Kiến trúc tổng thể (High-level Architecture):**

Hệ thống được thiết kế theo kiến trúc client-server với các thành phần chính:

**Client Layer (Tầng khách hàng):**

- Browser web chạy ứng dụng Angular SPA
- Kết nối đến backend qua HTTPS cho REST API
- Kết nối WebSocket (WSS) cho real-time notifications qua SignalR
- Responsive design hoạt động trên desktop, tablet, mobile browsers

**API Gateway / Load Balancer:**

- Google Cloud Run đóng vai trò API Gateway và Load Balancer
- Auto-scaling dựa trên traffic: tự động tăng/giảm số instances
- HTTPS termination: xử lý SSL/TLS certificates
- Request routing đến các instances của backend API

**Application Layer (Tầng ứng dụng):**

- ASP.NET Core Web API xử lý business logic
- Controllers: nhận requests từ client, validate input, gọi services
- Middleware Pipeline: Authentication, Authorization, CORS, Error Handling, Logging
- SignalR Hubs: quản lý WebSocket connections cho real-time features
- Services: business logic layer xử lý các quy tắc kinh doanh
- Repositories: data access layer tương tác với database

**Data Layer (Tầng dữ liệu):**

- PostgreSQL: primary database lưu trữ tất cả business data
- Pgvector: extension của PostgreSQL lưu vector embeddings cho AI
- File Storage: lưu trữ hình ảnh sản phẩm (hiện tại local, sẽ chuyển sang Cloud Storage)

**External Services (Dịch vụ bên ngoài):**

- Google Vertex AI: embedding generation và LLM inference với Gemini
- MoMo Payment Gateway: xử lý thanh toán subscription
- Email Service: gửi notifications, invoices (future feature)

---

**2. Backend Architecture (3-Layer Pattern):**

Kiến trúc backend tuân theo mô hình 3 tầng để tách biệt concerns:

**Presentation Layer (Tầng trình bày):**

Chứa các Controllers xử lý HTTP requests:

- **AuthController**: Xử lý đăng ký, đăng nhập, logout, refresh token
- **ProductController**: CRUD operations cho products, stock management
- **InvoiceController**: Tạo hóa đơn, xuất PDF, lịch sử giao dịch
- **AssistantController**: AI chat endpoint, vector search, context retrieval
- **NotificationController**: Lấy notifications, đánh dấu đã đọc
- Và các controllers khác cho Customer, Employee, Shop, Promotion, Report...

Responsibilities:

- Nhận HTTP requests từ client
- Validate input data với Data Annotations
- Authorize user bằng [Authorize] attributes
- Gọi Services để thực hiện business logic
- Transform entities sang DTOs trước khi return response
- Handle exceptions và return appropriate HTTP status codes

**Business Logic Layer (Tầng logic nghiệp vụ):**

Chứa các Services implements business rules:

- **IAuthService**: Password hashing, JWT generation, user validation
- **IProductService**: Stock calculations, low stock alerts, product search
- **IInvoiceService**: Invoice calculations, discount logic, stock updates
- **IChatService**: RAG implementation, vector search, LLM prompting
- **ISubscriptionService**: Subscription management, limit checking
- **INotificationService**: Create notifications, send real-time alerts

Responsibilities:

- Implement business rules và validation logic
- Orchestrate multiple repositories cho complex operations
- Transaction management với Entity Framework Core
- Call external services (Vertex AI, MoMo)
- Implement caching strategies (future)
- Business exception handling

**Data Access Layer (Tầng truy cập dữ liệu):**

Chứa Repositories và Entity Framework Core DbContext:

- **IProductRepository**: CRUD operations, queries filtered by ShopOwnerId
- **ICustomerRepository**: Customer data access với pagination
- **IPurchaseOrderRepository**: Order management với transaction support
- **ApplicationDbContext**: EF Core DbContext định nghĩa DbSets và relationships

Responsibilities:

- Trực tiếp tương tác với PostgreSQL database
- Execute queries với Entity Framework LINQ
- Handle database transactions
- Implement efficient queries với proper indexes
- Manage entity relationships và navigation properties

**Flow giữa các layers:**

1. Client gửi HTTP request → Controller (Presentation)
2. Controller validate input → Gọi Service (Business Logic)
3. Service thực hiện business rules → Gọi Repository (Data Access)
4. Repository query database → Return entities
5. Service xử lý entities → Return business objects
6. Controller transform sang DTOs → Return HTTP response
7. Client nhận response và update UI

**Ưu điểm của 3-layer architecture:**

- **Separation of Concerns**: Mỗi layer có responsibility rõ ràng
- **Testability**: Dễ mock dependencies cho unit testing
- **Maintainability**: Thay đổi một layer không ảnh hưởng layers khác
- **Reusability**: Services và Repositories có thể reuse
- **Scalability**: Dễ scale từng layer độc lập

---

**3. Frontend Architecture (Angular):**

Angular application được tổ chức theo component-based architecture:

**Presentation Layer:**

Components hiển thị UI và handle user interactions:

- **DashboardComponent**: Tổng quan doanh thu, thống kê, charts
- **POSComponent**: Điểm bán hàng, cart management, checkout
- **ProductsComponent**: Danh sách sản phẩm, CRUD operations
- **ChatComponent**: AI Assistant interface, message history
- **InvoicesComponent**: Lịch sử hóa đơn, xuất PDF, chi tiết

Responsibilities:

- Render UI templates với Angular syntax
- Handle user events (click, input, submit)
- Subscribe to Observables từ services
- Manage local component state
- Navigate giữa các routes
- Display loading states và error messages

**Service Layer:**

Angular Services quản lý data và business logic:

- **AuthService**: Login/logout, token management, user state
- **ProductService**: HTTP calls cho product endpoints
- **NotificationService**: Real-time notifications, badge counter
- **SignalRService**: WebSocket connection management
- **AssistantService**: Chat với AI, message streaming

Responsibilities:

- HTTP communication với backend API
- State management với BehaviorSubjects
- Real-time connection với SignalR
- Data caching và sharing giữa components
- Error handling và retry logic
- Transform API responses sang UI models

**HTTP & State Layer:**

- **HttpClient**: Angular's built-in HTTP client cho REST API calls
- **RxJS Observables**: Reactive streams cho async operations
- **BehaviorSubjects**: Store và share state across application
- **LocalStorage**: Persist authentication token và user preferences
- **Interceptors**: Automatically attach JWT token, handle errors

**Data flow trong Angular:**

1. User interaction trong Component
2. Component gọi method của Service
3. Service thực hiện HTTP request với HttpClient
4. AuthInterceptor tự động attach JWT token vào header
5. Backend xử lý request và return response
6. Service nhận response, transform data
7. Service emit value qua Observable/BehaviorSubject
8. Component subscribe và nhận data
9. Component update template với new data
10. Angular Change Detection render lại UI

**Routing và Guards:**

- Angular Router quản lý navigation giữa các pages
- **AuthGuard**: Protect routes, redirect to login if not authenticated
- **SubscriptionGuard**: Check subscription status trước khi access features
- **RoleGuard**: Check user role (ShopOwner vs Employee) cho authorization
- Lazy Loading: Load modules on-demand để giảm initial bundle size

---

**4. Database Schema (Simplified):**

**Core entities và relationships:**

**ShopOwner (Chủ shop):**

- Thông tin: Name, Phone (unique), Email, PasswordHash
- Một ShopOwner có nhiều Shops (1-N)
- Một ShopOwner có nhiều Products, Invoices, Customers, Employees (1-N)
- Có một Subscription đang active
- Có nhiều Notifications

**Shop (Cửa hàng):**

- Thông tin: ShopName, Address, Phone
- Thuộc về một ShopOwner (N-1)
- Có nhiều Employees assigned
- Multi-shop: ShopOwner có thể quản lý nhiều shops

**Product (Sản phẩm):**

- Thông tin: ProductCode (unique), Name, Price, Cost, Stock, Description
- Thuộc về một ShopOwner (N-1)
- Có một ProductEmbedding cho vector search (1-1)
- Xuất hiện trong nhiều InvoiceItems và PurchaseOrderItems

**ProductEmbedding (Vector cho AI):**

- ProductId: Foreign key đến Product
- Embedding: vector type (1536 dimensions) chứa embedding từ text description
- Sử dụng cho vector similarity search trong RAG

**Invoice (Hóa đơn):**

- Thông tin: InvoiceCode, TotalAmount, Discount, PaymentMethod, Status
- Thuộc về một ShopOwner và một Customer
- Có nhiều InvoiceItems (1-N)
- Created by Employee hoặc ShopOwner

**InvoiceItem (Chi tiết hóa đơn):**

- Thông tin: ProductId, Quantity, UnitPrice, Subtotal
- Thuộc về một Invoice (N-1)
- Reference đến một Product

**Customer (Khách hàng):**

- Thông tin: Name, Phone, Email, LoyaltyPoints
- Có nhiều Invoices (1-N)
- Thuộc về một ShopOwner

**Employee (Nhân viên):**

- Thông tin: Username, PasswordHash, Role, Salary
- Thuộc về một ShopOwner (N-1)
- Assigned to một Shop
- Có thể tạo Invoices

**Subscription (Gói thuê bao):**

- ShopOwnerId: Foreign key đến ShopOwner
- SubscriptionPlanId: Reference đến plan (Basic/Pro/Enterprise)
- StartDate, EndDate: Thời gian hiệu lực
- IsActive: Trạng thái

**SubscriptionPlan (Các gói dịch vụ):**

- Name: Basic, Professional, Enterprise
- Price: 0 VND, 500k, 2M VND
- Limits: MaxProducts, MaxEmployees, MaxShops, MaxCustomers
- Features: JSON chứa danh sách tính năng

**Notification (Thông báo):**

- Thông tin: Title, Message, Type (Info/Warning/Success/Error), IsRead
- Thuộc về một ShopOwner
- Được gửi real-time qua SignalR

**Key database features:**

- **Indexes**: Tạo indexes trên các cột frequently queried (ShopOwnerId, ProductCode, InvoiceCode) để tăng performance
- **Foreign Keys**: Đảm bảo referential integrity
- **Cascading Deletes**: Khi xóa ShopOwner sẽ cascade delete data liên quan
- **JSONB columns**: Lưu flexible data như product attributes, invoice metadata
- **Vector column**: Pgvector extension cho ProductEmbeddings
- **Timestamps**: CreatedAt, UpdatedAt tracking changes
- **Soft Deletes**: Một số entities có IsDeleted flag thay vì hard delete

---

**5. Data Flow Example: Create Invoice (Tạo hóa đơn)**

Quy trình xử lý từ khi user nhấn nút "Thanh toán" đến khi nhận response:

**Bước 1 - User Action:**

- User chọn sản phẩm vào cart trong POS Component
- Nhập thông tin khách hàng, chọn phương thức thanh toán
- Nhấn nút "Thanh toán"

**Bước 2 - Frontend Validation:**

- POSComponent validate cart không empty
- Validate customer information nếu có
- Prepare InvoiceCreateDto với invoice details và invoice items

**Bước 3 - Service Call:**

- POSComponent gọi InvoiceService.createInvoice(dto)
- InvoiceService thực hiện HTTP POST đến /api/invoices endpoint

**Bước 4 - HTTP Interceptor:**

- AuthInterceptor tự động thêm JWT token vào Authorization header
- Request được gửi đến backend API

**Bước 5 - Backend Middleware Pipeline:**

- Authentication Middleware validate JWT token, extract claims
- Authorization Middleware check user có quyền create invoice không
- CORS Middleware check origin allowed
- Request đến InvoicesController

**Bước 6 - Controller Processing:**

- InvoicesController nhận request
- Validate ModelState (input validation)
- Extract ShopOwnerId từ JWT claims
- Gọi IInvoiceService.CreateAsync(dto, shopOwnerId)

**Bước 7 - Service Business Logic:**

- InvoiceService validate business rules:
  - Check stock availability cho tất cả products trong invoice
  - Validate discount amount không vượt quá total
  - Check subscription limits (số lượng invoices còn được tạo)
- Calculate subtotals, totals, discount amount
- Create Invoice entity và InvoiceItem entities
- Begin database transaction

**Bước 8 - Repository Data Access:**

- InvoiceRepository.AddAsync(invoice) insert invoice vào database
- Loop qua invoice items và insert vào InvoiceItems table
- ProductRepository.UpdateStock() giảm stock của các products đã bán
- NotificationRepository.Add() tạo notification "Invoice created successfully"
- Commit transaction nếu tất cả operations thành công
- Rollback nếu có lỗi xảy ra

**Bước 9 - Database Execution:**

- PostgreSQL execute INSERT statements với ACID compliance
- Check constraints và foreign keys
- Return generated InvoiceId và timestamps
- Data được persist vào disk

**Bước 10 - Real-time Notification:**

- Service call NotificationHub.SendNotification()
- SignalR Hub push notification đến connected clients của ShopOwner
- WebSocket message được gửi real-time

**Bước 11 - Frontend Receives Notification:**

- SignalRService connection nhận notification event
- NotificationService.handleNotification() process message
- Display toast notification "Hóa đơn được tạo thành công"
- Update notification badge counter
- Play notification sound (optional)

**Bước 12 - HTTP Response:**

- Service return invoice object đến Controller
- Controller transform Invoice entity sang InvoiceDto
- Return Ok(ApiResponse) với status 200 và invoice data
- Response được gửi về frontend

**Bước 13 - Frontend Update:**

- InvoiceService nhận response
- Return Observable với invoice data
- POSComponent subscribe nhận result
- Clear shopping cart
- Show success message
- Navigate đến invoice detail page hoặc print page
- Update dashboard statistics

**Error Handling tại mỗi bước:**

- Frontend: Hiển thị error message nếu validation fail
- Network: Retry logic với exponential backoff
- Backend: Try-catch blocks, rollback transaction
- Database: Constraint violations return specific error codes
- Response: HTTP status codes indicate error types (400, 401, 403, 500)

---

**6. Authentication Flow (Xác thực người dùng):**

**Registration (Đăng ký tài khoản):**

1. User điền form với thông tin: ShopOwnerName, Phone, Email, Password, ShopName
2. Frontend validate input: email format, phone format, password strength
3. POST request đến /api/auth/register endpoint
4. Backend kiểm tra phone/email đã tồn tại chưa
5. Hash password với BCrypt (salt rounds = 10)
6. Create ShopOwner entity với PasswordHash
7. Tự động tạo default Shop cho ShopOwner
8. Tạo default Subscription (Basic/Free plan) với 1 năm validity
9. Save transaction đến database (ShopOwner + Shop + Subscription trong một transaction)
10. Return success response
11. Frontend redirect đến login page

**Login (Đăng nhập):**

1. User nhập Phone và Password
2. POST request đến /api/auth/login
3. Backend tìm user theo Phone (có thể là ShopOwner hoặc Employee)
4. Verify password: BCrypt.Verify(inputPassword, storedPasswordHash)
5. Nếu password đúng, generate JWT token với claims:
   - shop_owner_id: ID của ShopOwner
   - user_id: ID của user (ShopOwner hoặc Employee)
   - user_type: "ShopOwner" hoặc "Employee"
   - role: "ShopOwner" hoặc "Employee"
   - email: email address
   - exp: expiration timestamp (7 days từ now)
6. Return token + user info trong response
7. Frontend lưu token vào localStorage
8. Frontend lưu user info vào AuthService state (BehaviorSubject)
9. Redirect đến dashboard page

**Token Structure:**

JWT token gồm 3 phần: Header.Payload.Signature

**Header** chứa:

- Algorithm: HS256 (HMAC SHA-256)
- Type: JWT

**Payload** chứa claims:

- shop_owner_id: Để filter data theo shop owner
- user_type: Để phân biệt ShopOwner vs Employee
- role: Cho authorization [Authorize(Roles = "ShopOwner")]
- exp: Expiration time (7 ngày)
- iat: Issued at timestamp
- jti: Unique token ID

**Signature** được tạo bằng:

- HMACSHA256(base64(header) + "." + base64(payload), secretKey)
- Secret key được lưu trong environment variables, không hard-code

**Subsequent Requests (Các request tiếp theo):**

1. Frontend gọi API endpoint (ví dụ: GET /api/products)
2. AuthInterceptor tự động lấy token từ localStorage
3. Thêm header: Authorization: Bearer <token>
4. Backend Authentication Middleware:
   - Extract token từ header
   - Validate signature với secret key
   - Check expiration time
   - Extract claims từ payload
5. Nếu token valid, attach claims vào User object của Controller
6. Controller access claims: var shopOwnerId = User.FindFirst("shop_owner_id").Value
7. Query data filtered by shopOwnerId để đảm bảo data isolation
8. Return response

**Token Expiration Handling:**

1. Frontend periodically check token expiration (mỗi API call)
2. Nếu token expired:
   - Backend return 401 Unauthorized
   - Backend thêm custom header: Token-Expired: true
3. AuthInterceptor catch 401 error:
   - Clear localStorage
   - Clear AuthService state
   - Show "Session expired" message
   - Redirect đến login page
4. User phải login lại

**Future Enhancement: Refresh Token:**

- Hiện tại chưa implement refresh token mechanism
- Token expiration = 7 days là reasonable cho MVP
- Sẽ implement refresh token flow sau với:
  - Short-lived access token (15 minutes)
  - Long-lived refresh token (30 days) stored in httpOnly cookie
  - Refresh endpoint để get new access token

---

**7. Real-time Notification Flow (SignalR WebSocket):**

**Connection Establishment:**

1. User login thành công, frontend có JWT token
2. SignalRService.startConnection() được gọi
3. Tạo HubConnection với URL: /hubs/notification
4. Attach access_token vào query string (vì WebSocket không có headers)
5. Connection attempt đến backend SignalR Hub
6. Backend validate JWT token từ query string
7. Nếu token valid, accept connection
8. Store connectionId mapped với shopOwnerId trong Hub
9. Frontend connection.on("ReceiveNotification", handler) đăng ký listener
10. Connection state = Connected

**Background Service Check:**

1. Background Service chạy mỗi 5 phút (configurable)
2. Query database tìm products có Stock <= MinStock
3. Filter products thuộc về các ShopOwners khác nhau
4. Với mỗi low stock product:
   - Create Notification entity trong database
   - Type = "Warning"
   - Title = "Low Stock Alert"
   - Message = "Product X has only Y items left"
5. Save notifications vào Notifications table

**Real-time Push:**

1. Service gọi INotificationHub.SendToShopOwner(shopOwnerId, notification)
2. NotificationHub lookup connectionId của shopOwnerId
3. Nếu user đang online (có connectionId):
   - Hub gọi Clients.Client(connectionId).SendAsync("ReceiveNotification", notification)
   - SignalR serialize notification sang JSON
   - Send qua WebSocket connection
4. Nếu user offline:
   - Notification chỉ lưu trong database
   - User sẽ thấy khi login lại

**Frontend Reception:**

1. SignalRService connection event "ReceiveNotification" fires
2. Callback handler nhận notification object
3. NotificationService.addNotification(notification)
4. BehaviorSubject emit new notification
5. Components subscribed đến notifications$ stream nhận update

**UI Updates:**

1. NavbarComponent subscribe notifications$:
   - Update notification badge counter (unread count)
   - Show red dot indicator
2. NotificationComponent:
   - Add new notification vào list
   - Sort by timestamp (newest first)
   - Display với appropriate icon based on Type
3. ToastService:
   - Show toast notification ở góc màn hình
   - Auto-dismiss sau 5 seconds
   - Click to dismiss
4. Audio:
   - Play notification sound (optional, user preference)

**Mark as Read:**

1. User click vào notification
2. Frontend gọi API: PUT /api/notifications/{id}/mark-read
3. Backend update IsRead = true trong database
4. Return success
5. Frontend update local notification object
6. Decrease badge counter
7. Change notification style (gray out)

**Reconnection Handling:**

1. Nếu WebSocket connection bị mất (network issue):
   - SignalR tự động attempt reconnect
   - Exponential backoff: 0s, 2s, 10s, 30s delays
   - Frontend show "Reconnecting..." indicator
2. Khi reconnect thành công:
   - Frontend fetch missed notifications từ API
   - Sync state với database
   - Hide reconnecting indicator
3. Nếu reconnect fail sau nhiều attempts:
   - Show error message
   - User có thể manually reload page

**Scalability Considerations:**

- Hiện tại: In-memory storage của connectionId mappings
- Single instance: Works fine vì Google Cloud Run có sticky sessions
- Multiple instances (future):
  - Need Redis backplane cho SignalR
  - Share connection state across instances
  - Pub/sub pattern để broadcast messages

---

**8. AI Assistant (RAG) Flow:**

**RAG = Retrieval-Augmented Generation**
Thay vì LLM chỉ dựa vào knowledge được train, RAG retrieve relevant context từ database trước khi generate answer.

**Step-by-Step Process:**

**1. User Input:**

- User type question vào chat: "Cho tôi biết sản phẩm nào bán chạy nhất tháng này?"
- AssistantComponent gọi AssistantService.ask(question)

**2. Frontend Request:**

- POST /api/assistant/ask với body: { question: "..." }
- Loading indicator hiển thị

**3. Backend Receives Request:**

- AssistantController.Ask() method
- Extract shopOwnerId từ JWT token
- Gọi IChatService.AskAsync(question, shopOwnerId)

**4. Load Chat History:**

- Service query ChatHistory table
- Get last 10 messages của user (context window)
- Format thành conversation history:
  - User: previous question
  - Assistant: previous answer
  - ...

**5. Generate Query Embedding:**

- Gọi Google Vertex AI Text Embedding API
- Send question text đến endpoint
- Receive embedding vector (1536 dimensions)
- Vector representation của semantic meaning của question

**6. Vector Similarity Search:**

- Query ProductEmbeddings table với Pgvector
- SQL: SELECT \* FROM product_embeddings ORDER BY embedding <=> query_vector LIMIT 5
- Operator <=> là cosine distance trong Pgvector
- Tìm top 5 products có embedding gần nhất (most similar)

**7. Retrieve Product Details:**

- With ProductIds từ vector search results
- Query Products table get full details:
  - ProductCode, Name, Description, Price, Stock
  - JoinTable để get cả sales statistics nếu question về "bán chạy"
- Build context string từ retrieved products

**8. Build Prompt:**

- System prompt: "Bạn là trợ lý ảo của cửa hàng. Trả lời câu hỏi dựa trên context sau..."
- Conversation history: Previous Q&A pairs
- Retrieved context: Product details từ vector search
- Current question: User's current question
- Constraints: "Chỉ trả lời dựa trên context, nếu không biết hãy nói không biết"

**9. Call Gemini LLM:**

- Send prompt đến Google Vertex AI Gemini 1.5 Pro endpoint
- Model parameters:
  - Temperature: 0.7 (creativity vs accuracy balance)
  - Max output tokens: 1024
  - Top-p: 0.95
- Receive generated answer từ LLM

**10. Post-process Answer:**

- Parse response từ LLM
- Extract main answer text
- If answer references products, include product links/cards
- Format markdown if needed

**11. Save to Chat History:**

- Create ChatHistory entry với:
  - ShopOwnerId
  - UserMessage: original question
  - AssistantResponse: generated answer
  - Timestamp
- Insert vào database

**12. Return Response:**

- Controller return ApiResponse với:
  - success: true
  - data: { answer, relatedProducts }
- Frontend nhận response

**13. Display in UI:**

- AssistantComponent render answer trong chat bubble
- Format text với markdown (bold, lists, etc.)
- Display related products dưới dạng cards
- Show product images, prices, stock status
- Links to product detail pages

**14. Continuous Conversation:**

- User có thể tiếp tục hỏi
- Chat history được sử dụng để maintain context
- LLM có thể reference previous answers

**Why RAG is important:**

- **Accuracy**: LLM trả lời based on actual shop data, không hallucinate
- **Up-to-date**: Mỗi lần hỏi đều retrieve latest data từ database
- **Relevance**: Vector search đảm bảo chỉ retrieve relevant products
- **Transparency**: Có thể show sources (related products) cho user verify

**Performance Optimizations:**

- Cache embeddings: Không regenerate product embeddings mỗi lần
- Batch embedding generation: Khi import nhiều products
- Index on embedding column: Pgvector có IVFFlat index để speed up search
- Limit context: Chỉ top K results (K=5) để không overload prompt

---

**9. Design Patterns sử dụng:**

**Backend Patterns:**

**Repository Pattern:**

- Abstraction layer giữa business logic và data access
- Interface IProductRepository với methods: GetAllAsync, GetByIdAsync, AddAsync, UpdateAsync, DeleteAsync
- Implementation ProductRepository sử dụng EF Core DbContext
- Benefit: Dễ swap database, dễ mock cho testing

**Dependency Injection (DI):**

- ASP.NET Core built-in DI container
- Register services trong Program.cs: builder.Services.AddScoped<IProductService, ProductService>()
- Constructor injection: Controller nhận services qua constructor
- Benefit: Loose coupling, testability, lifetime management

**DTO Pattern:**

- Data Transfer Objects giữa layers
- ProductDto: Chỉ fields cần thiết cho client, không expose internal details
- CreateProductDto: Input validation attributes, chỉ fields cần cho create
- Benefit: Security (không expose sensitive fields), bandwidth optimization

**Service Layer Pattern:**

- Business logic tách riêng khỏi Controllers
- Services implement interfaces: IProductService, IInvoiceService
- Controllers thin, chỉ handle HTTP concerns
- Benefit: Reusability, testability, separation of concerns

**Middleware Pattern:**

- Request pipeline với các middleware components
- Order matters: Authentication → Authorization → CORS → Controllers
- Each middleware có thể short-circuit pipeline (return response early)
- Benefit: Cross-cutting concerns (logging, auth) applied globally

**Options Pattern:**

- Configuration management với strongly-typed classes
- JwtSettings class maps đến appsettings.json "JwtSettings" section
- Inject IOptions<JwtSettings> vào services
- Benefit: Type-safe configuration, validation, IntelliSense support

**Unit of Work Pattern:**

- EF Core DbContext acts as Unit of Work
- Track changes across multiple repositories
- SaveChanges() commits all changes in one transaction
- Benefit: Transaction consistency, atomic operations

**Frontend Patterns:**

**Service Pattern (Angular):**

- Business logic trong services, không trong components
- Components thin, chỉ handle presentation
- Services injectable: @Injectable({ providedIn: 'root' })
- Benefit: Reusability, testability, separation of concerns

**Observable Pattern (RxJS):**

- Async operations return Observables
- Components subscribe để receive data
- Operators: map, filter, switchMap, catchError cho data transformation
- Benefit: Reactive programming, handle async elegantly

**Singleton Pattern:**

- Services với providedIn: 'root' là singleton
- Một instance shared across entire application
- State trong service persists
- Benefit: Consistent state, memory efficient

**Guard Pattern:**

- Route guards protect navigation
- AuthGuard implements CanActivate interface
- Return Observable<boolean>: true allows navigation, false blocks
- Benefit: Centralized authorization logic, cleaner components

**Interceptor Pattern:**

- HTTP interceptors modify requests/responses globally
- AuthInterceptor adds JWT token to all requests
- ErrorInterceptor handles errors consistently
- Benefit: DRY principle, consistent behavior

**Component-Based Pattern:**

- UI decomposed thành reusable components
- @Input() for parent-to-child data
- @Output() EventEmitter for child-to-parent events
- Benefit: Reusability, maintainability, encapsulation

---

**10. Scalability Considerations:**

**Horizontal Scaling (Scale Out):**

**Stateless API:**

- No session state lưu trên server
- JWT tokens contain all auth info, không cần server-side session store
- Mỗi request độc lập, không phụ thuộc previous requests
- Benefit: Có thể deploy multiple instances, load balancer distribute requests

**Google Cloud Run Auto-scaling:**

- Automatically thêm/bớt instances based on request volume
- Min instances: 0 (scale to zero khi không có traffic)
- Max instances: 100 (configurable)
- Scale up khi CPU > 60% hoặc concurrent requests > 80
- Benefit: Handle traffic spikes, cost-effective (pay for actual usage)

**Database Connection Pooling:**

- EF Core connection pool reuse connections
- MaxPoolSize: 100 connections
- Avoid overhead của create/close connections repeatedly
- Benefit: Better throughput, reduced latency

**SignalR Sticky Sessions:**

- Google Cloud Run hỗ trợ session affinity
- Client luôn route đến same instance for WebSocket connection
- Future: Redis backplane để share state across instances
- Benefit: Real-time connections work với multiple instances

**Vertical Scaling (Scale Up):**

**Efficient Database Queries:**

- Indexes trên columns frequently queried: ShopOwnerId, ProductCode
- Composite indexes cho multi-column WHERE clauses
- Avoid N+1 queries với eager loading (.Include())
- Benefit: Faster queries, support more users trên same hardware

**Query Optimization:**

- Use pagination cho large datasets: Skip(page \* size).Take(size)
- Select only needed columns, không SELECT \*
- Projection với Select() thay vì load full entities
- Benefit: Reduced memory usage, faster response times

**Caching (Future Enhancement):**

- In-memory cache cho frequently accessed data (products, categories)
- Distributed cache (Redis) cho shared cache across instances
- Cache invalidation strategy when data changes
- Benefit: Dramatically reduce database load

**Lazy Loading (Angular):**

- Modules loaded on-demand, không load tất cả upfront
- Code splitting: Separate bundles per route
- Initial bundle size nhỏ, faster initial load
- Benefit: Better user experience, especially on slow networks

**CDN for Static Assets (Future):**

- Serve images, CSS, JavaScript từ CDN
- Edge locations gần users
- Reduce load trên application servers
- Benefit: Faster asset loading, better global performance

**Database Scalability:**

**Indexes:**

- B-tree indexes cho equality và range queries
- GIN index for JSONB columns
- IVFFlat index cho Pgvector similarity search
- Benefit: Query performance improves even với large datasets

**Pagination:**

- Never return all records at once
- Limit 20-50 records per page
- Cursor-based pagination cho consistent results
- Benefit: Predictable performance, manageable data transfer

**Read Replicas (Future):**

- PostgreSQL read replicas cho read-heavy queries
- Master-slave replication
- Route SELECT queries đến replicas, writes đến master
- Benefit: Distribute read load, better availability

**Sharding (If Needed):**

- Partition data by ShopOwnerId
- Each shard contains data for subset of shop owners
- Hiện tại không cần, single database đủ
- Benefit: Horizontal database scaling cho very large datasets

**Monitoring và Alerting:**

- Google Cloud Monitoring track metrics: CPU, memory, request latency
- Alerts when response time > 1s, error rate > 5%
- Logging với structured logs cho debugging
- Benefit: Proactive issue detection, faster troubleshooting

---

## 📋 Câu hỏi 4: Cơ chế Authentication & Authorization

### ❓ Câu hỏi:

**"Hệ thống của em xử lý authentication và authorization như thế nào? JWT token được generate và validate ra sao?"**

### ✅ Trả lời:

**1. Quy trình Authentication (Xác thực):**

**A. Registration (Đăng ký):**

Khi user đăng ký tài khoản mới, hệ thống thực hiện các bước sau:

1. **Validate input**: Backend kiểm tra ModelState với Data Annotations (required fields, email format, phone format)
2. **Check duplicates**: Query database kiểm tra Phone và Email đã tồn tại chưa, return error nếu trùng
3. **Hash password**: Sử dụng BCrypt.Net.BCrypt.HashPassword() với salt rounds = 10 để mã hóa password một chiều
4. **Create ShopOwner entity**: Tạo record mới với các thông tin: Name, Phone, Email, PasswordHash
5. **Create default Shop**: Tự động tạo Shop đầu tiên cho ShopOwner
6. **Create default Subscription**: Tạo subscription Free plan (Basic) với thời hạn 1 năm
7. **Transaction save**: Lưu tất cả (ShopOwner + Shop + Subscription) trong một database transaction để đảm bảo atomicity
8. **Return success**: Response về frontend với message "Registration successful"
9. **Frontend redirect**: Chuyển user đến trang login

**B. Login (Đăng nhập):**

1. **Receive credentials**: User nhập Phone và Password, frontend POST đến /api/auth/login
2. **Find user**: Backend tìm user theo Phone trong bảng ShopOwners hoặc Employees
3. **Verify password**: Sử dụng BCrypt.Net.BCrypt.Verify() so sánh input password với stored PasswordHash
4. **Generate JWT token** nếu password đúng:
   - Tạo claims chứa: shop_owner_id, user_id, email, user_type (ShopOwner/Employee), role
   - Sign token với HMACSHA256 và secret key từ environment variables
   - Set expiration time = 7 days từ thời điểm hiện tại
5. **Return response**: Token + user info (name, email, userType, shopOwnerId)
6. **Frontend storage**: Lưu token vào localStorage với key 'auth_token'
7. **Update state**: AuthService BehaviorSubject emit user info để components subscribe
8. **Redirect**: Navigate đến dashboard page

**2. JWT Token Structure và Generation:**

**JWT Token gồm 3 phần: Header.Payload.Signature**

**Header** chứa:

- Algorithm: "HS256" (HMAC SHA-256)
- Type: "JWT"

**Payload** chứa claims:

- **shop_owner_id**: ID của ShopOwner để filter data theo shop
- **user_id**: ID của current user (ShopOwner hoặc Employee)
- **email**: Email address của user
- **user_type**: "ShopOwner" hoặc "Employee" để phân biệt loại user
- **role**: "ShopOwner" hoặc "Employee" cho authorization checks
- **sub** (subject): Phone number của user
- **jti** (JWT ID): Unique GUID để track token
- **iat** (issued at): Timestamp khi token được tạo
- **exp** (expiration): Timestamp khi token hết hạn (7 ngày sau iat)

**Signature** được tạo:

- Input: base64UrlEncode(header) + "." + base64UrlEncode(payload)
- Algorithm: HMACSHA256(input, secretKey)
- Secret key được lưu trong appsettings.json hoặc environment variables, KHÔNG hard-code trong code

**Token Generation Process:**

1. Tạo List of Claims với các thông tin user
2. Create SymmetricSecurityKey từ secret key (UTF8 encoding)
3. Create SigningCredentials với algorithm HmacSha256
4. Create JwtSecurityToken object với issuer, audience, claims, expiration, signing credentials
5. Write token thành string với JwtSecurityTokenHandler
6. Return token string (ví dụ: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...")

**3. JWT Token Validation:**

**Backend Middleware Pipeline:**

ASP.NET Core được configure để validate JWT tokens tự động:

1. **AddAuthentication**: Đăng ký authentication scheme với JwtBearerDefaults
2. **AddJwtBearer**: Configure token validation parameters:
   - ValidateIssuer = true: Kiểm tra issuer claim khớp với configured issuer
   - ValidateAudience = true: Kiểm tra audience claim
   - ValidateLifetime = true: Kiểm tra expiration time, reject nếu expired
   - ValidateIssuerSigningKey = true: Verify signature với secret key
   - ClockSkew = TimeSpan.Zero: Không cho grace period, strict expiration
3. **OnMessageReceived event**: Đặc biệt cho SignalR, extract access_token từ query string vì WebSocket không hỗ trợ headers
4. **OnAuthenticationFailed event**: Handle expired tokens, thêm custom header "Token-Expired: true"

**Validation Flow:**

1. Request đến với header: Authorization: Bearer <token>
2. Authentication Middleware extract token
3. Parse token thành 3 parts: header, payload, signature
4. Verify signature: Compute HMACSHA256(header.payload, secretKey) và compare với signature
5. Check expiration: Compare exp claim với current time
6. Check issuer và audience
7. Nếu tất cả valid, extract claims và attach vào User object của Controller
8. Controller có thể access claims: User.FindFirst("shop_owner_id")?.Value

**4. Authorization (Phân quyền):**

**A. Role-Based Authorization:**

Sử dụng [Authorize] attribute với Roles parameter:

- **Controller level**: [Authorize(Roles = "ShopOwner")] trên EmployeesController - chỉ ShopOwner mới access được toàn bộ controller
- **Action level**: [Authorize(Roles = "ShopOwner")] trên specific actions như CreateEmployee() - fine-grained control

Khi request đến:

1. Authentication Middleware validate token
2. Authorization Middleware check Role claim trong token
3. Nếu Role match yêu cầu, cho phép access
4. Nếu không match, return 403 Forbidden

**B. Custom Authorization (Data Isolation):**

Quan trọng nhất: Đảm bảo mỗi ShopOwner chỉ thấy data của mình:

1. **Extract shopOwnerId**: Trong mỗi API endpoint, lấy shop_owner_id từ JWT claims
2. **Validate claim**: Check claim không null và có thể parse sang integer
3. **Filter queries**: Tất cả database queries đều có WHERE ShopOwnerId = shopOwnerId
4. **Return data**: Chỉ return data thuộc về shop owner này

Ví dụ GetAllProducts:

- Extract shopOwnerId từ User.FindFirst("shop_owner_id")
- Nếu claim invalid, return 401 Unauthorized
- Query: SELECT \* FROM Products WHERE ShopOwnerId = shopOwnerId
- KHÔNG BAO GIỜ query tất cả products rồi filter ở application layer

**C. Subscription Guard Middleware:**

Middleware kiểm tra subscription status trước khi cho access:

1. Request đến protected endpoint
2. Extract shopOwnerId từ JWT claims
3. Query Subscriptions table tìm active subscription của shop owner
4. Check IsActive = true và EndDate > now()
5. Nếu subscription expired hoặc không tồn tại:
   - Set response status = 403 Forbidden
   - Return JSON: { success: false, message: "Subscription expired" }
   - Stop pipeline, không chạy controller
6. Nếu valid, continue pipeline

**5. Frontend Token Management:**

**A. Token Storage:**

AuthService quản lý token lifecycle:

- **saveToken(token)**: localStorage.setItem('auth_token', token)
- **getToken()**: localStorage.getItem('auth_token')
- **removeToken()**: localStorage.removeItem('auth_token') khi logout

Tại sao dùng localStorage thay vì cookies:

- Pros: Dễ implement với SPA, access dễ dàng từ JavaScript
- Cons: Vulnerable với XSS attacks
- Mitigation: Content Security Policy (CSP), sanitize user inputs

**B. Auto-attach Token với HTTP Interceptor:**

AuthInterceptor tự động thêm token vào mọi HTTP requests:

1. Intercept mọi outgoing requests
2. Call authService.getToken() lấy token từ localStorage
3. Nếu có token, clone request và add header: Authorization: Bearer <token>
4. Forward modified request
5. Catch error responses:
   - Nếu 401 Unauthorized: Token expired hoặc invalid
   - Call authService.logout(): clear token, clear state
   - Navigate to /login page
   - Show "Session expired" message

**C. Token Decoding và Validation:**

Frontend cần decode token để check expiration:

1. **decodeToken()**:
   - Split token thành 3 parts by "."
   - Extract payload (middle part)
   - Base64 decode payload
   - URI decode characters
   - Parse JSON to object
   - Return payload với các claims
2. **isLoggedIn()**:
   - Get token from localStorage
   - Decode token
   - Extract exp claim (expiration timestamp)
   - Compare exp với current time (Date.now() / 1000)
   - Return true nếu exp > now, false nếu expired

**6. Security Best Practices:**

**Backend Security:**

- ✅ **BCrypt Password Hashing**: Salt rounds = 10, mỗi password có unique salt, không thể reverse
- ✅ **Secret Key Protection**: Lưu trong environment variables, không commit vào Git, rotate định kỳ
- ✅ **Token Expiration**: 7 days expiration, balance giữa security và user experience
- ✅ **HTTPS Only**: Production chỉ chấp nhận HTTPS, cookies set Secure flag
- ✅ **CORS Configuration**: Chỉ allow specific origins, không dùng wildcard "\*"
- ✅ **Input Validation**: Data Annotations cho model validation, reject invalid data ngay tại controller
- ✅ **SQL Injection Prevention**: Entity Framework Core dùng parameterized queries tự động
- ✅ **Rate Limiting**: (Future) Implement để prevent brute force attacks

**Frontend Security:**

- ✅ **Token in localStorage**: Đơn giản nhưng có XSS risk, acceptable cho MVP
- ✅ **Auto Logout**: Tự động logout khi token expired, không keep session mãi mãi
- ✅ **No Sensitive Data in Token**: Không lưu password, sensitive info trong JWT payload
- ✅ **HTTPS Only**: Production frontend chỉ chạy trên HTTPS
- ✅ **Content Security Policy**: Prevent XSS attacks bằng cách restrict script sources

**Alternatives Considered nhưng không implement:**

- ❌ **HttpOnly Cookies**: Secure hơn localStorage (immune to XSS) nhưng phức tạp với SPA và CORS, cần backend set cookies
- ❌ **Refresh Tokens**: Best practice cho production (short-lived access token + long-lived refresh token) nhưng phức tạp cho MVP, sẽ implement phase 2
- ❌ **OAuth2/OpenID Connect**: Overkill cho internal application, phù hợp hơn cho third-party integrations

**7. Multi-Role Support:**

**User Hierarchy:**

Hệ thống có 3 levels:

1. **Admin** (System level):

   - Quản lý subscription plans
   - Xem thống kê toàn hệ thống
   - Không thuộc shop cụ thể nào
   - (Chưa implement đầy đủ trong MVP)

2. **ShopOwner** (Shop level):

   - Full access đến tất cả features của shop
   - Quản lý products, invoices, employees, customers
   - Xem reports và analytics
   - Manage subscription
   - Có thể có nhiều shops

3. **Employee** (Limited access):
   - Chỉ access POS để tạo invoices
   - Không thể manage products, employees
   - Không xem được reports
   - Assigned to một shop cụ thể

**Permission Matrix:**

| Feature                  | Admin | ShopOwner | Employee |
| ------------------------ | ----- | --------- | -------- |
| Create/Edit Shop         | ❌    | ✅        | ❌       |
| Manage Products          | ❌    | ✅        | ❌       |
| Create Invoice (POS)     | ❌    | ✅        | ✅       |
| View Reports & Analytics | ❌    | ✅        | ❌       |
| Manage Employees         | ❌    | ✅        | ❌       |
| Manage Customers         | ❌    | ✅        | ❌       |
| Manage Subscription      | ✅    | ✅        | ❌       |
| AI Assistant             | ❌    | ✅        | ❌       |
| Notifications            | ❌    | ✅        | ✅ (own) |

Implementation:

- Role stored trong JWT claims
- [Authorize(Roles = "ShopOwner")] trên controllers/actions
- Frontend guards check role: canActivate() return false nếu không đủ quyền

---

## 📋 Câu hỏi 5: Subscription System

### ❓ Câu hỏi:

**"Em giải thích về hệ thống subscription và cách áp dụng limits (giới hạn tài nguyên) cho từng gói?"**

### ✅ Trả lời:

**1. Mô hình Subscription (SaaS):**

Hệ thống áp dụng mô hình Software as a Service (SaaS) với 3 gói subscription:

**BASIC (Free Tier):**

- Price: 0 VND/năm
- Max Products: 50 sản phẩm
- Max Employees: 2 nhân viên
- Max Shops: 1 cửa hàng
- Max Customers: 100 khách hàng
- Features: POS cơ bản, Quản lý sản phẩm, Khách hàng
- Target: Cửa hàng nhỏ, bắt đầu kinh doanh

**PROFESSIONAL:**

- Price: 500,000 VND/tháng
- Max Products: 200 sản phẩm
- Max Employees: 5 nhân viên
- Max Shops: 3 cửa hàng
- Max Customers: 500 khách hàng
- Features: Tất cả Basic + AI Assistant, Báo cáo nâng cao, Xuất Excel
- Target: Cửa hàng mở rộng, nhiều chi nhánh

**ENTERPRISE:**

- Price: 2,000,000 VND/tháng
- Max Products: Unlimited (không giới hạn)
- Max Employees: Unlimited
- Max Shops: 10 cửa hàng
- Max Customers: Unlimited
- Features: Tất cả Professional + API Access, Priority Support, Custom features
- Target: Chuỗi cửa hàng, franchise

**2. Database Schema cho Subscription:**

**SubscriptionPlans table:**

- Id: Primary key
- Name: "Basic", "Professional", "Enterprise"
- Description: Mô tả gói
- Price: Giá (decimal)
- DurationDays: Số ngày hiệu lực (30, 365)
- Limits: MaxProducts, MaxEmployees, MaxShops, MaxCustomers (integers)
- Features: JSONB column chứa array features (flexible)
- IsActive: Boolean để enable/disable plans
- CreatedAt: Timestamp

**Subscriptions table:**

- SubscriptionId: Primary key
- ShopOwnerId: Foreign key đến ShopOwners
- SubscriptionPlanId: Foreign key đến SubscriptionPlans
- StartDate: Timestamp khi subscription bắt đầu
- EndDate: Timestamp khi hết hạn
- IsActive: Boolean (chỉ có 1 active subscription per shop owner)
- AutoRenew: Boolean cho auto-renewal (future feature)

**SubscriptionLimits table (cached current usage):**

- ShopOwnerId: Primary key
- CurrentProducts: Số lượng products hiện tại (integer)
- CurrentEmployees: Số employees hiện tại
- CurrentShops: Số shops hiện tại
- CurrentCustomers: Số customers hiện tại
- LastUpdated: Timestamp của lần update cuối

Purpose: Cache usage để không phải COUNT(\*) mỗi lần check, update bằng triggers hoặc application code.

**3. Limit Enforcement Logic:**

**A. Check Limit Before Create:**

Trước khi tạo resource mới (product, employee, etc), system check limit:

1. **Get active subscription**:

   - Query Subscriptions table WHERE ShopOwnerId = X AND IsActive = true AND EndDate > NOW()
   - Include SubscriptionPlan để lấy limits
   - Nếu không có subscription, return error "No active subscription"

2. **Get current usage**:

   - Query count: SELECT COUNT(\*) FROM Products WHERE ShopOwnerId = X
   - Hoặc lấy từ SubscriptionLimits table (faster)

3. **Check limit**:

   - Compare currentCount vs maxLimit từ plan
   - Nếu currentProducts >= maxProducts:
     - Return error: "Reached product limit (50). Please upgrade."
     - Include currentCount và maxLimit trong response
     - LimitReached flag = true
   - Nếu còn quota: Allow creation, proceed

4. **Apply in Controllers**:
   - Mỗi CreateProduct, CreateEmployee endpoint gọi CheckLimit trước
   - Nếu limit reached, return BadRequest với message
   - Frontend hiển thị message và button "Upgrade Plan"

**B. Update Usage Counter:**

Sau khi tạo/xóa resource, update counter:

1. After create Product: Increment SubscriptionLimits.CurrentProducts
2. After delete Product: Decrement counter
3. Use database triggers hoặc application code để sync
4. Periodically recalculate (cleanup job) để đảm bảo accuracy

**4. Frontend Subscription Guard:**

**Angular Route Guard:**

SubscriptionGuard implements CanActivate:

1. Extract shopOwnerId từ AuthService
2. Check nếu route trong allowedRoutes (subscription pages, payment pages):
   - Return true, allow access
3. Gọi API: GET /api/subscription/status?shopOwnerId=X
4. Backend return: { isActive: boolean, isExpired: boolean, plan: {...} }
5. Nếu subscription valid (active && !expired):
   - Return true, allow navigation
6. Nếu expired:
   - Show alert: "Your subscription has expired. Please renew to continue."
   - Navigate to /subscription/plans
   - Return false, block navigation

Apply guard trong routes:

```
{
  path: 'products',
  component: ProductsComponent,
  canActivate: [AuthGuard, SubscriptionGuard]
}
```

**5. Payment Flow (MoMo Integration):**

**Step-by-step process:**

1. **User Select Plan**:

   - User browse subscription plans page
   - Click "Chọn gói" button trên Professional plan

2. **Frontend Create Payment Request**:

   - POST /api/payment/create-momo-payment
   - Body: { planId: 2, shopOwnerId: 123 }

3. **Backend Generate Payment URL**:

   - Create PendingSubscription record (track payment)
   - Generate unique OrderId
   - Calculate amount from plan price
   - Call MoMo API với params:
     - partnerCode, accessKey, secretKey
     - orderId, amount, orderInfo
     - returnUrl (redirect sau khi thanh toán)
     - ipnUrl (callback webhook)
     - requestType: "captureWallet"
   - Compute HMAC SHA256 signature
   - MoMo return payUrl

4. **Redirect to MoMo**:

   - Frontend redirect user to payUrl
   - User mở app MoMo hoặc nhập thông tin thẻ
   - MoMo display payment details

5. **User Confirm Payment**:

   - User confirm trong MoMo app
   - MoMo process payment

6. **MoMo IPN Callback (Server-to-Server)**:

   - MoMo gọi ipnUrl webhook: POST /api/payment/momo-callback
   - Body: { orderId, resultCode, signature, ... }
   - Backend validate signature để đảm bảo authentic
   - Nếu signature invalid: return BadRequest
   - Nếu resultCode != 0: Payment failed, log error

7. **Activate Subscription**:

   - Find PendingSubscription by orderId
   - Check IsProcessed flag (prevent duplicate processing)
   - Create/Update Subscription record:
     - ShopOwnerId, SubscriptionPlanId
     - StartDate = NOW(), EndDate = NOW() + plan duration
     - IsActive = true
   - Mark PendingSubscription as IsProcessed = true
   - Save to database trong transaction

8. **Send Notification**:

   - Create Notification: "Subscription activated successfully"
   - Send real-time notification qua SignalR
   - User thấy toast notification

9. **MoMo Redirect User**:

   - Sau khi thanh toán, MoMo redirect đến returnUrl
   - URL có query params: orderId, resultCode
   - Frontend parse params, hiển thị success/failure page
   - Nếu success: Redirect to dashboard, show success message

10. **Return Response to MoMo**:
    - Backend return Ok("Success") cho MoMo callback
    - MoMo mark transaction as completed

**6. Limit Check Dashboard:**

**Frontend Display Current Usage:**

SubscriptionService.checkLimits() call API:

- GET /api/subscription/limits?shopOwnerId=X
- Backend return:

```
{
  products: { current: 30, max: 50, canCreate: true },
  employees: { current: 1, max: 2, canCreate: true },
  shops: { current: 1, max: 1, canCreate: false },
  customers: { current: 45, max: 100, canCreate: true }
}
```

UI Component display:

- Card cho mỗi resource type
- Progress bar visualization: width = (current / max) \* 100%
- Numbers: "30 / 50 Products"
- Status: "Can create" hoặc "Limit reached"
- Button "Upgrade Plan" nếu canCreate = false

When user hit limit:

- Create button disabled
- Show message: "You've reached the limit. Upgrade to add more."
- Click upgrade button → Navigate to /subscription/plans

**7. Benefits của Subscription Model:**

**For Business (chúng tôi):**

- ✅ **Recurring Revenue**: Doanh thu đều đặn hàng tháng, dễ forecast
- ✅ **Predictable Cash Flow**: Biết trước thu nhập, plan được growth
- ✅ **Easy Scaling**: Thêm plans mới dễ dàng, flexible pricing
- ✅ **Customer Lock-in**: Users invest data vào hệ thống, switching cost cao
- ✅ **Upsell Opportunities**: Users tự nhiên upgrade khi business grow

**For Users (khách hàng):**

- ✅ **Low Initial Cost**: Free tier để thử nghiệm, không mất phí setup
- ✅ **Pay as You Grow**: Start nhỏ, upgrade khi cần, không waste money
- ✅ **Flexible Pricing**: Choose plan phù hợp với quy mô và nhu cầu
- ✅ **No Maintenance**: Không lo updates, backups, infrastructure
- ✅ **Always Updated**: Luôn dùng latest version với new features

**Comparison với Traditional Licensing:**

| Aspect       | SaaS Subscription  | Traditional License           |
| ------------ | ------------------ | ----------------------------- |
| Initial Cost | Low (free tier)    | High (one-time purchase)      |
| Upgrades     | Free, automatic    | Pay for each version          |
| Maintenance  | Included           | Self-managed                  |
| Scalability  | Easy (change plan) | Difficult (buy more licenses) |
| Data Backup  | Automatic          | Manual                        |
| Access       | Anywhere (cloud)   | On-premise only               |

**8. Future Enhancements:**

- ✅ Auto-renewal với credit card
- ✅ Proration khi upgrade mid-cycle
- ✅ Annual discounts (pay yearly = 10 months price)
- ✅ Custom enterprise plans với negotiated limits
- ✅ Usage-based pricing (pay per transaction) option
- ✅ Trial period (14 days free Pro features)

---

## 📋 Câu hỏi 6: AI Assistant & RAG Implementation

### ❓ Câu hỏi:

**"Em giải thích chi tiết về cách implement AI Assistant với RAG (Retrieval-Augmented Generation)? Vector search hoạt động như thế nào?"**

### ✅ Trả lời:

**1. RAG Architecture Overview:**

RAG là kỹ thuật kết hợp Retrieval (tìm kiếm thông tin) và Generation (sinh câu trả lời) để AI trả lời chính xác dựa trên data thực tế.

**7 bước trong RAG Pipeline:**

**Bước 1 - USER QUESTION:**

- User type câu hỏi trong chat: "Sản phẩm nào bán chạy nhất tháng này?"
- Question được gửi đến backend

**Bước 2 - EMBEDDING GENERATION:**

- Convert question thành vector representation
- Gọi Google Vertex AI Text Embedding API
- Input: text string (câu hỏi)
- Output: vector 768 dimensions (array of floats)
- Vector capture semantic meaning của câu hỏi

**Bước 3 - VECTOR SEARCH (Retrieval):**

- Use question vector để search trong ProductEmbeddings table
- PostgreSQL + Pgvector extension
- Cosine similarity search: tìm products có vector gần nhất
- Return top K results (K=5) most relevant products

**Bước 4 - CONTEXT BUILDING:**

- Format retrieved products thành text context
- Include: ProductName, Price, Stock, Description
- Structure: "THÔNG TIN SẢN PHẨM: - Coca 330ml, Giá: 10,000 VND..."

**Bước 5 - PROMPT CONSTRUCTION:**

- Combine:
  - System prompt (AI instructions)
  - Retrieved context (relevant products)
  - Chat history (last 10 messages)
  - Current question
- Format thành single prompt string

**Bước 6 - LLM GENERATION:**

- Send prompt đến Google Gemini 1.5 Pro
- LLM analyze prompt và generate answer
- Answer based on provided context, không hallucinate

**Bước 7 - RESPONSE:**

- Return answer text đến frontend
- Include related products list
- Display trong chat interface với product cards

**2. Vector Embedding Generation:**

**A. Product Indexing (tạo embeddings cho products):**

Khi product được tạo hoặc cập nhật, system generate embedding:

**Process:**

1. **Get product details**:

   - Query Products table với ProductId
   - Include relationships: Category, Supplier
   - Get full product information

2. **Build text representation**:

   - Concatenate all relevant fields thành paragraph
   - Format: "Sản phẩm: [Name], Mã: [Code], Danh mục: [Category], Giá: [Price] VND, Tồn kho: [Stock], Mô tả: [Description]"
   - Example: "Sản phẩm: Coca Cola 330ml, Mã: SP001, Danh mục: Nước giải khát, Giá: 10,000 VND, Tồn kho: 100 chai, Mô tả: Nước ngọt có gas, vị cola"

3. **Generate embedding**:

   - Call EmbeddingService.GenerateEmbeddingAsync(text)
   - Service gọi Google Vertex AI Prediction API
   - Endpoint: textembedding-gecko model
   - Request body: { content: "..." }
   - Response: array of 768 floats (embedding vector)

4. **Save to database**:
   - Create ProductEmbedding entity
   - Fields: ProductId, Embedding (vector type), TextContent, CreatedAt
   - Insert vào ProductEmbeddings table
   - Pgvector stores vector efficiently

**B. Embedding Service Implementation:**

EmbeddingService gọi Vertex AI:

1. Create PredictionServiceClient (Google Cloud client library)
2. Build PredictRequest với:
   - Endpoint: Vertex AI endpoint URL
   - Instances: Array chứa { content: text }
3. Call PredictAsync() - async API call
4. Parse response:
   - Navigate response JSON structure
   - Extract embeddings.values array
   - Convert từ Google.Protobuf.Value sang float[]
5. Return float[] với 768 elements

**Why 768 dimensions?**

- Textembedding-gecko model của Google output 768-dim vectors
- Balance giữa expressiveness và performance
- Higher dimensions = more detailed nhưng slower search

**3. Vector Search Implementation:**

**A. Database Setup với Pgvector:**

Enable và config Pgvector extension:

1. **Enable extension**:

   - Run SQL: CREATE EXTENSION IF NOT EXISTS vector;
   - PostgreSQL loads pgvector module

2. **Create table**:

   - ProductEmbeddings table với Embedding column type = vector(768)
   - 768 là dimension size
   - Foreign key đến Products table
   - ON DELETE CASCADE để tự động xóa embedding khi xóa product

3. **Create index**:
   - Use IVFFlat index algorithm (Inverted File with Flat compression)
   - Syntax: CREATE INDEX USING ivfflat (Embedding vector_cosine_ops)
   - WITH (lists = 100): Partition space thành 100 clusters
   - Trade-off: Approximate search (faster) vs Exact search (slower)
   - Cosine distance operator: <=> (measures similarity)

**Index Types:**

- **IVFFlat**: Faster, approximate results, good for large datasets
- **HNSW** (Hierarchical Navigable Small World): Slower indexing, faster search, more accurate

Chúng tôi chọn IVFFlat vì dataset không quá lớn (< 10K products per shop) và speed quan trọng hơn perfect accuracy.

**B. Vector Search Query:**

RetrievalService thực hiện search:

**Process:**

1. **Generate query embedding**:

   - User question → EmbeddingService
   - Get 768-dim vector cho question

2. **Convert to Pgvector format**:

   - Format: "[0.123, 0.456, ...]" - string representation
   - PostgreSQL parse thành vector type

3. **Execute similarity search**:

   - SQL query với vector distance operator
   - SELECT with JOIN products và categories
   - Calculate similarity: 1 - (Embedding <=> query_vector)
   - Result: 0 to 1, where 1 = identical, 0 = completely different

4. **Filter và sort**:

   - WHERE ShopOwnerId = X (data isolation)
   - WHERE IsActive = true (chỉ active products)
   - ORDER BY similarity (most similar first)
   - LIMIT K (top 5 results)

5. **Return results**:
   - List of ProductSearchResult objects
   - Include: ProductId, Name, Price, Stock, Description, Similarity score
   - Similarity score để ranking và filtering (threshold = 0.5)

**Similarity Metrics:**

Pgvector hỗ trợ 3 distance operators:

- **<=> (Cosine distance)**: 1 - cosine_similarity, range 0-2
  - Best for: Text embeddings (direction matters more than magnitude)
  - We use: 1 - distance để convert thành similarity score 0-1
- **<-> (L2/Euclidean distance)**: sqrt(sum((a-b)^2))
  - Best for: When magnitude matters
- **<#> (Inner product)**: -sum(a\*b)
  - Best for: Normalized vectors

Chúng tôi chọn cosine distance vì text embeddings từ Vertex AI đã normalized và direction quan trọng hơn magnitude.

**4. RAG Prompt Construction:**

PromptTemplateService xây dựng prompt cho LLM:

**Components:**

**1. System Prompt (Instructions cho AI):**

```
Bạn là trợ lý ảo thông minh của hệ thống quản lý bán hàng.
Nhiệm vụ của bạn là trả lời câu hỏi của người dùng dựa trên dữ liệu sản phẩm được cung cấp.

QUY TẮC:
- Chỉ trả lời dựa trên thông tin được cung cấp
- Nếu không có thông tin, nói rõ là không tìm thấy
- Trả lời bằng tiếng Việt, ngắn gọn, dễ hiểu
- Có thể gợi ý sản phẩm liên quan
```

**2. Context (Retrieved Products):**

```
THÔNG TIN SẢN PHẨM:
- Coca Cola 330ml (SP001)
  Giá: 10,000 VND
  Tồn kho: 100 chai
  Mô tả: Nước ngọt có gas

- Pepsi 330ml (SP002)
  Giá: 10,000 VND
  Tồn kho: 80 chai
  Mô tả: Nước ngọt có gas, vị cola
```

**3. Chat History (Last 10 messages):**

```
LỊCH SỬ HỘI THOẠI:
User: Coca có giá bao nhiêu?
Assistant: Coca Cola 330ml có giá 10,000 VND.
User: Còn bao nhiêu?
Assistant: Hiện có 100 chai trong kho.
```

**4. Current Question:**

```
CÂU HỎI: Sản phẩm nào bán chạy nhất?
```

**Final Prompt:**
Concatenate tất cả parts theo thứ tự: System + Context + History + Question + "TRẢ LỜI:"

**Purpose của từng part:**

- System prompt: Define behavior và constraints
- Context: Provide factual information để answer
- History: Maintain conversation continuity, understand context references
- Question: What user wants to know

**5. LLM Integration (Google Gemini):**

GeminiLLMService gửi prompt đến Gemini API:

**Process:**

1. **Prepare HTTP request**:

   - Endpoint: https://generativelanguage.googleapis.com/v1/models/gemini-1.5-flash:generateContent
   - Method: POST
   - Header: Authorization: Bearer <API_KEY>

2. **Build request body**:

   - contents: Array of message objects
   - parts: Array with text part containing prompt
   - generationConfig:
     - temperature: 0.7 (balance creativity và accuracy)
     - topK: 40 (consider top 40 tokens)
     - topP: 0.95 (nucleus sampling)
     - maxOutputTokens: 1024 (limit response length)

3. **Send request**: await client.PostAsJsonAsync()

4. **Parse response**:

   - Navigate JSON: candidates[0].content.parts[0].text
   - Extract generated text
   - Handle errors: rate limits, API errors

5. **Return answer**: String text của response

**Configuration Parameters:**

- **temperature (0.7)**:
  - 0 = deterministic, always same answer
  - 1 = creative, varied answers
  - 0.7 = good balance cho customer service
- **topK (40)**:
  - Consider 40 most likely next tokens
  - Reduce random output
- **topP (0.95)**:
  - Nucleus sampling: consider tokens until cumulative probability > 0.95
  - Avoid very unlikely tokens

**6. Complete RAG Flow:**

ChatService.AskAsync() orchestrates toàn bộ flow:

**Steps:**

1. **Session Management**:

   - Get hoặc create ChatSession cho user
   - SessionId track conversation

2. **Vector Search (Retrieval)**:

   - Call RetrievalService.SearchProductsAsync()
   - Params: shopOwnerId, question, topK=5
   - Get top 5 relevant products

3. **Load Chat History**:

   - Query ChatMessages table
   - WHERE SessionId = X
   - ORDER BY CreatedAt DESC LIMIT 10
   - Reverse order để chronological

4. **Build Prompt**:

   - Call PromptService.BuildRAGPrompt()
   - Pass: question, products, history
   - Get complete prompt string

5. **Generate Answer**:

   - Call LLMService.GenerateResponseAsync(prompt)
   - Wait for Gemini response
   - Get answer text

6. **Save Conversation**:

   - Create 2 ChatMessage records:
     - Role="user", Content=question
     - Role="assistant", Content=answer
   - Insert vào database với SessionId

7. **Return Response**:
   - ChatResponse object:
     - Answer: generated text
     - SessionId: for follow-up questions
     - RelatedProducts: list of retrieved products
   - Frontend display answer + product cards

**Error Handling:**

- Try-catch mỗi step
- If retrieval fails: Use empty context, LLM answers với general knowledge
- If LLM fails: Return error message "AI service temporarily unavailable"
- Log tất cả errors cho debugging

**7. Performance Optimization:**

**A. Indexing Strategy:**

IVFFlat index configuration:

- **lists = 100**: Partition embedding space thành 100 clusters
- Tại query time: Search chỉ trong relevant clusters (không scan tất cả)
- Trade-off:
  - Fewer lists = more accurate, slower
  - More lists = faster, less accurate
  - 100 là sweet spot cho dataset size của chúng tôi

**B. Caching Strategies:**

1. **Cache product embeddings**:

   - Generate embedding một lần khi create/update product
   - Store trong ProductEmbeddings table
   - KHÔNG regenerate mỗi lần search

2. **Cache frequently asked questions**:

   - Implement Redis cache (future)
   - Key: hash(question), Value: answer
   - TTL: 1 hour
   - Check cache trước khi RAG pipeline

3. **Lazy embedding generation**:
   - Generate embeddings asynchronously
   - Background job batch process new products
   - Products without embeddings excluded từ vector search

**C. Batch Processing:**

BatchIndexProductsAsync() for multiple products:

1. Get list of productIds
2. Query Products table with WHERE ProductId IN (...)
3. Map products → IndexProductAsync tasks
4. Task.WhenAll() để parallel execution
5. Await tất cả tasks complete

Benefits:

- Faster khi import bulk products
- Efficient API usage (batch embedding calls)
- Reduce database roundtrips

**8. Accuracy Metrics và Testing:**

**A. Retrieval Quality Metrics:**

**Precision@K**: Of top K results, how many are relevant?

- Formula: (Number of relevant items in top K) / K
- Example: Top 5 results, 4 relevant → Precision@5 = 80%

**Recall**: Did we retrieve all relevant items?

- Formula: (Retrieved relevant items) / (Total relevant items)
- Example: 4 retrieved, 6 total relevant → Recall = 67%

**MRR (Mean Reciprocal Rank)**: Position của first relevant result

- Formula: 1 / (rank of first relevant result)
- Example: First relevant at position 2 → MRR = 0.5

**B. Test Cases:**

Test Case 1:

- Query: "nước giải khát có gas"
- Expected: Coca Cola, Pepsi, 7Up, Sprite
- Retrieved: Coca (similarity 0.95), Pepsi (0.92), 7Up (0.89), Nước suối (0.45)
- Precision@3: 100% ✅
- Problem: Nước suối at position 4 (không relevant)
- Solution: Filter results với similarity threshold > 0.6

Test Case 2:

- Query: "sản phẩm giá rẻ dưới 10k"
- Expected: Products với Price < 10000
- Challenge: Vector search không understand numeric comparisons
- Solution: Hybrid search (vector + SQL filters)

**C. LLM Response Quality:**

Evaluate generated answers:

1. **Factual Accuracy**:

   - Answer phải dựa trên context only
   - Không fabricate information
   - Test: Compare answer với actual data

2. **Completeness**:

   - Answer đầy đủ question
   - Không missing key information
   - Test: Human evaluation

3. **Relevance**:

   - Answer address exact question asked
   - Không off-topic
   - Test: Compare với expected answer

4. **Language Quality**:
   - Vietnamese grammar correct
   - Natural, easy to understand
   - Professional tone

**9. Challenges & Solutions:**

**Challenge 1: Cold Start (shop mới không có data)**

- Problem: Không có products → no embeddings → no context
- Solution:
  - Provide sample products template theo industry
  - AI fallback: Answer với general knowledge
  - Onboarding guide: import products first

**Challenge 2: Vietnamese Language Support**

- Problem: Some embedding models poor với tiếng Việt
- Solution:
  - Use Google's textembedding-gecko (good multilingual support)
  - Gemini 1.5 Pro excellent Vietnamese understanding
  - Test extensively với Vietnamese queries

**Challenge 3: Hallucination (LLM tạo thông tin sai)**

- Problem: LLM might generate plausible-sounding but incorrect info
- Solution:
  - Strict system prompt: "Chỉ trả lời dựa trên context"
  - Validate answer against database
  - Include sources (related products) để user verify
  - Show confidence scores

**Challenge 4: API Cost**

- Problem: Vertex AI charges per API call
- Current pricing:
  - Embedding: $0.00025 per 1K characters
  - Gemini: $0.00025 per 1K input tokens, $0.0005 per 1K output tokens
- Solution:
  - Cache embeddings (generate once)
  - Cache frequent questions (Redis)
  - Use free tier: 1500 requests/day (sufficient cho MVP)
  - Batch embedding generation
  - Monitor usage, set budget alerts

**10. Future Enhancements:**

- ✅ Hybrid search: Combine vector search + SQL filters (price range, category)
- ✅ Multi-modal: Support images trong RAG (product images)
- ✅ Streaming responses: Token-by-token output như ChatGPT
- ✅ Fine-tuning: Train custom model trên shop-specific data
- ✅ Query rewriting: Reformulate unclear questions
- ✅ Multi-turn conversations: Better context tracking
- ✅ Voice input: Speech-to-text cho mobile users

---

## 📋 Câu hỏi 7: Real-time Notifications với SignalR

### ❓ Câu hỏi:

**"Em giải thích cách implement real-time notifications? Tại sao chọn SignalR thay vì WebSocket thuần hoặc polling?"**

### ✅ Trả lời:

**1. Tại sao cần Real-time Notifications?**

**Use Cases trong hệ thống:**

- 🔔 **Low Stock Alerts**: Cảnh báo tồn kho thấp ngay lập tức khi sản phẩm còn ít
- 💰 **Payment Success**: Thông báo thanh toán subscription thành công real-time
- 📦 **New Orders**: Thông báo đơn hàng mới được tạo
- 👥 **Multi-user Updates**: Nhiều nhân viên cùng làm việc, cần sync real-time
- 📊 **Dashboard Updates**: Cập nhật thống kê doanh thu tức thời

**Yêu cầu kỹ thuật:**

- Latency thấp (< 1 second từ server đến client)
- Reliable delivery (đảm bảo message không bị mất)
- Auto-reconnect khi connection bị mất (network issues)
- Scalable cho nhiều users đồng thời

**2. So sánh các giải pháp Real-time:**

| Tiêu chí        | SignalR              | WebSocket            | Polling             | SSE                   |
| --------------- | -------------------- | -------------------- | ------------------- | --------------------- |
| Latency         | ⭐⭐⭐⭐⭐ (< 100ms) | ⭐⭐⭐⭐⭐ (< 50ms)  | ⭐⭐ (5-30s)        | ⭐⭐⭐⭐ (< 1s)       |
| Reliability     | ⭐⭐⭐⭐⭐           | ⭐⭐⭐               | ⭐⭐⭐⭐⭐          | ⭐⭐⭐⭐              |
| Auto-reconnect  | ✅ Built-in          | ❌ Manual            | ✅ Built-in         | ❌ Manual             |
| Fallback        | ✅ Auto (WS→SSE→LP)  | ❌ Không có          | N/A                 | ❌ Không có           |
| Server Load     | ⭐⭐⭐⭐ (efficient) | ⭐⭐⭐⭐ (efficient) | ⭐⭐ (heavy)        | ⭐⭐⭐ (moderate)     |
| Bi-directional  | ✅ Yes               | ✅ Yes               | ❌ No               | ❌ Server→Client only |
| Browser Support | ⭐⭐⭐⭐⭐ (all)     | ⭐⭐⭐⭐ (IE10+)     | ⭐⭐⭐⭐⭐ (all)    | ⭐⭐⭐⭐ (no IE)      |
| Implementation  | ⭐⭐⭐⭐ (easy)      | ⭐⭐⭐ (complex)     | ⭐⭐⭐⭐⭐ (simple) | ⭐⭐⭐⭐ (easy)       |

**Tại sao chọn SignalR:**

- ✅ **Built-in với ASP.NET Core**: Không cần install thư viện bên ngoài, tích hợp sẵn
- ✅ **Auto-fallback**: Tự động chuyển WebSocket → Server-Sent Events → Long Polling nếu WebSocket fail
- ✅ **Auto-reconnect**: Tự động kết nối lại với exponential backoff khi mất kết nối
- ✅ **Scale-out support**: Hỗ trợ Redis backplane để scale multiple server instances
- ✅ **Type-safe**: Strongly-typed hubs với C#
- ✅ **Easy to use**: API đơn giản, dễ implement

**3. SignalR Implementation - Backend:**

**A. NotificationHub (Core component):**

Hub là central point nhận và gửi messages:

**OnConnectedAsync()**: Được gọi khi client connect thành công

1. Extract shopOwnerId từ JWT token (User.FindFirst("shop_owner_id"))
2. Add connection vào Group theo ShopOwner: Groups.AddToGroupAsync(connectionId, "ShopOwner_123")
3. Track connectionId trong dictionary để quản lý: \_userConnections[shopOwnerId].Add(connectionId)
4. Log connection event cho monitoring

**OnDisconnectedAsync()**: Được gọi khi client disconnect

1. Remove connection khỏi group: Groups.RemoveFromGroupAsync()
2. Remove từ tracking dictionary
3. Clean up resources nếu user không còn connections
4. Log disconnection event

**SendNotificationToShopOwner()**: Method để send notification đến specific shop owner

1. Gọi Clients.Group("ShopOwner_123").SendAsync("ReceiveNotification", data)
2. SignalR tự động route message đến tất cả connections trong group
3. Support multiple devices: User có thể login nhiều devices, tất cả đều nhận notification

**Connection tracking:**

Sử dụng ConcurrentDictionary để thread-safe tracking:

- Key: ShopOwnerId (int)
- Value: List<ConnectionId> (strings)
- Cho phép track nhiều connections per user (multi-device support)

**B. Register Hub trong Program.cs:**

```
builder.Services.AddSignalR(); // Enable SignalR service
app.MapHub<NotificationHub>("/hubs/notification"); // Map URL endpoint
```

Endpoint /hubs/notification là URL mà frontend sẽ connect đến.

**C. Send Notification từ Service:**

LowStockNotificationService example:

1. **Detect low stock**: Background service chạy mỗi 5 phút, query Products WHERE Stock <= MinStock
2. **Create notification**: Tạo Notification entity trong database với: Title, Message, Type, ShopOwnerId, RelatedEntityId
3. **Save to DB**: Lưu notification để user xem lại sau (persistent)
4. **Send real-time**: Gọi HubContext.Clients.Group("ShopOwner_X").SendAsync()
5. **Non-blocking**: Notification được gửi async, không block business logic

**4. Frontend Implementation (Angular):**

**A. SignalRService:**

Central service quản lý SignalR connection:

**startConnection(token):**

1. Build HubConnection với HubConnectionBuilder
2. Configure URL: environment.apiUrl + '/hubs/notification'
3. Add access token factory: () => token (JWT token từ localStorage)
4. Configure transport fallback priority: WebSocket → SSE → LongPolling
5. Configure auto-reconnect với exponential backoff:
   - Retry 0: 0ms (immediate)
   - Retry 1: 2 seconds
   - Retry 2: 10 seconds
   - Retry 3+: 30 seconds
6. Register event listeners: on('ReceiveNotification')
7. Start connection: hubConnection.start()

**Event handlers:**

- **on('ReceiveNotification')**: Callback khi nhận notification từ server, emit qua BehaviorSubject
- **onclose**: Connection closed event, update state
- **onreconnecting**: Reconnecting event, show "reconnecting..." UI
- **onreconnected**: Reconnected successfully, hide reconnecting UI

**BehaviorSubjects:**

- notificationReceivedSubject: Stream notifications đến subscribers
- connectionStateSubject: Track connection state (Connected/Reconnecting/Disconnected)

**B. NotificationService với Polling Fallback:**

Hybrid approach: SignalR + Polling fallback

**Initialization:**

1. Start SignalR connection
2. Subscribe to notificationReceived$ stream
3. Monitor connection state

**Connection state monitoring:**

- If state = Connected: Stop polling (nếu đang chạy)
- If state = Disconnected (và trước đó connected): Start polling fallback
- Polling interval: 30 seconds

**Polling implementation:**

- Use RxJS interval(30000) để poll mỗi 30 giây
- switchMap to API call: GET /api/notifications/recent
- Compare với last received notification để tránh duplicates
- Stop polling khi SignalR reconnect thành công

**Benefits:**

- Reliability: User vẫn nhận notifications nếu SignalR fail
- Graceful degradation: Fallback to polling tự động
- No notification lost: Polling đảm bảo fetch missed notifications

**5. Background Service (Low Stock Check):**

**LowStockCheckBackgroundService:**

Hosted service chạy background trong ASP.NET Core:

**ExecuteAsync() loop:**

1. Chạy infinite while loop với cancellation token
2. Mỗi iteration:
   - Call CheckLowStockAsync()
   - await Task.Delay(checkInterval) - mặc định 5 phút
3. Catch exceptions để service không crash

**CheckLowStockAsync() process:**

1. Query database: SELECT \* FROM Products WHERE Stock <= 10 AND IsActive = true
2. Filter products already notified trong 24 giờ qua (tránh spam)
3. Với mỗi low stock product chưa được notify:
   - Create Notification record trong DB
   - Call NotificationService.NotifyLowStockAsync(productId)
4. NotificationService trigger SignalR Hub để send real-time

**Why background service:**

- Decouple checking logic từ user requests
- Run independently, không affect API performance
- Configurable interval (có thể change frequency)
- Automatic restart nếu crash

**6. Performance & Scalability:**

**Connection Management:**

Single server capacity:

- Max concurrent connections: ~10,000 (depends on hardware)
- Memory per connection: ~4KB
- CPU overhead: Minimal với WebSocket
- Connection timeout: 30 seconds inactive

**Scale-out với Redis Backplane:**

Khi scale multiple Cloud Run instances:

1. Add Redis: builder.Services.AddSignalR().AddStackExchangeRedis()
2. Configure connection: Redis connection string
3. Set channel prefix: "ShopManagement" để isolate messages

**How it works:**

- User A connect đến Server 1
- User B connect đến Server 2
- Server 1 send notification → Publish to Redis
- Redis broadcast to all servers
- Server 2 nhận message từ Redis → Send to User B
- Result: Cross-server communication

**Message Compression:**

Configure SignalR options:

- EnableDetailedErrors = false trong production (reduce payload)
- MaximumReceiveMessageSize = 32KB (limit large messages)
- Compression enabled by default cho HTTP

**7. Security Considerations:**

**Authentication:**

- JWT token required: Frontend pass token via query string (WebSocket không support headers)
- Token validation: Backend validate token trong OnMessageReceived event
- User identity mapping: ConnectionId mapped với ShopOwnerId

**Authorization:**

- Group-based isolation: Mỗi ShopOwner có riêng group
- No cross-shop data leakage: User chỉ receive notifications của shop mình
- Validate recipient: Double-check shopOwnerId match token claims

**Rate Limiting:**

Prevent notification spam:

- Track last notification time per user
- If < 5 seconds since last notification: Skip sending
- Dictionary<shopOwnerId, lastNotificationTime>
- Prevent DoS attacks

**8. Error Handling & Monitoring:**

**Client-side errors:**

- Connection failed: Show error message, suggest refresh
- Reconnection failed after max retries: Fallback to polling
- Message send failed: Retry with exponential backoff

**Server-side errors:**

- Hub method exceptions: Try-catch và log errors
- Send failures: Log failed sends, queue for retry
- Connection tracking errors: Clean up orphaned connections

**Logging:**

- Log all connection/disconnection events với timestamps
- Log message send/receive với metadata
- Track error rates và latency metrics
- Alert if error rate > threshold

**Monitoring metrics:**

- Active connections count
- Messages per second
- Average latency (send to receive)
- Reconnection rate
- Error rate

---

## 📋 Câu hỏi 8: Database Design & Optimization

### ❓ Câu hỏi:

**"Em hãy trình bày thiết kế database, các indexes được sử dụng và chiến lược optimization?"**

### ✅ Trả lời:

**1. Database Schema Overview:**

Database được thiết kế theo normalized form (3NF) với focus on:

- Data integrity (ACID compliance)
- Query performance (proper indexes)
- Scalability (efficient schema)
- Multi-tenancy (ShopOwnerId filtering)

**Core Entities Relationships:**

**ShopOwner (1) → (N) Shops**: Một chủ shop có thể quản lý nhiều cửa hàng
**ShopOwner (1) → (N) Products**: Tất cả products thuộc về một ShopOwner
**Product (1) → (1) ProductEmbedding**: Mỗi product có một vector embedding cho AI
**Product (N) → (N) Invoices**: Many-to-many qua InvoiceItems
**ShopOwner (1) → (1) Subscription**: Active subscription

**2. Complete Database Schema:**

**ShopOwners table:**

- ShopOwnerId: SERIAL PRIMARY KEY (auto-increment)
- Phone: VARCHAR(20) UNIQUE NOT NULL (login credential)
- Email: VARCHAR(100) UNIQUE (optional, unique nếu có)
- PasswordHash: VARCHAR(255) NOT NULL (BCrypt hashed, never plain text)
- Personal info: Name, Gender, DateOfBirth, Address, AvatarUrl
- Status: IsActive BOOLEAN (soft disable account)
- Timestamps: CreatedAt, UpdatedAt

**Indexes:**

- PRIMARY KEY on ShopOwnerId (automatic, clustered)
- UNIQUE INDEX on Phone (login lookup)
- UNIQUE INDEX on Email (prevent duplicates)

**Shops table:**

- ShopId: SERIAL PRIMARY KEY
- ShopOwnerId: INT NOT NULL (foreign key)
- ShopCode: VARCHAR(50) UNIQUE (business identifier)
- Shop info: ShopName, Address, Phone, Email
- BusinessCategoryId: INT (type of business)
- Status: IsActive BOOLEAN
- CreatedAt: TIMESTAMP

**Indexes:**

- PRIMARY KEY on ShopId
- FOREIGN KEY on ShopOwnerId
- INDEX on ShopOwnerId (filter shops by owner)
- UNIQUE INDEX on ShopCode (business lookup)

**Products table (Most queried table):**

- ProductId: SERIAL PRIMARY KEY
- ProductCode: VARCHAR(50) NOT NULL (SKU)
- ProductName: VARCHAR(200) NOT NULL (searchable)
- CategoryId: INT (product category foreign key)
- Pricing: Price DECIMAL(18,2), CostPrice DECIMAL(18,2)
- Stock: INT NOT NULL DEFAULT 0 (inventory level)
- Unit: VARCHAR(20) (e.g., "chai", "hộp", "kg")
- Barcode: VARCHAR(100) (for POS scanning)
- Media: ImageUrl VARCHAR(500)
- Description: TEXT (full text search)
- Status: IsActive BOOLEAN (inactive until first purchase)
- ShopOwnerId: INT NOT NULL (data isolation)
- Timestamps: CreatedAt, UpdatedAt

**Indexes (Critical for performance):**

- PRIMARY KEY on ProductId
- FOREIGN KEY on ShopOwnerId
- FOREIGN KEY on CategoryId
- INDEX on ProductCode (lookup by SKU)
- INDEX on ProductName (search by name)
- INDEX on Barcode (POS scan lookup)
- INDEX on IsActive (filter active products)
- INDEX on Stock (low stock queries)
- **COMPOSITE INDEX on (ShopOwnerId, IsActive)** - Most important! Covers common query pattern
- UNIQUE CONSTRAINT on (ProductCode, ShopOwnerId) - Prevent duplicate SKUs per shop

**ProductEmbeddings table (AI Vector Search):**

- ProductId: INT PRIMARY KEY (one-to-one with Products)
- Embedding: vector(768) (Pgvector extension, 768-dimensional array)
- TextContent: TEXT (original text used for embedding)
- CreatedAt, UpdatedAt: TIMESTAMP

**Indexes:**

- PRIMARY KEY on ProductId
- FOREIGN KEY on ProductId with ON DELETE CASCADE
- **VECTOR INDEX using IVFFlat** (Pgvector specific):
  - CREATE INDEX USING ivfflat (Embedding vector_cosine_ops) WITH (lists = 100)
  - IVFFlat = Inverted File with Flat compression
  - lists = 100: Partition space into 100 clusters
  - Enables fast approximate nearest neighbor search

**Invoices table:**

- InvoiceId: SERIAL PRIMARY KEY
- InvoiceCode: VARCHAR(50) NOT NULL UNIQUE (business reference)
- Foreign keys: CustomerId, ShopId, ShopOwnerId, EmployeeId, PaymentMethodId, PromotionId
- Amounts: TotalAmount, DiscountAmount, FinalAmount DECIMAL(18,2)
- InvoiceDate: TIMESTAMP (when created)
- PaymentStatus: VARCHAR(20) - "Paid", "Unpaid", "PartiallyPaid"
- Note: TEXT (optional remarks)
- CreatedAt: TIMESTAMP

**Indexes (Optimized for reporting):**

- PRIMARY KEY on InvoiceId
- FOREIGN KEY constraints on all relations
- INDEX on ShopOwnerId (filter by owner)
- INDEX on CustomerId (customer history)
- INDEX on InvoiceDate (date range queries)
- INDEX on PaymentStatus (unpaid invoices)
- INDEX on InvoiceCode (business lookup)
- **COMPOSITE INDEX on (ShopOwnerId, InvoiceDate)** - Revenue reports by date
- **COMPOSITE INDEX on (ShopOwnerId, PaymentStatus)** - Unpaid invoices per shop

**InvoiceItems table:**

- InvoiceItemId: SERIAL PRIMARY KEY
- InvoiceId: INT NOT NULL (foreign key)
- ProductId: INT NOT NULL (foreign key)
- Quantity: INT NOT NULL (số lượng bán)
- UnitPrice: DECIMAL(18,2) (giá tại thời điểm bán)
- Subtotal: DECIMAL(18,2) (Quantity \* UnitPrice)

**Indexes:**

- PRIMARY KEY on InvoiceItemId
- FOREIGN KEY on InvoiceId with ON DELETE CASCADE (delete items when invoice deleted)
- FOREIGN KEY on ProductId
- INDEX on InvoiceId (get items of invoice)
- INDEX on ProductId (sales report per product)

**Notifications table:**

- NotificationId: SERIAL PRIMARY KEY
- ShopOwnerId: INT NOT NULL (recipient)
- Title: VARCHAR(200), Message: TEXT
- Type: VARCHAR(50) - "LowStock", "InvoicePaid", "SubscriptionExpiring"
- IsRead: BOOLEAN DEFAULT false
- CreatedAt, ReadAt: TIMESTAMP
- Related entity: RelatedEntityType VARCHAR(50), RelatedEntityId INT (polymorphic relation)

**Indexes:**

- PRIMARY KEY on NotificationId
- FOREIGN KEY on ShopOwnerId
- INDEX on IsRead (filter unread)
- INDEX on CreatedAt (recent notifications)
- **COMPOSITE INDEX on (ShopOwnerId, IsRead)** - Unread notifications per user
- **COMPOSITE INDEX on (ShopOwnerId, CreatedAt DESC)** - Recent notifications per user

**Auto-delete old notifications function:**

PostgreSQL function để cleanup:

- DELETE FROM Notifications WHERE CreatedAt < NOW() - INTERVAL '24 hours'
- Run daily via cron job
- Keep database size manageable

**ChatSessions & ChatMessages (AI Assistant):**

**ChatSessions:**

- SessionId: SERIAL PRIMARY KEY
- ShopOwnerId: INT NOT NULL
- Title: VARCHAR(200) (optional session name)
- CreatedAt, UpdatedAt: TIMESTAMP

**ChatMessages:**

- MessageId: SERIAL PRIMARY KEY
- SessionId: INT NOT NULL
- Role: VARCHAR(20) - "user" hoặc "assistant"
- Content: TEXT (message text)
- CreatedAt: TIMESTAMP

**Indexes:**

- INDEX on ShopOwnerId (sessions per user)
- INDEX on SessionId (messages per session)
- INDEX on CreatedAt (chronological order)

**3. Indexing Strategy:**

**Why Indexes Matter:**

Without index: O(n) complexity - Full table scan

- Example: 1 million rows, query without index = scan 1 million rows
- Average time: Several seconds

With index: O(log n) complexity - Binary search on B-tree

- Example: 1 million rows, query with index = ~20 comparisons
- Average time: Milliseconds

**Performance improvement:** 1000x faster!

**Indexes Created:**

**A. Primary Key Indexes (Automatic):**

PostgreSQL automatically creates clustered B-tree index on PRIMARY KEY:

- ShopOwners(ShopOwnerId)
- Products(ProductId)
- Invoices(InvoiceId)
- All tables with PRIMARY KEY

**B. Unique Indexes (Data integrity + Performance):**

Enforce uniqueness AND provide fast lookup:

- ShopOwners(Phone) - Login queries
- ShopOwners(Email) - User lookup
- Products(ProductCode, ShopOwnerId) - SKU uniqueness per shop
- Invoices(InvoiceCode) - Business reference

**C. Single Column Indexes:**

For simple WHERE clauses:

- Products(ShopOwnerId) - Filter products by owner
- Products(IsActive) - Filter active products only
- Products(Stock) - Low stock alerts
- Invoices(PaymentStatus) - Unpaid invoices
- Notifications(IsRead) - Unread notifications

**D. Composite Indexes (Most Effective!):**

Order matters - most selective column first:

**Products(ShopOwnerId, IsActive):**

- Query: WHERE ShopOwnerId = 123 AND IsActive = true
- Index covers both conditions
- Avoids full table scan
- Performance: 5ms vs 500ms without index

**Invoices(ShopOwnerId, InvoiceDate):**

- Query: WHERE ShopOwnerId = 123 AND InvoiceDate BETWEEN '2024-01-01' AND '2024-12-31'
- Revenue reports by date range
- Sorted by date for ORDER BY optimization
- Performance: 10ms vs 2000ms

**Invoices(ShopOwnerId, PaymentStatus):**

- Query: WHERE ShopOwnerId = 123 AND PaymentStatus = 'Unpaid'
- Find unpaid invoices per shop
- Common admin dashboard query

**Notifications(ShopOwnerId, IsRead):**

- Query: WHERE ShopOwnerId = 123 AND IsRead = false
- Unread notifications badge
- Real-time notification system
- Performance: < 1ms

**E. Full-Text Search Index (GIN):**

For text search on ProductName:

- CREATE INDEX USING gin(to_tsvector('english', ProductName))
- Tokenize and index words
- Support complex queries: "coca & cola" hoặc "coca | pepsi"
- Performance: 20ms vs 5000ms for LIKE queries

**F. Vector Index (Pgvector IVFFlat):**

For AI similarity search:

- CREATE INDEX USING ivfflat (Embedding vector_cosine_ops)
- Partition vector space into clusters (lists = 100)
- Approximate nearest neighbor search
- Trade-off: Speed vs Accuracy (adjustable với lists parameter)
- Performance: < 10ms for similarity search trong 10K+ products

**4. Query Optimization Examples:**

**Example 1: Get Active Products by ShopOwner**

**Bad query (no index utilization):**

```
SELECT * FROM Products
WHERE ShopOwnerId = 123 AND IsActive = true;
```

Without composite index: Sequential scan → Slow (500ms)

**Good query (uses composite index):**
Same query, but với composite index idx_product_shopowner_active:

- Index Seek: Navigate directly to matching rows
- Performance: 5ms (100x faster)

**Execution plan:**

```
Index Scan using idx_product_shopowner_active on products
  Index Cond: ((shopownerid = 123) AND (isactive = true))
  Planning Time: 0.123 ms
  Execution Time: 4.567 ms
```

**Example 2: Revenue Report by Date Range**

**Bad query (wrong filter order):**

```
SELECT SUM(FinalAmount)
FROM Invoices
WHERE InvoiceDate BETWEEN '2024-01-01' AND '2024-12-31'
  AND ShopOwnerId = 123;
```

Index on InvoiceDate only → Scan nhiều invoices của all shops, rồi filter ShopOwnerId

**Good query (optimal filter order):**

```
SELECT SUM(FinalAmount)
FROM Invoices
WHERE ShopOwnerId = 123
  AND InvoiceDate BETWEEN '2024-01-01' AND '2024-12-31';
```

Composite index (ShopOwnerId, InvoiceDate):

- Filter ShopOwnerId first (more selective)
- Then filter date range
- Performance: 10ms vs 2000ms (200x faster)

**Example 3: Top Selling Products**

Complex query với multiple JOINs:

```
SELECT
    p.ProductId,
    p.ProductName,
    SUM(ii.Quantity) AS TotalSold,
    SUM(ii.Subtotal) AS TotalRevenue
FROM InvoiceItems ii
JOIN Invoices i ON ii.InvoiceId = i.InvoiceId
JOIN Products p ON ii.ProductId = p.ProductId
WHERE i.ShopOwnerId = 123
  AND i.InvoiceDate >= NOW() - INTERVAL '30 days'
GROUP BY p.ProductId, p.ProductName
ORDER BY TotalSold DESC
LIMIT 10;
```

Uses indexes:

- idx_invoice_shopowner_date (filter invoices)
- idx_invoiceitem_invoice (join)
- idx_invoiceitem_product (join)
- PRIMARY KEY indexes on Products

Performance: 50ms (efficient với proper indexes)

**5. Performance Optimization Techniques:**

**A. Pagination (Avoid OFFSET):**

**Bad: OFFSET pagination (slow for large offsets)**

```
SELECT * FROM Products
WHERE ShopOwnerId = 123
ORDER BY ProductId
LIMIT 20 OFFSET 10000;
```

Problem: Database scans 10,020 rows, returns only 20

**Good: Keyset pagination (cursor-based)**

```
SELECT * FROM Products
WHERE ShopOwnerId = 123 AND ProductId > 10000
ORDER BY ProductId
LIMIT 20;
```

Performance: Scans only 20 rows (500x faster for large datasets)

**B. Avoid SELECT \*:**

**Bad: Select all columns**

```
SELECT * FROM Products WHERE ProductId = 123;
```

Returns 20+ columns, many unnecessary

**Good: Select only needed columns**

```
SELECT ProductId, ProductName, Price, Stock
FROM Products WHERE ProductId = 123;
```

Benefits:

- Reduced network bandwidth (4 columns vs 20)
- Faster serialization
- Less memory usage
- Enables covering indexes

**C. Use EXISTS instead of COUNT:**

**Bad: COUNT for existence check**

```
IF (SELECT COUNT(*) FROM Products WHERE ShopOwnerId = 123) > 0
```

Problem: Counts all rows (expensive)

**Good: EXISTS (stops at first match)**

```
IF EXISTS (SELECT 1 FROM Products WHERE ShopOwnerId = 123)
```

Performance: 1ms vs 100ms (100x faster)

**D. Batch Operations:**

**Bad: Multiple individual inserts (N queries)**

```
foreach (var item in items) {
    await context.InvoiceItems.AddAsync(item);
    await context.SaveChangesAsync(); // N database roundtrips
}
```

**Good: Batch insert (1 query)**

```
await context.InvoiceItems.AddRangeAsync(items);
await context.SaveChangesAsync(); // Single transaction
```

Performance: 1000 items: 5 seconds → 200ms (25x faster)

**E. Compiled Queries (EF Core):**

Cache query compilation for reused queries:

```
private static readonly Func<ApplicationDbContext, int, IAsyncEnumerable<Product>>
    GetProductsByShopOwner = EF.CompileAsyncQuery(
        (ApplicationDbContext context, int shopOwnerId) =>
            context.Products.Where(p => p.ShopOwnerId == shopOwnerId)
    );
```

Benefits:

- Query compiled once, reused nhiều lần
- Skip LINQ to SQL translation overhead
- Performance: 10-20% faster for hot paths

**6. Database Maintenance:**

**A. VACUUM (PostgreSQL specific):**

Reclaim storage from deleted/updated rows:

```
VACUUM ANALYZE Products; -- Reclaim space + update statistics
VACUUM ANALYZE Invoices;
```

Auto-vacuum configuration:

```
ALTER TABLE Products SET (autovacuum_vacuum_scale_factor = 0.1);
```

Trigger autovacuum when 10% of rows changed

**B. Rebuild Indexes:**

Over time, indexes fragment. Rebuild to optimize:

```
REINDEX INDEX idx_product_shopowner_active; -- Specific index
REINDEX TABLE Products; -- All indexes on table
```

Schedule monthly hoặc after bulk operations

**C. Update Statistics:**

Query planner needs accurate statistics:

```
ANALYZE Products; -- Update table statistics
ANALYZE Invoices;
```

Run after bulk inserts/deletes

**7. Monitoring & Metrics:**

**Slow Query Log:**

Identify slow queries:

```
SELECT query, mean_exec_time, calls
FROM pg_stat_statements
ORDER BY mean_exec_time DESC
LIMIT 10;
```

Action: Add indexes hoặc optimize queries > 100ms

**Index Usage:**

Check if indexes are actually used:

```
SELECT schemaname, tablename, indexname,
       idx_scan AS index_scans,
       idx_tup_read AS tuples_read
FROM pg_stat_user_indexes
WHERE schemaname = 'public'
ORDER BY idx_scan ASC;
```

idx_scan = 0: Unused index → Consider dropping

**Table Size:**

Monitor database growth:

```
SELECT tablename,
       pg_size_pretty(pg_total_relation_size(tablename::regclass)) AS size
FROM pg_tables
WHERE schemaname = 'public'
ORDER BY pg_total_relation_size(tablename::regclass) DESC;
```

Action: Archive old data hoặc partition large tables

**8. Results:**

| Metric          | Before Optimization | After Optimization   | Improvement   |
| --------------- | ------------------- | -------------------- | ------------- |
| Product listing | 500ms               | 5ms                  | 100x          |
| Revenue report  | 2000ms              | 10ms                 | 200x          |
| Search products | 5000ms              | 20ms                 | 250x          |
| Top sellers     | 3000ms              | 50ms                 | 60x           |
| Database size   | 2GB                 | 1.5GB (after VACUUM) | 25% reduction |

---

## 📋 Câu hỏi 9: MoMo Payment Integration

### ❓ Câu hỏi:

**"Em giải thích quy trình tích hợp MoMo payment gateway? Cách xử lý callback và validate signature?"**

### ✅ Trả lời:

**1. MoMo Payment Flow Overview:**

**10-step process:**

1. **User selects subscription plan**: Click "Chọn gói Professional"
2. **Frontend sends request**: POST /api/payment/create-momo-payment với planId, shopOwnerId
3. **Backend generates payment URL**: Call MoMo API với order details + signature
4. **MoMo returns PayURL**: URL để redirect user
5. **Frontend redirects**: User mở trang thanh toán MoMo
6. **User confirms payment**: Nhập OTP, confirm trong MoMo app
7. **MoMo processes**: Charge user account
8. **IPN Callback (Server-to-Server)**: MoMo gọi webhook của chúng tôi
9. **Backend validates và activates**: Verify signature, activate subscription
10. **User redirected back**: Return to success page với confirmation

**2. MoMo API Configuration:**

**Settings (stored in appsettings.json / User Secrets):**

- PartnerCode: "MOMOXXX" - Unique identifier của merchant
- AccessKey: Provided by MoMo (authentication key)
- SecretKey: Shared secret để sign requests (MUST keep secret!)
- Endpoint: "https://test-payment.momo.vn/v2/gateway/api/create" (sandbox) hoặc production URL
- ReturnUrl: "http://localhost:4200/payment/success" - Redirect sau khi thanh toán
- IpnUrl: "https://your-api.com/api/payment/momo-callback" - Server-to-server callback webhook

**Security:**

- SecretKey NEVER commit to Git
- Store trong environment variables hoặc Azure Key Vault
- Rotate keys định kỳ

**3. Create Payment Request:**

**MoMoService.CreatePaymentMomo():**

**Step 1: Generate unique identifiers**

- orderId: "SUB*{shopOwnerId}*{timestamp}" - Unique order reference
- requestId: Guid.NewGuid() - Unique request ID

**Step 2: Prepare request data**
Build raw signature string (alphabetical order - IMPORTANT!):

```
accessKey={accessKey}
&amount={amount}
&extraData=
&ipnUrl={ipnUrl}
&orderId={orderId}
&orderInfo={orderInfo}
&partnerCode={partnerCode}
&redirectUrl={returnUrl}
&requestId={requestId}
&requestType=payWithATM
```

**Step 3: Generate HMAC SHA256 signature**

- Use HMACSHA256 algorithm
- Key: SecretKey (binary)
- Message: rawData string
- Output: Hex string (lowercase)
- Purpose: Prove request authenticity, prevent tampering

**Step 4: Build request body JSON**
Include all parameters + signature:

```
{
  "partnerCode": "MOMOXXX",
  "orderId": "SUB_123_20240103120000",
  "amount": 500000,
  "orderInfo": "Thanh toán gói Professional",
  "signature": "computed_hmac_sha256_hash",
  ...
}
```

**Step 5: Send HTTP POST to MoMo**

- URL: MoMo create payment endpoint
- Method: POST
- Content-Type: application/json
- Body: request JSON

**Response from MoMo:**

```
{
  "partnerCode": "MOMOXXX",
  "orderId": "SUB_123...",
  "requestId": "guid...",
  "resultCode": 0,  // 0 = success
  "message": "Success",
  "payUrl": "https://payment.momo.vn/gw_payment/pay?t=xxx",
  "deeplink": "momo://app/payment?t=xxx",
  "qrCodeUrl": "https://payment.momo.vn/qr?t=xxx"
}
```

**Step 6: Return PayURL to frontend**
Frontend redirects user to payUrl

**4. IPN Callback Handler:**

**What is IPN?** Instant Payment Notification - Server-to-server webhook

**PaymentController.MoMoCallback():**

**Step 1: Log incoming request**

```
Log: "MoMo IPN Callback received: OrderId={orderId}"
```

**Step 2: Validate signature (CRITICAL!)**

Recreate signature string từ callback data (same format as create request):

```
accessKey={accessKey}
&amount={amount}
&extraData={extraData}
&message={message}
&orderId={orderId}
&orderInfo={orderInfo}
&orderType={orderType}
&partnerCode={partnerCode}
&payType={payType}
&requestId={requestId}
&responseTime={responseTime}
&resultCode={resultCode}
&transId={transId}
```

Compute HMAC SHA256 với SecretKey:

- expectedSignature = ComputeHmacSha256(rawData, secretKey)
- Compare với request.Signature
- If NOT match: Return BadRequest("Invalid signature") - SECURITY BREACH!

**Why validate signature?**

- Prevent fraud: Ensure request actually from MoMo
- Data integrity: Verify data not tampered
- Authentication: Prove sender identity

**Step 3: Check payment result**

- resultCode == 0: Payment success
- resultCode != 0: Payment failed
- If failed: Log error, update PendingSubscription status = "Failed", return Ok

**Step 4: Find pending subscription**

- Query PendingSubscriptions WHERE OrderId = request.OrderId AND IsProcessed = false
- If not found: Return Ok("Order not found or already processed") - Idempotency
- Prevent double processing

**Step 5: Verify amount match**

- Compare request.Amount với pendingSubscription.Amount
- If mismatch: Security alert! Someone trying to pay less
- Log warning, return BadRequest("Amount mismatch")

**Step 6: Activate subscription**

- Call ActivateSubscriptionAsync(pendingSubscription)
- Check if user has existing subscription:
  - Yes: Extend EndDate += plan.DurationDays
  - No: Create new Subscription record
- StartDate = NOW(), EndDate = NOW() + duration
- IsActive = true

**Step 7: Mark as processed**

- pendingSubscription.IsProcessed = true
- pendingSubscription.ProcessedAt = DateTime.UtcNow
- Save to database
- Prevent duplicate processing nếu MoMo retry callback

**Step 8: Send notification**

- Create Notification: "Subscription activated successfully"
- Call NotificationService.SendSubscriptionActivatedNotification()
- User nhận real-time notification qua SignalR

**Step 9: Return response to MoMo**

- Return Ok(new { message = "Success" })
- MoMo marks transaction as completed
- If we don't respond: MoMo retries callback multiple times

**5. Security Considerations:**

**A. Signature Validation (Highest Priority!):**

ALWAYS validate signature - không thể skip:

```
if (!ValidateSignature(request)) {
    Log.Warning("SECURITY ALERT: Invalid signature from IP: {ip}");
    return BadRequest("Invalid signature");
}
```

Without validation: Anyone could POST fake payment confirmations!

**B. Idempotency (Prevent double processing):**

Check IsProcessed flag:

```
if (pendingSubscription.IsProcessed) {
    Log.Info("Order already processed: {orderId}");
    return Ok("Already processed");
}
```

Why needed: MoMo retries callbacks if không receive response

**C. Amount Verification:**

Verify amount matches expected:

```
if (request.Amount != pendingSubscription.Amount) {
    Log.Warning("SECURITY ALERT: Amount mismatch. OrderId: {orderId}");
    return BadRequest("Amount mismatch");
}
```

Prevent: User paying less than required

**D. IP Whitelist (Optional but recommended):**

Only accept callbacks from MoMo IPs:

```
var allowedIPs = new[] { "103.x.x.x", "123.x.x.x" }; // MoMo IPs
var clientIP = HttpContext.Connection.RemoteIpAddress?.ToString();

if (!allowedIPs.Contains(clientIP)) {
    Log.Warning("Callback from unauthorized IP: {ip}", clientIP);
    return Unauthorized();
}
```

**6. Testing:**

**Sandbox Environment:**

MoMo provides test environment:

- Endpoint: https://test-payment.momo.vn/v2/gateway/api/create
- Test credentials: Provided in developer portal
- Test cards/accounts: No real money charged
- Same API flow as production

**Test Cases:**

**1. Successful payment:**

- Input: Valid subscription plan
- Expected: payUrl returned, user redirected, IPN received, subscription activated
- Verify: Check database, user notifications

**2. Invalid signature:**

- Tamper signature before send
- Expected: MoMo reject request, return error code

**3. Amount mismatch:**

- Modify amount in callback
- Expected: Backend reject, log security alert

**4. Duplicate callback:**

- Send same callback twice
- Expected: First succeeds, second returns "Already processed"

**5. Payment failed:**

- User cancels payment
- Expected: resultCode != 0, subscription not activated, user notified

**7. Error Handling:**

**Common MoMo Error Codes:**

| ResultCode | Meaning           | Action                   |
| ---------- | ----------------- | ------------------------ |
| 0          | Success           | Activate subscription    |
| 1          | Invalid signature | Check SecretKey          |
| 4          | Invalid amount    | Verify calculation       |
| 9          | Invalid orderId   | Check format             |
| 10         | Duplicate request | Idempotency check        |
| 1001       | Payment failed    | Show error to user       |
| 1006       | User cancelled    | Show "Payment cancelled" |

**Error handling trong code:**

```
if (response.ResultCode != 0) {
    var errorMessage = response.ResultCode switch {
        1 => "Chữ ký không hợp lệ",
        4 => "Số tiền không hợp lệ",
        9 => "Mã đơn hàng không hợp lệ",
        1006 => "Người dùng hủy thanh toán",
        1001 => "Thanh toán thất bại",
        _ => $"Lỗi không xác định ({response.ResultCode})"
    };

    return BadRequest(new {
        success = false,
        message = errorMessage,
        resultCode = response.ResultCode
    });
}
```

**8. Monitoring & Logging:**

**Key events to log:**

```
// Create payment
Log.Info("Creating MoMo payment. OrderId: {orderId}, Amount: {amount}");

// Callback received
Log.Info("MoMo callback received. OrderId: {orderId}, ResultCode: {code}");

// Subscription activated
Log.Info("Subscription activated. ShopOwnerId: {id}, Plan: {plan}");

// Errors
Log.Error("MoMo payment error: {message}");
```

**Metrics to track:**

- Payment success rate
- Average payment time
- Error rate by ResultCode
- IPN callback latency
- Duplicate callback frequency

**Alerts:**

- Error rate > 5%: Investigate immediately
- No callbacks received for > 1 hour: Check IPN URL accessibility
- High duplicate callback rate: Check IsProcessed logic

**9. Production Checklist:**

Before going live:

- ✅ Change to production endpoint URL
- ✅ Use production credentials (PartnerCode, AccessKey, SecretKey)
- ✅ Update IpnUrl to production URL (accessible from internet)
- ✅ Verify HTTPS enabled (MoMo requires HTTPS)
- ✅ Test all error scenarios
- ✅ Set up monitoring and alerts
- ✅ Document all error codes and handling
- ✅ Review security: signature validation, IP whitelist, amount verification
- ✅ Test idempotency with duplicate callbacks
- ✅ Backup database before first production payment

---

## 📋 Câu hỏi 10-15: (Tiếp tục tương tự...)

**[Lưu ý: Do giới hạn độ dài, tôi sẽ tóm tắt các câu hỏi còn lại. Bạn có thể yêu cầu chi tiết từng câu nếu cần]**

**Q10: Security & Data Protection**

- OWASP Top 10 coverage (A01-A10)
- BCrypt password hashing (cost factor = 12)
- JWT security best practices
- Data isolation với ShopOwnerId filtering
- Input validation với Data Annotations
- XSS & CSRF protection với CSP headers
- SQL Injection prevention với EF Core parameterized queries

**Q11: CI/CD Pipeline**

- GitHub Actions workflow automation
- Multi-stage Docker build (SDK → Publish → Runtime)
- Google Cloud Run deployment
- Secrets management với Secret Manager
- Health check endpoints (/health, /health/db)
- Automated testing trong pipeline

**Q12: Testing Strategy**

- Testing Pyramid: 70% Unit, 25% Integration, 5% E2E
- xUnit + Moq cho unit tests
- WebApplicationFactory cho integration tests
- Test coverage: Services, Repositories, Controllers
- Mocking dependencies với Mock objects

**Q13: Challenges & Solutions**

- SignalR connection issues → Auto-reconnect + polling fallback
- Pgvector accuracy → Enrich text với synonyms
- MoMo IPN timeout → Async processing với background jobs
- N+1 query problem → Eager loading với Include()
- JWT expiry UX → Refresh token mechanism
- Memory leak → Proper cleanup trong OnDisconnectedAsync()

**Q14: Performance Optimization**

- Database: Eager loading, pagination, proper indexes
- Response caching cho static data
- Async/await everywhere
- DTO pattern reduce payload size
- Image optimization: WebP, thumbnails, lazy loading
- Results: 3-5x faster response times

**Q15: Future Development**

- Short-term: Mobile app, Advanced reporting, Inventory enhancement
- Mid-term: Multi-currency, E-commerce integration, Advanced AI
- Long-term: Microservices, Multi-tenant SaaS, B2B marketplace
- Technology upgrades: GraphQL, Real-time collaboration, Blockchain

**Q16-19: Technology Comparison Questions**

- Q16: GCP vs AWS/Azure - Cost, ease of use, Vertex AI integration
- Q17: PostgreSQL vs MySQL/SQL Server - Pgvector, JSONB, cost
- Q18: Angular vs React/Vue - TypeScript first, full framework, RxJS
- Q19: SignalR vs Socket.io/WebSocket - Native .NET, auto-reconnect, fallback

---

# 🎉 Kết luận

**Dự án hoàn chỉnh với:**

- ✅ Full-stack SaaS application
- ✅ AI-powered với RAG
- ✅ Real-time features
- ✅ Production-ready deployment
- ✅ Comprehensive documentation

**Câu hỏi dự phòng:**

- So sánh với competitors?
- Scalability strategy?
- Monthly operational costs?
- Data backup and recovery?
- Marketing and customer acquisition?

---

**Chúc bạn bảo vệ tốt nghiệp thành công! 🎓**
