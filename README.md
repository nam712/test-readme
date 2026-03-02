# Komorebi - Huong Dan Cau Hinh Chi Tiet

Tai lieu nay liet ke **tat ca file cau hinh**, **tung dong can doi**, va huong dan chay du an trong cac kich ban khac nhau.

---

## Muc Luc

- [Komorebi - Huong Dan Cau Hinh Chi Tiet](#komorebi---huong-dan-cau-hinh-chi-tiet)
  - [Muc Luc](#muc-luc)
  - [1. So Do Database](#1-so-do-database)
    - [Tao Database](#tao-database)
  - [2. Cau Hinh Tung Service](#2-cau-hinh-tung-service)
    - [2.1 API Gateway](#21-api-gateway)
    - [2.2 Auth Service](#22-auth-service)
    - [2.3 Manga Service](#23-manga-service)
    - [2.4 Comment Service](#24-comment-service)
    - [2.5 User Service](#25-user-service)
    - [2.6 Admin Service](#26-admin-service)
    - [2.7 Notification Service](#27-notification-service)
    - [2.8 Background Worker](#28-background-worker)
    - [2.9 Frontend (Next.js)](#29-frontend-nextjs)
  - [3. Chay Bang Docker Compose](#3-chay-bang-docker-compose)
    - [3.1 Chay Toan Bo](#31-chay-toan-bo)
    - [3.2 Chi Chay Infrastructure (de backend local)](#32-chi-chay-infrastructure-de-backend-local)
  - [4. Chay Local Tren Windows (Khong Docker)](#4-chay-local-tren-windows-khong-docker)
    - [4.1 Yeu Cau Bat Buoc](#41-yeu-cau-bat-buoc)
    - [4.2 Cai Dat](#42-cai-dat)
    - [4.3 Tao Database](#43-tao-database)
    - [4.4 Cau Hinh](#44-cau-hinh)
    - [4.5 Chay](#45-chay)
  - [5. Cau Hinh Storage (Local / MinIO / S3)](#5-cau-hinh-storage-local--minio--s3)
    - [5.1 Local Storage (mac dinh — khong can MinIO/S3)](#51-local-storage-mac-dinh--khong-can-minios3)
    - [5.2 MinIO](#52-minio)
    - [5.3 AWS S3 / Cloudflare R2](#53-aws-s3--cloudflare-r2)
    - [5.4 Bang Tom Tat Storage](#54-bang-tom-tat-storage)
  - [6. Quy Tac Quan Trong](#6-quy-tac-quan-trong)
    - [JWT Secret phai giong nhau](#jwt-secret-phai-giong-nhau)
    - [PostgreSQL password phai nhat quan](#postgresql-password-phai-nhat-quan)
    - [Service nao dung gi](#service-nao-dung-gi)
    - [Tat Redis / RabbitMQ?](#tat-redis--rabbitmq)

---

## 1. So Do Database

He thong su dung **5 databases rieng biet** (microservices pattern). Moi service so huu database rieng, khong FK cross-database.

```
komorebi_auth    (1 bang)   ← AuthService (:5001)
  └── users

komorebi_manga   (8 bang)   ← MangaService (:5002)
  ├── genres
  ├── manga_metadata
  ├── manga_chapters
  ├── manga_ratings
  ├── manga_reports
  ├── manga_follows
  ├── manga_daily_views
  └── reading_histories

komorebi_comment (3 bang)   ← CommentService (:5003)
  ├── comments
  ├── comment_likes
  └── comment_reports

komorebi_user    (9 bang)   ← UserService (:5004)
  ├── users                 (dong bo tu komorebi_auth qua RabbitMQ)
  ├── uploader_applications
  ├── device_tokens
  ├── notifications
  ├── notification_preferences
  ├── user_reading_preferences
  ├── search_history
  ├── reading_lists
  └── reading_list_items

komorebi_admin   (3 bang)   ← AdminService (:5005)
  ├── categories
  ├── settings
  └── posts
```

**Luu y:** AdminService ket noi **doc** tu nhieu database khac de hien thi dashboard:

- `DefaultConnection` → komorebi_admin (bang rieng cua admin)
- `UserDb` → komorebi_auth (doc users de quan ly)
- `MangaDb` → komorebi_manga (doc manga de thong ke)
- `CommentDb` → komorebi_comment (doc comments de quan ly)

Tuong tu, UserService cung doc tu `MangaDb` va `CommentDb` (hien thi thong ke profile).

### Tao Database

Chay 1 lenh duy nhat:

```bash
psql -U postgres -f webtruyendb.sql
```
```bash
"C:\Program Files\PostgreSQL\18\bin\psql.exe" -U postgres -f webtruyendb.sql
& "C:\Program Files\PostgreSQL\18\bin\psql.exe" -U postgres -f webtruyendb.sql
```
File SQL se tu dong tao 5 databases va toan bo 23 bang.

---

## 2. Cau Hinh Tung Service

> **Quy uoc**: Gia tri `postgres` trong connection string la password mac dinh. Doi thanh password that cua ban.

### 2.1 API Gateway

**File:** `Komorebi-api/src/Gateway/ApiGateway/appsettings.json`

| Dong | Key                                     | Gia tri mac dinh                                | Can doi?                             |
| ---- | --------------------------------------- | ----------------------------------------------- | ------------------------------------ |
| 20   | `Jwt:Secret`                            | `super-secret-key-minimum-32-characters-long!!` | **Co** — phai giong o TAT CA service |
| 21   | `Jwt:Issuer`                            | `komorebi-auth`                                 | Phai giong tat ca service            |
| 22   | `Jwt:Audience`                          | `komorebi-api`                                  | Phai giong tat ca service            |
| 25   | `Cors:AllowedOrigins[0]`                | `http://localhost:3000`                         | Doi neu frontend chay port khac      |
| 28   | `ServiceUrls:AuthService`               | `http://localhost:5001`                         | URL Auth Service                     |
| 29   | `ServiceUrls:MangaService`              | `http://localhost:5002`                         | URL Manga Service                    |
| 30   | `ServiceUrls:CommentService`            | `http://localhost:5003`                         | URL Comment Service                  |
| 31   | `ServiceUrls:UserService`               | `http://localhost:5004`                         | URL User Service                     |
| 32   | `ServiceUrls:AdminService`              | `http://localhost:5005`                         | URL Admin Service                    |
| 90   | `ReverseProxy:Clusters:auth-cluster`    | `http://localhost:5001`                         | Proxy den Auth                       |
| 97   | `ReverseProxy:Clusters:manga-cluster`   | `http://localhost:5002`                         | Proxy den Manga                      |
| 104  | `ReverseProxy:Clusters:comment-cluster` | `http://localhost:5003`                         | Proxy den Comment                    |
| 111  | `ReverseProxy:Clusters:user-cluster`    | `http://localhost:5004`                         | Proxy den User                       |
| 118  | `ReverseProxy:Clusters:admin-cluster`   | `http://localhost:5005`                         | Proxy den Admin                      |

> Gateway **khong co** PostgreSQL connection — chi proxy request.

---

### 2.2 Auth Service

**File:** `Komorebi-api/src/Services/AuthService/AuthService.Api/appsettings.json`

| Dong | Key                                   | Gia tri mac dinh                                                                      | Can doi?                        |
| ---- | ------------------------------------- | ------------------------------------------------------------------------------------- | ------------------------------- |
| 3    | `ConnectionStrings:DefaultConnection` | `Host=localhost;Port=5432;Database=komorebi_auth;Username=postgres;Password=postgres` | **Co** — doi Password           |
| 4    | `ConnectionStrings:Redis`             | `localhost:6379`                                                                      | Doi neu Redis co password       |
| 7    | `Jwt:Secret`                          | `super-secret-key-minimum-32-characters-long!!`                                       | **Phai giong Gateway**          |
| 8    | `Jwt:Issuer`                          | `komorebi-auth`                                                                       | Phai giong Gateway              |
| 9    | `Jwt:Audience`                        | `komorebi-api`                                                                        | Phai giong Gateway              |
| 10   | `Jwt:AccessTokenExpiryMinutes`        | `15`                                                                                  | Thoi gian het han access token  |
| 11   | `Jwt:RefreshTokenExpiryDays`          | `30`                                                                                  | Thoi gian het han refresh token |
| 14   | `Google:ClientId`                     | `your-google-client-id...`                                                            | Tuy chon — Google OAuth         |
| 17   | `RabbitMQ:Host`                       | `localhost`                                                                           | Doi neu RabbitMQ o may khac     |
| 18   | `RabbitMQ:User`                       | `guest`                                                                               | RabbitMQ username               |
| 19   | `RabbitMQ:Pass`                       | `guest`                                                                               | RabbitMQ password               |

---

### 2.3 Manga Service

**File:** `Komorebi-api/src/Services/MangaService/MangaService.Api/appsettings.json`

| Dong   | Key                                   | Gia tri mac dinh                                  | Can doi?                                 |
| ------ | ------------------------------------- | ------------------------------------------------- | ---------------------------------------- |
| 3      | `ConnectionStrings:DefaultConnection` | `...Database=komorebi_manga;...Password=postgres` | **Co** — doi Password                    |
| 4      | `ConnectionStrings:Redis`             | `localhost:6379`                                  | Doi neu Redis co password                |
| 7      | `Jwt:Secret`                          | (giong Auth)                                      | **Phai giong Gateway**                   |
| 8      | `Jwt:Issuer`                          | `komorebi-auth`                                   | Phai giong Gateway                       |
| 9      | `Jwt:Audience`                        | `komorebi-api`                                    | Phai giong Gateway                       |
| 11     | `RabbitMQ:Host`                       | `localhost`                                       |                                          |
| 12     | `RabbitMQ:User`                       | `guest`                                           |                                          |
| 13     | `RabbitMQ:Pass`                       | `guest`                                           |                                          |
| **17** | **`Storage:Provider`**                | **`"local"`**                                     | **`local` / `minio` / `s3`** (xem muc 5) |
| 19     | `Storage:Local:BasePath`              | `D:/komorebi-storage`                             | Thu muc luu anh tren o dia               |
| 20     | `Storage:Local:BaseUrl`               | `http://localhost:5002/storage`                   | URL truy cap anh                         |
| 22     | `Storage:MinIO:Endpoint`              | `localhost:9000`                                  | MinIO endpoint                           |
| 23     | `Storage:MinIO:AccessKey`             | `minioadmin`                                      | MinIO access key                         |
| 24     | `Storage:MinIO:SecretKey`             | `minioadmin`                                      | MinIO secret key                         |
| 25     | `Storage:MinIO:UseSSL`                | `false`                                           |                                          |
| 26     | `Storage:MinIO:CdnBaseUrl`            | `""`                                              | CDN URL (tuy chon)                       |
| 29     | `Storage:S3:Region`                   | `auto`                                            | AWS region                               |
| 30     | `Storage:S3:BucketPrefix`             | `komorebi-`                                       | Tien to ten bucket                       |
| 31     | `Storage:S3:Endpoint`                 | `""`                                              | Endpoint (R2/custom)                     |
| 32     | `Storage:S3:AccessKey`                | `""`                                              | AWS access key                           |
| 33     | `Storage:S3:SecretKey`                | `""`                                              | AWS secret key                           |

---

### 2.4 Comment Service

**File:** `Komorebi-api/src/Services/CommentService/CommentService.Api/appsettings.json`

| Dong | Key                                   | Gia tri mac dinh                                    | Can doi?               |
| ---- | ------------------------------------- | --------------------------------------------------- | ---------------------- |
| 3    | `ConnectionStrings:DefaultConnection` | `...Database=komorebi_comment;...Password=postgres` | **Co** — doi Password  |
| 4    | `ConnectionStrings:MangaDb`           | `...Database=komorebi_manga;...Password=postgres`   | **Co** — doi Password  |
| 5    | `ConnectionStrings:Redis`             | `localhost:6379`                                    |                        |
| 8    | `Jwt:Secret`                          | (giong)                                             | **Phai giong Gateway** |
| 12   | `RabbitMQ:Host`                       | `localhost`                                         |                        |
| 13   | `RabbitMQ:User`                       | `guest`                                             |                        |
| 14   | `RabbitMQ:Pass`                       | `guest`                                             |                        |

---

### 2.5 User Service

**File:** `Komorebi-api/src/Services/UserService/UserService.Api/appsettings.json`

| Dong   | Key                                   | Gia tri mac dinh                                    | Can doi?                                 |
| ------ | ------------------------------------- | --------------------------------------------------- | ---------------------------------------- |
| 3      | `ConnectionStrings:DefaultConnection` | `...Database=komorebi_user;...Password=postgres`    | **Co** — doi Password                    |
| 4      | `ConnectionStrings:MangaDb`           | `...Database=komorebi_manga;...Password=postgres`   | **Co** — doi Password                    |
| 5      | `ConnectionStrings:CommentDb`         | `...Database=komorebi_comment;...Password=postgres` | **Co** — doi Password                    |
| 6      | `ConnectionStrings:Redis`             | `localhost:6379`                                    |                                          |
| 9      | `Jwt:Secret`                          | (giong)                                             | **Phai giong Gateway**                   |
| **14** | **`Storage:Provider`**                | **`"local"`**                                       | **`local` / `minio` / `s3`** (xem muc 5) |
| 16     | `Storage:Local:BasePath`              | `D:/komorebi-storage`                               | Thu muc avatar                           |
| 17     | `Storage:Local:BaseUrl`               | `http://localhost:5001/storage`                     | URL avatar                               |
| 19-24  | `Storage:MinIO:*`                     | (giong MangaService)                                |                                          |
| 26-31  | `Storage:S3:*`                        | (giong MangaService)                                |                                          |
| 34     | `RabbitMQ:Host`                       | `localhost`                                         |                                          |
| 35     | `RabbitMQ:User`                       | `guest`                                             |                                          |
| 36     | `RabbitMQ:Pass`                       | `guest`                                             |                                          |

---

### 2.6 Admin Service

**File:** `Komorebi-api/src/Services/AdminService/AdminService.Api/appsettings.json`

| Dong | Key                                   | Gia tri mac dinh                                    | Can doi?               |
| ---- | ------------------------------------- | --------------------------------------------------- | ---------------------- |
| 3    | `ConnectionStrings:DefaultConnection` | `...Database=komorebi_admin;...Password=postgres`   | **Co** — doi Password  |
| 4    | `ConnectionStrings:UserDb`            | `...Database=komorebi_auth;...Password=postgres`    | **Co** — doi Password  |
| 5    | `ConnectionStrings:MangaDb`           | `...Database=komorebi_manga;...Password=postgres`   | **Co** — doi Password  |
| 6    | `ConnectionStrings:CommentDb`         | `...Database=komorebi_comment;...Password=postgres` | **Co** — doi Password  |
| 7    | `ConnectionStrings:Redis`             | `localhost:6379`                                    |                        |
| 10   | `Jwt:Secret`                          | (giong)                                             | **Phai giong Gateway** |

> Admin Service la service duy nhat **KHONG dung RabbitMQ**.

---

### 2.7 Notification Service

**File:** `Komorebi-api/src/Services/NotificationService/NotificationService.Api/appsettings.json`

| Dong | Key                       | Gia tri mac dinh                                | Can doi?                   |
| ---- | ------------------------- | ----------------------------------------------- | -------------------------- |
| 3    | `ConnectionStrings:Redis` | `localhost:6379`                                |                            |
| 6    | `Jwt:Issuer`              | `komorebi-auth`                                 | **Phai giong Gateway**     |
| 7    | `Jwt:Audience`            | `komorebi-api`                                  | **Phai giong Gateway**     |
| 8    | `Jwt:SecretKey`           | (giong Jwt:Secret o cac service khac)           | **Phai giong Gateway**     |
| 10   | `RabbitMQ:Host`           | `localhost`                                     |                            |
| 11   | `RabbitMQ:User`           | `guest`                                         |                            |
| 12   | `RabbitMQ:Pass`           | `guest`                                         |                            |
| 16   | `Resend:ApiKey`           | `re_CHANGE_THIS`                                | API key Resend (gui email) |
| 17   | `Resend:FromEmail`        | `noreply@komorebi.vn`                           | Email nguoi gui            |
| 21   | `Email:SiteUrl`           | `https://komorebi.vn`                           | URL website                |
| 22   | `Email:UnsubscribeSecret` | `CHANGE_THIS...`                                | Secret huy dang ky email   |

> Notification Service **khong co PostgreSQL** — chi dung Redis + RabbitMQ + SignalR.

---

### 2.8 Background Worker

**File:** `Komorebi-api/src/Services/BackgroundWorker/BackgroundWorker.Worker/appsettings.json`

| Dong | Key                                | Gia tri mac dinh                                  | Can doi?              |
| ---- | ---------------------------------- | ------------------------------------------------- | --------------------- |
| 3    | `ConnectionStrings:MangaDb`        | `...Database=komorebi_manga;...Password=postgres` | **Co** — doi Password |
| 4    | `ConnectionStrings:UserDb`         | `...Database=komorebi_user;...Password=postgres`  | **Co** — doi Password |
| 7    | `RabbitMQ:Host`                    | `localhost`                                       |                       |
| 8    | `RabbitMQ:User`                    | `guest`                                           |                       |
| 9    | `RabbitMQ:Pass`                    | `guest`                                           |                       |

---

### 2.9 Frontend (Next.js)

**File:** `komorebi-web/.env.local`

| Dong | Key                    | Gia tri mac dinh        | Can doi?                     |
| ---- | ---------------------- | ----------------------- | ---------------------------- |
| 2    | `NEXT_PUBLIC_API_URL`  | `http://localhost:5000` | URL API Gateway              |
| 3    | `NEXT_PUBLIC_SITE_URL` | `http://localhost:3000` | URL frontend                 |
| 6    | `NEXTAUTH_URL`         | `http://localhost:3000` | NextAuth URL                 |
| 7    | `NEXTAUTH_SECRET`      | _(trong)_               | **Co** — secret cho NextAuth |
| 10   | `GOOGLE_CLIENT_ID`     | _(trong)_               | Tuy chon — Google OAuth      |
| 11   | `GOOGLE_CLIENT_SECRET` | _(trong)_               | Tuy chon — Google OAuth      |

---

## 3. Chay Bang Docker Compose

### 3.1 Chay Toan Bo

Khi chay Docker Compose day du, **khong can sua appsettings.json** — docker-compose.yml da override bang environment variables. Chi can sua file `.env`:

```bash
cd infra
cp .env.example .env
```

Sua file `infra/.env` — cac key SENSITIVE bat buoc:

| Key                 | Mo ta                         | Vi du                 |
| ------------------- | ----------------------------- | --------------------- |
| `POSTGRES_USER`     | Database username             | `manga`               |
| `POSTGRES_PASSWORD` | Database password             | `postgres123`         |
| `REDIS_PASSWORD`    | Redis password                | `redis123`            |
| `RABBITMQ_USER`     | RabbitMQ username             | `manga`               |
| `RABBITMQ_PASSWORD` | RabbitMQ password             | `rabbit123`           |
| `JWT_SECRET`        | JWT signing key (>= 48 ky tu) | `my-super-secret-...` |
| `MINIO_ACCESS_KEY`  | MinIO access key              | `minioadmin`          |
| `MINIO_SECRET_KEY`  | MinIO secret key (>= 8 ky tu) | `minioadmin123`       |

Cac key tuy chon:

| Key                      | Mo ta           | Mac dinh   |
| ------------------------ | --------------- | ---------- |
| `GOOGLE_CLIENT_ID`       | Google OAuth    | _(trong)_  |
| `GOOGLE_CLIENT_SECRET`   | Google OAuth    | _(trong)_  |
| `GRAFANA_ADMIN_PASSWORD` | Grafana admin   | `admin123` |
| `CDN_BASE_URL`           | CDN URL cho anh | _(trong)_  |

```bash
# Khoi chay toan bo
docker compose --env-file .env up -d

# Kiem tra
docker compose ps

# Xem logs
docker compose logs -f api-gateway
```

**Truy cap:**
| Dich vu | URL |
|---------|-----|
| Frontend | http://localhost:3000 |
| API Gateway | http://localhost:5000 |
| RabbitMQ Management | http://localhost:15672 |
| MinIO Console | http://localhost:9001 |
| Grafana | http://localhost:3001 |
| Jaeger UI | http://localhost:16686 |

---

### 3.2 Chi Chay Infrastructure (de backend local)

Neu muon chay code .NET + Next.js truc tiep tren may, chi Docker cho PostgreSQL/Redis/RabbitMQ:

```bash
cd infra
docker compose --env-file .env up -d postgres pgbouncer redis rabbitmq
```

Luc nay can sua appsettings.json de tro vao Docker containers:

**Connection strings khi dung Docker infrastructure:**

```
# PostgreSQL (qua PgBouncer, port 6432 tren host)
Host=localhost;Port=6432;Database=komorebi_xxx;Username=<POSTGRES_USER>;Password=<POSTGRES_PASSWORD>

# Hoac truc tiep PostgreSQL (port 5432)
Host=localhost;Port=5432;Database=komorebi_xxx;Username=<POSTGRES_USER>;Password=<POSTGRES_PASSWORD>

# Redis (co password)
localhost:6379,password=<REDIS_PASSWORD>

# RabbitMQ
Host: localhost
User: <RABBITMQ_USER>
Pass: <RABBITMQ_PASSWORD>
```

Doi trong **tat ca** appsettings.json:

- Dong `"ConnectionStrings:DefaultConnection"` → doi Username/Password
- Dong `"ConnectionStrings:Redis"` → them `,password=xxx` neu co
- Dong `"RabbitMQ:User"/"RabbitMQ:Pass"` → doi theo .env

---

## 4. Chay Local Tren Windows (Khong Docker)

### 4.1 Yeu Cau Bat Buoc

Hien tai code **BAT BUOC** can Redis va RabbitMQ. Ca hai se throw exception neu thieu.
Cac dong code throw exception:

- **Redis**: `Komorebi.Infrastructure/DependencyInjection.cs` dong 23

  ```csharp
  var connectionString = configuration.GetConnectionString("Redis")
      ?? throw new InvalidOperationException("Redis connection string is not configured.");
  ```

- **RabbitMQ**: `Komorebi.Infrastructure/DependencyInjection.cs` dong 110
  ```csharp
  var rabbitOptions = configuration.GetSection(...).Get<RabbitMqOptions>()
      ?? throw new InvalidOperationException("RabbitMQ options are not configured.");
  ```

### 4.2 Cai Dat

1. **PostgreSQL 16** — tai tu https://www.postgresql.org/download/
2. **Redis** — Windows khong ho tro chinh thuc. Dung 1 trong:
   - [Memurai](https://www.memurai.com/) (Redis cho Windows)
   - Redis qua WSL: `wsl --install` roi `sudo apt install redis-server`
   - Docker chi cho Redis: `docker run -d -p 6379:6379 redis:7-alpine`
3. **RabbitMQ** — tai tu https://www.rabbitmq.com/download.html (can cai Erlang truoc)
   - Hoac Docker: `docker run -d -p 5672:5672 -p 15672:15672 rabbitmq:3.13-management-alpine`

**Cach de nhat:** Chi Docker cho 3 infrastructure services:

```bash
docker run -d --name pg -e POSTGRES_PASSWORD=postgres -p 5432:5432 postgres:16-alpine
docker run -d --name redis -p 6379:6379 redis:7-alpine
docker run -d --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3.13-management-alpine
```

### 4.3 Tao Database

```bash
psql -U postgres -f webtruyendb.sql
```

Lenh nay tao 5 databases va 23 bang tu dong.

### 4.4 Cau Hinh

Neu dung gia tri mac dinh (postgres/postgres, Redis khong password, RabbitMQ guest/guest) thi **khong can sua gi** — cac appsettings.json da co gia tri mac dinh phu hop.

Neu doi password PostgreSQL, doi **tat ca** file sau:

| File                                | Dong can doi                                           |
| ----------------------------------- | ------------------------------------------------------ |
| AuthService `appsettings.json`      | Dong 3: `Password=xxx`                                 |
| MangaService `appsettings.json`     | Dong 3: `Password=xxx`                                 |
| CommentService `appsettings.json`   | Dong 3, 4: `Password=xxx` (2 connection strings)       |
| UserService `appsettings.json`      | Dong 3, 4, 5: `Password=xxx` (3 connection strings)    |
| AdminService `appsettings.json`     | Dong 3, 4, 5, 6: `Password=xxx` (4 connection strings) |
| BackgroundWorker `appsettings.json` | Dong 3, 4: `Password=xxx` (2 connection strings)       |

### 4.5 Chay

```bash
# 1. Tao thu muc storage
mkdir D:\komorebi-storage

# 2. Gateway (bat buoc chay dau tien)
cd Komorebi-api/src/Gateway/ApiGateway
dotnet run
# Chay tren http://localhost:5000

# 3. Auth Service (terminal moi)
cd Komorebi-api/src/Services/AuthService/AuthService.Api
dotnet run
# Chay tren http://localhost:5001

# 4. Manga Service (terminal moi)
cd Komorebi-api/src/Services/MangaService/MangaService.Api
dotnet run
# Chay tren http://localhost:5002

# 5. Comment Service (terminal moi)
cd Komorebi-api/src/Services/CommentService/CommentService.Api
dotnet run
# Chay tren http://localhost:5003

# 6. User Service (terminal moi)
cd Komorebi-api/src/Services/UserService/UserService.Api
dotnet run
# Chay tren http://localhost:5004

# 7. Admin Service (terminal moi)
cd Komorebi-api/src/Services/AdminService/AdminService.Api
dotnet run
# Chay tren http://localhost:5005

# 8. Frontend (terminal moi)
cd komorebi-web
npm install
npm run dev
# Chay tren http://localhost:3000
```

**Thu tu khuyen nghi:** Gateway → Auth → Manga → (phan con lai bat ky) → Frontend.

Notification Service va Background Worker la tuy chon:

- **Notification Service**: Can RabbitMQ + Redis. Chi can khi muon thong bao real-time.
- **Background Worker**: Xu ly background jobs. Chi can khi muon cap nhat thong ke tu dong.

---

## 5. Cau Hinh Storage (Local / MinIO / S3)

Chi **2 service** dung storage: **MangaService** (anh bia + anh chuong) va **UserService** (avatar).

Doi key `Storage:Provider` trong appsettings.json cua CA 2 service.

### 5.1 Local Storage (mac dinh — khong can MinIO/S3)

Anh duoc luu truc tiep vao o dia.

**MangaService** `appsettings.json`:

```json
// Dong 17 — doi provider
"Provider": "local",

// Dong 19 — duong dan thu muc
"BasePath": "D:/komorebi-storage",

// Dong 20 — URL truy cap anh
"BaseUrl": "http://localhost:5002/storage"
```

**UserService** `appsettings.json`:

```json
// Dong 14 — doi provider
"Provider": "local",

// Dong 16 — duong dan thu muc
"BasePath": "D:/komorebi-storage",

// Dong 17 — URL truy cap avatar
"BaseUrl": "http://localhost:5001/storage"
```

Tao thu muc truoc: `mkdir D:\komorebi-storage`

### 5.2 MinIO

**MangaService** `appsettings.json`:

```json
// Dong 17 — doi
"Provider": "minio",

// Dong 22-26 — dien thong tin MinIO
"Endpoint": "localhost:9000",
"AccessKey": "minioadmin",
"SecretKey": "minioadmin",
"UseSSL": false,
"CdnBaseUrl": ""                    // CDN URL neu co (VD: https://cdn.yourdomain.com)
```

**UserService** `appsettings.json`: tuong tu, doi dong 14 thanh `"minio"`, dong 19-24.

Chay MinIO:

```bash
docker run -d -p 9000:9000 -p 9001:9001 \
  -e MINIO_ROOT_USER=minioadmin \
  -e MINIO_ROOT_PASSWORD=minioadmin \
  minio/minio server /data --console-address ":9001"
```

MinIO Console: http://localhost:9001

### 5.3 AWS S3 / Cloudflare R2

**MangaService** `appsettings.json`:

```json
// Dong 17 — doi
"Provider": "s3",

// Dong 29-33 — dien thong tin S3
"Region": "ap-southeast-1",        // AWS region (hoac "auto" cho R2)
"BucketPrefix": "komorebi-",
"Endpoint": "",                     // Trong cho AWS, dien URL cho R2
"AccessKey": "AKIA...",
"SecretKey": "..."
```

**UserService** `appsettings.json`: tuong tu, doi dong 14 + dong 26-31.

**Cho Cloudflare R2:**

```json
"Region": "auto",
"Endpoint": "https://<account-id>.r2.cloudflarestorage.com",
"AccessKey": "<R2-access-key>",
"SecretKey": "<R2-secret-key>"
```

### 5.4 Bang Tom Tat Storage

| Provider | Doi o dau                  | MangaService dong | UserService dong |
| -------- | -------------------------- | ----------------- | ---------------- |
| `local`  | Chi `BasePath` + `BaseUrl` | 17, 19, 20        | 14, 16, 17       |
| `minio`  | MinIO Endpoint/Key         | 17, 22-26         | 14, 19-24        |
| `s3`     | S3 Region/Key/Endpoint     | 17, 29-33         | 14, 26-31        |

**Logic chuyen doi** nam tai: `Komorebi.Infrastructure/DependencyInjection.cs` dong 37-90

```csharp
var provider = configuration["Storage:Provider"] ?? "local";
switch (provider.ToLowerInvariant())
{
    case "local":  → LocalFileStorageService
    case "minio":  → MinioStorageService
    case "s3":     → S3StorageService
    default:       → throw ArgumentException
}
```

---

## 6. Quy Tac Quan Trong

### JWT Secret phai giong nhau

Chuoi JWT secret phai **GIONG HET** trong tat ca 7 file appsettings.json (Gateway + 6 services). Neu khac nhau, token tu Auth Service se bi reject o cac service khac.

Cac file can kiem tra:

1. `Gateway/ApiGateway/appsettings.json` dong 20 — key: `Jwt:Secret`
2. `AuthService/AuthService.Api/appsettings.json` dong 7 — key: `Jwt:Secret`
3. `MangaService/MangaService.Api/appsettings.json` dong 7 — key: `Jwt:Secret`
4. `CommentService/CommentService.Api/appsettings.json` dong 8 — key: `Jwt:Secret`
5. `UserService/UserService.Api/appsettings.json` dong 9 — key: `Jwt:Secret`
6. `AdminService/AdminService.Api/appsettings.json` dong 10 — key: `Jwt:Secret`
7. `NotificationService/NotificationService.Api/appsettings.json` dong 8 — key: `Jwt:SecretKey` (**ten khac**, gia tri phai giong)

> **Luu y:** NotificationService dung ten key `Jwt:SecretKey` (thay vi `Jwt:Secret`) vi Program.cs cua no doc `builder.Configuration["Jwt:SecretKey"]`. Gia tri phai trung khop voi cac service con lai.

### PostgreSQL password phai nhat quan

Neu doi password PostgreSQL, phai doi **tat ca** connection strings trong tat ca appsettings.json. Tong cong khoang **14 connection strings** phan tan o 6 file.

### Service nao dung gi

| Service           | PostgreSQL                                      | Redis  | RabbitMQ | Storage |
| ----------------- | ----------------------------------------------- | ------ | -------- | ------- |
| Gateway           | Khong                                           | Khong  | Khong    | Khong   |
| Auth              | `komorebi_auth`                                 | **Co** | **Co**   | Khong   |
| Manga             | `komorebi_manga`                                | **Co** | **Co**   | **Co**  |
| Comment           | `komorebi_comment` + `manga`                    | **Co** | **Co**   | Khong   |
| User              | `komorebi_user` + `manga` + `comment`           | **Co** | **Co**   | **Co**  |
| Admin             | `komorebi_admin` + `auth` + `manga` + `comment` | **Co** | Khong    | Khong   |
| Notification      | Khong                                           | **Co** | **Co**   | Khong   |
| Background Worker | `komorebi_manga` + `komorebi_user`               | Khong  | **Co**   | Khong   |

### Tat Redis / RabbitMQ?

Hien tai **KHONG THE TAT**. Ca hai throw exception khi thieu config. Neu muon chay khong can Redis/RabbitMQ, can sua code trong `Komorebi.Infrastructure/DependencyInjection.cs` de tao fallback (VD: in-memory cache thay Redis, no-op event bus thay RabbitMQ).

Cach de nhat la chay chung qua Docker:

```bash
docker run -d -p 6379:6379 redis:7-alpine
docker run -d -p 5672:5672 rabbitmq:3.13-management-alpine
```
