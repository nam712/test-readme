# Komorebi - Web Truyen Tranh

**Komorebi** (木漏れ日) la nen tang doc truyen tranh truc tuyen danh cho nguoi doc Viet Nam, xay dung tren kien truc microservices voi .NET 8 va Next.js 14.

[![.NET](https://img.shields.io/badge/.NET-8.0-512BD4?logo=dotnet)](https://dotnet.microsoft.com/)
[![Next.js](https://img.shields.io/badge/Next.js-14-000000?logo=nextdotjs)](https://nextjs.org/)
[![React](https://img.shields.io/badge/React-18-61DAFB?logo=react)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript)](https://www.typescriptlang.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-4169E1?logo=postgresql)](https://www.postgresql.org/)
[![Redis](https://img.shields.io/badge/Redis-7-DC382D?logo=redis)](https://redis.io/)
[![RabbitMQ](https://img.shields.io/badge/RabbitMQ-3.13-FF6600?logo=rabbitmq)](https://www.rabbitmq.com/)
[![TailwindCSS](https://img.shields.io/badge/TailwindCSS-3-06B6D4?logo=tailwindcss)](https://tailwindcss.com/)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker)](https://docs.docker.com/compose/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

---

## Muc Luc

- [Kien Truc He Thong](#kien-truc-he-thong)
- [Cong Nghe Su Dung](#cong-nghe-su-dung)
- [Tinh Nang](#tinh-nang)
- [Yeu Cau He Thong](#yeu-cau-he-thong)
- [Khoi Chay Nhanh voi Docker Compose](#khoi-chay-nhanh-voi-docker-compose)
- [Cai Dat Moi Truong Phat Trien (Khong Docker)](#cai-dat-moi-truong-phat-trien-khong-docker)
- [Bien Moi Truong](#bien-moi-truong)
- [API Routes](#api-routes)
- [Cau Truc Du An](#cau-truc-du-an)
- [Dong Gop](#dong-gop)
- [Giay Phep](#giay-phep)

---

## Kien Truc He Thong

```
                                  +------------------+
                                  |   Next.js 14     |
                                  |   Frontend       |
                                  |   :3000          |
                                  +--------+---------+
                                           |
                                           | HTTP / WebSocket
                                           |
                                  +--------v---------+
                                  |   API Gateway    |
                                  |   (YARP)         |
                                  |   :5000          |
                                  |   JWT | Rate     |
                                  |   Limit | CB     |
                                  +--------+---------+
                                           |
                 +----------+---------+----+----+---------+----------+
                 |          |         |         |         |          |
          +------v--+ +----v----+ +--v------+ +v-------+ +v------+ +v-----------+
          |  Auth   | |  Manga  | | Comment | |  User  | | Admin | | Notification|
          | Service | | Service | | Service | |Service | |Service| |  Service    |
          |  :5001  | |  :5002  | |  :5003  | | :5004  | | :5005 | |   :5007     |
          +----+----+ +----+----+ +---+-----+ +---+----+ +---+---+ +------+------+
               |           |          |           |          |             |
               v           v          v           v          v             |
          +---------+ +---------+ +----------+ +--------+ +--------+     |
          |komorebi | |komorebi | |komorebi  | |komorebi| |komorebi|     |
          |  _auth  | | _manga  | | _comment | | _user  | | _admin |     |
          +---------+ +---------+ +----------+ +--------+ +--------+     |
               |           |          |           |          |             |
               +-----+-----+----+----+-----+-----+----------+             |
                     |          |          |                               |
                     v          v          v                               |
               +---------+ +--------+ +----------+                        |
               |PostgreSQL| | Redis  | | RabbitMQ |<-----------------------+
               |   16     | |   7    | |  3.13    |
               +---------+ +--------+ | MassTransit|
                                      +----------+
                                           |
                                      +----v-----+
                                      |  Storage  |
                                      | Local /   |
                                      | MinIO / S3|
                                      +----------+

  Observability: OpenTelemetry --> Prometheus --> Grafana
                                   Jaeger (Distributed Tracing)
  Real-time:    SignalR Hub + Redis Backplane
```

---

## Cong Nghe Su Dung

### Backend

| Thanh phan       | Cong nghe                                     |
| ---------------- | --------------------------------------------- |
| Runtime          | .NET 8                                        |
| Architecture     | Clean Architecture (CQRS + MediatR)           |
| API Gateway      | YARP Reverse Proxy                            |
| Authentication   | JWT Bearer + Google OAuth                     |
| Database         | PostgreSQL 16 (per-service database)          |
| Full-text Search | PostgreSQL tsvector + unaccent (Vietnamese)   |
| Caching          | Redis 7                                       |
| Message Queue    | RabbitMQ 3.13 + MassTransit                   |
| Real-time        | SignalR + Redis Backplane                     |
| Object Storage   | Local Disk / MinIO / AWS S3 (pluggable)       |
| Observability    | OpenTelemetry + Prometheus + Grafana + Jaeger |

### Frontend

| Thanh phan       | Cong nghe               |
| ---------------- | ----------------------- |
| Framework        | Next.js 14 (App Router) |
| UI Library       | React 18                |
| Language         | TypeScript 5            |
| Styling          | TailwindCSS + shadcn/ui |
| State Management | Zustand                 |
| Data Fetching    | @tanstack/react-query   |
| Tables           | @tanstack/react-table   |
| Forms            | react-hook-form + zod   |
| Charts           | Recharts                |
| Drag & Drop      | @dnd-kit                |
| Real-time Client | @microsoft/signalr      |
| Icons            | lucide-react            |
| PWA              | next-pwa                |

---

## Tinh Nang

### Doc Truyen

- Tim kiem truyen tieng Viet voi full-text search (unaccent + tsvector), ho tro tim kiem khong dau
- Doc truyen voi tuy chinh huong doc (doc ngang / doc doc), zoom, cuon tu dong
- Danh dau trang, theo doi truyen de nhan thong bao chuong moi
- Danh sach doc tuy chinh: 5 danh sach mac dinh moi nguoi dung + khong gioi han danh sach tu tao
- Progressive Web App (PWA) -- cai dat nhu ung dung tren dien thoai

### He Thong Uploader

- Dang ky lam uploader voi he thong duyet don
- Upload truyen va chuong voi nhieu backend luu tru (local / MinIO / S3)
- Kiem duyet noi dung: tu dong duyet, cho xet duyet, tu choi

### Cong Dong

- Binh luan theo chuong voi tra loi va like
- Bao cao binh luan vi pham
- Thong bao real-time qua SignalR (chuong moi, tra loi binh luan, ...)
- Thong bao qua email

### Quan Tri

- Dashboard thong ke (so luong truyen, nguoi dung, luot xem, ...)
- Quan ly nguoi dung, phan quyen
- Xet duyet noi dung va xu ly bao cao
- Quan ly cau hinh he thong

---

## Yeu Cau He Thong

### Chay voi Docker (khuyen nghi)

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) >= 4.x
- [Docker Compose](https://docs.docker.com/compose/) >= 2.x

### Phat trien khong Docker

- [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0)
- [Node.js](https://nodejs.org/) >= 20.x
- [PostgreSQL](https://www.postgresql.org/download/) >= 16
- [Redis](https://redis.io/download/) >= 7 (tuy chon -- co the tat caching)
- [RabbitMQ](https://www.rabbitmq.com/download.html) >= 3.13 (tuy chon -- chi can khi su dung inter-service events)

---

## Khoi Chay Nhanh voi Docker Compose

1. **Clone repository:**

   ```bash
   git clone <repository-url> truyenfull
   cd truyenfull
   ```

2. **Tao file `.env` tu template:**

   ```bash
   cp infra/.env.example infra/.env
   ```

   Chinh sua `infra/.env` voi cac gia tri phu hop (xem bang [Bien Moi Truong](#bien-moi-truong) ben duoi).

3. **Khoi chay toan bo he thong:**

   ```bash
   cd infra
   docker-compose up -d
   ```

4. **Kiem tra trang thai:**

   ```bash
   docker-compose ps
   ```

5. **Truy cap ung dung:**

   | Dich vu             | URL                    |
   | ------------------- | ---------------------- |
   | Frontend            | http://localhost:3000  |
   | API Gateway         | http://localhost:5000  |
   | Grafana             | http://localhost:3001  |
   | Jaeger UI           | http://localhost:16686 |
   | RabbitMQ Management | http://localhost:15672 |

---

## Cai Dat Moi Truong Phat Trien (Khong Docker)

Huong dan nay danh cho lap trinh vien muon chay tung service truc tiep tren may, khong qua Docker.

### Buoc 1: Tao Databases

Ket noi vao PostgreSQL va tao 5 databases:

```sql
CREATE DATABASE komorebi_auth;
CREATE DATABASE komorebi_manga;
CREATE DATABASE komorebi_comment;
CREATE DATABASE komorebi_user;
CREATE DATABASE komorebi_admin;
```

Cai dat extension `unaccent` cho database manga (bat buoc cho full-text search tieng Viet):

```sql
\c komorebi_manga
CREATE EXTENSION IF NOT EXISTS unaccent;
```

Neu co file `webtruyendb.sql`, chay de tao schema:

```bash
psql -U postgres -f webtruyendb.sql
```

### Buoc 2: Cau Hinh appsettings

Moi service co file `appsettings.Development.json`. Cap nhat connection strings cho phu hop voi PostgreSQL local:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Host=localhost;Port=5432;Database=komorebi_<service>;Username=postgres;Password=your_password"
  },
  "Jwt": {
    "SecretKey": "your-secret-key-at-least-32-characters-long",
    "Issuer": "komorebi",
    "Audience": "komorebi-client",
    "ExpiryInMinutes": 60
  }
}
```

**Cau hinh Storage** (chi can cho Manga Service):

```json
{
  "Storage": {
    "Provider": "local",
    "Local": {
      "BasePath": "D:/komorebi-storage",
      "BaseUrl": "http://localhost:5002/storage"
    },
    "MinIO": {
      "Endpoint": "localhost:9000",
      "AccessKey": "minioadmin",
      "SecretKey": "minioadmin"
    },
    "S3": {
      "Region": "auto",
      "BucketPrefix": "komorebi-",
      "Endpoint": "",
      "AccessKey": "",
      "SecretKey": ""
    }
  }
}
```

### Buoc 3: Cau Hinh Redis va RabbitMQ (Tuy Chon)

**Redis** (tuy chon): Neu khong cai Redis, tat cac cau hinh caching trong tung service. He thong van hoat dong binh thuong nhung se khong co cache.

**RabbitMQ** (tuy chon): Neu khong cai RabbitMQ, cac su kien giua cac service (vd: thong bao chuong moi) se khong hoat dong. Cac chuc nang trong tung service van chay doc lap.

Neu can, cai dat va chay:

```bash
# Redis
redis-server

# RabbitMQ (sau khi cai dat)
rabbitmq-server
```

### Buoc 4: Chay Backend Services

Mo terminal rieng cho tung service (hoac su dung `dotnet watch` de hot-reload):

```bash
# API Gateway (bat buoc -- diem vao chinh)
cd Komorebi-api/src/Gateway/ApiGateway
dotnet run

# Auth Service
cd Komorebi-api/src/Services/AuthService
dotnet run

# Manga Service
cd Komorebi-api/src/Services/MangaService
dotnet run

# Comment Service
cd Komorebi-api/src/Services/CommentService
dotnet run

# User Service
cd Komorebi-api/src/Services/UserService
dotnet run

# Admin Service
cd Komorebi-api/src/Services/AdminService
dotnet run

# Notification Service (can RabbitMQ + Redis cho SignalR)
cd Komorebi-api/src/Services/NotificationService
dotnet run
```

**Thu tu khoi chay khuyen nghi:** API Gateway truoc, sau do cac service con lai theo bat ky thu tu nao.

### Buoc 5: Chay Frontend

```bash
cd komorebi-web
npm install
npm run dev
```

Frontend se chay tai `http://localhost:3000` va goi API qua Gateway tai `http://localhost:5000`.

### Buoc 6: Kiem Tra

Truy cap `http://localhost:3000` tren trinh duyet. Dang ky tai khoan moi hoac dang nhap bang Google OAuth (can cau hinh Google Client ID/Secret trong Auth Service).

---

## Bien Moi Truong

### Backend (.NET Services)

| Bien                                   | Mo ta                                 | Gia tri mac dinh                | Bat buoc |
| -------------------------------------- | ------------------------------------- | ------------------------------- | -------- |
| `ConnectionStrings__DefaultConnection` | PostgreSQL connection string          | _(xem tung service)_            | Co       |
| `Jwt__SecretKey`                       | Secret key cho JWT (>= 32 ky tu)      | _(khong co)_                    | Co       |
| `Jwt__Issuer`                          | JWT Issuer                            | `komorebi`                      | Co       |
| `Jwt__Audience`                        | JWT Audience                          | `komorebi-client`               | Co       |
| `Jwt__ExpiryInMinutes`                 | Thoi gian het han access token (phut) | `60`                            | Khong    |
| `Redis__ConnectionString`              | Redis connection string               | `localhost:6379`                | Khong    |
| `RabbitMQ__Host`                       | RabbitMQ host                         | `localhost`                     | Khong    |
| `RabbitMQ__Username`                   | RabbitMQ username                     | `guest`                         | Khong    |
| `RabbitMQ__Password`                   | RabbitMQ password                     | `guest`                         | Khong    |
| `Storage__Provider`                    | Storage backend (local / minio / s3)  | `local`                         | Khong    |
| `Storage__Local__BasePath`             | Thu muc luu tru anh (khi dung local)  | `D:/komorebi-storage`           | Khong    |
| `Storage__Local__BaseUrl`              | URL truy cap anh (khi dung local)     | `http://localhost:5002/storage` | Khong    |
| `Storage__MinIO__Endpoint`             | MinIO endpoint                        | `localhost:9000`                | Khong    |
| `Storage__MinIO__AccessKey`            | MinIO access key                      | `minioadmin`                    | Khong    |
| `Storage__MinIO__SecretKey`            | MinIO secret key                      | `minioadmin`                    | Khong    |
| `Storage__S3__Region`                  | AWS S3 region                         | `auto`                          | Khong    |
| `Storage__S3__BucketPrefix`            | Tien to ten bucket S3                 | `komorebi-`                     | Khong    |
| `Google__ClientId`                     | Google OAuth Client ID                | _(khong co)_                    | Khong    |
| `Google__ClientSecret`                 | Google OAuth Client Secret            | _(khong co)_                    | Khong    |

### Frontend (Next.js)

| Bien                      | Mo ta                    | Gia tri mac dinh                | Bat buoc |
| ------------------------- | ------------------------ | ------------------------------- | -------- |
| `NEXT_PUBLIC_API_URL`     | URL cua API Gateway      | `http://localhost:5000`         | Co       |
| `NEXT_PUBLIC_SIGNALR_URL` | URL cua SignalR Hub      | `http://localhost:5007/hubs`    | Khong    |
| `NEXT_PUBLIC_STORAGE_URL` | URL truy cap anh/storage | `http://localhost:5002/storage` | Khong    |

---

## API Routes

Tat ca API duoc truy cap qua Gateway tai `http://localhost:5000`. Gateway su dung YARP de route den cac service tuong ung.

### Auth Service (:5001)

| Method | Route                | Mo ta                         | Auth  |
| ------ | -------------------- | ----------------------------- | ----- |
| POST   | `/api/auth/register` | Dang ky tai khoan moi         | Khong |
| POST   | `/api/auth/login`    | Dang nhap bang email/password | Khong |
| POST   | `/api/auth/google`   | Dang nhap bang Google OAuth   | Khong |
| POST   | `/api/auth/refresh`  | Lam moi access token          | Khong |
| POST   | `/api/auth/logout`   | Dang xuat                     | Co    |

### Manga Service (:5002)

| Method | Route                              | Mo ta                         | Auth  |
| ------ | ---------------------------------- | ----------------------------- | ----- |
| GET    | `/api/manga`                       | Danh sach truyen (phan trang) | Khong |
| GET    | `/api/manga/{slug}`                | Chi tiet truyen               | Khong |
| GET    | `/api/manga/{slug}/chapters`       | Danh sach chuong              | Khong |
| GET    | `/api/manga/{slug}/chapters/{num}` | Doc chuong                    | Khong |
| GET    | `/api/manga/search?q=`             | Tim kiem truyen               | Khong |
| POST   | `/api/manga`                       | Tao truyen moi                | Co    |
| PUT    | `/api/manga/{id}`                  | Cap nhat truyen               | Co    |
| DELETE | `/api/manga/{id}`                  | Xoa truyen                    | Co    |
| POST   | `/api/manga/{id}/chapters`         | Them chuong moi               | Co    |

### Comment Service (:5003)

| Method | Route                       | Mo ta                           | Auth  |
| ------ | --------------------------- | ------------------------------- | ----- |
| GET    | `/api/comments?chapterId=`  | Danh sach binh luan theo chuong | Khong |
| POST   | `/api/comments`             | Tao binh luan moi               | Co    |
| PUT    | `/api/comments/{id}`        | Sua binh luan                   | Co    |
| DELETE | `/api/comments/{id}`        | Xoa binh luan                   | Co    |
| POST   | `/api/comments/{id}/like`   | Like binh luan                  | Co    |
| POST   | `/api/comments/{id}/report` | Bao cao binh luan               | Co    |

### User Service (:5004)

| Method | Route                                 | Mo ta                     | Auth |
| ------ | ------------------------------------- | ------------------------- | ---- |
| GET    | `/api/users/profile`                  | Xem profile ca nhan       | Co   |
| PUT    | `/api/users/profile`                  | Cap nhat profile          | Co   |
| GET    | `/api/users/reading-lists`            | Danh sach doc             | Co   |
| POST   | `/api/users/reading-lists`            | Tao danh sach doc moi     | Co   |
| POST   | `/api/users/reading-lists/{id}/manga` | Them truyen vao danh sach | Co   |
| GET    | `/api/users/preferences`              | Xem cai dat doc           | Co   |
| PUT    | `/api/users/preferences`              | Cap nhat cai dat doc      | Co   |
| POST   | `/api/users/follows/{mangaId}`        | Theo doi truyen           | Co   |
| DELETE | `/api/users/follows/{mangaId}`        | Bo theo doi truyen        | Co   |

### Admin Service (:5005)

| Method | Route                                   | Mo ta                          | Auth  |
| ------ | --------------------------------------- | ------------------------------ | ----- |
| GET    | `/api/admin/dashboard`                  | Thong ke tong quan             | Admin |
| GET    | `/api/admin/users`                      | Quan ly nguoi dung             | Admin |
| PUT    | `/api/admin/users/{id}/role`            | Phan quyen nguoi dung          | Admin |
| GET    | `/api/admin/reports`                    | Danh sach bao cao              | Admin |
| PUT    | `/api/admin/reports/{id}`               | Xu ly bao cao                  | Admin |
| GET    | `/api/admin/uploader-applications`      | Danh sach don dang ky uploader | Admin |
| PUT    | `/api/admin/uploader-applications/{id}` | Duyet/tu choi don dang ky      | Admin |
| GET    | `/api/admin/settings`                   | Cau hinh he thong              | Admin |
| PUT    | `/api/admin/settings`                   | Cap nhat cau hinh              | Admin |

### Notification Service (:5007)

| Method | Route                          | Mo ta                   | Auth |
| ------ | ------------------------------ | ----------------------- | ---- |
| GET    | `/api/notifications`           | Danh sach thong bao     | Co   |
| PUT    | `/api/notifications/{id}/read` | Danh dau da doc         | Co   |
| PUT    | `/api/notifications/read-all`  | Danh dau tat ca da doc  | Co   |
| --     | `/hubs/notifications`          | SignalR Hub (WebSocket) | Co   |

---

## Cau Truc Du An

```
truyenfull/
|
+-- Komorebi-api/                          # .NET 8 Backend
|   +-- src/
|   |   +-- Gateway/
|   |   |   +-- ApiGateway/                # YARP Reverse Proxy
|   |   |       +-- Program.cs             # Gateway configuration
|   |   |       +-- appsettings.json       # Route mappings, rate limits
|   |   |
|   |   +-- Services/
|   |   |   +-- AuthService/               # Xac thuc & phan quyen
|   |   |   |   +-- Domain/               # Entities, Value Objects
|   |   |   |   +-- Application/           # CQRS Commands/Queries, MediatR Handlers
|   |   |   |   +-- Infrastructure/        # EF Core, Repositories, External Services
|   |   |   |   +-- API/                   # Controllers, Middleware
|   |   |   |
|   |   |   +-- MangaService/              # Quan ly truyen & chuong
|   |   |   +-- CommentService/            # Binh luan & tuong tac
|   |   |   +-- UserService/               # Profile & tuy chon nguoi dung
|   |   |   +-- AdminService/              # Quan tri he thong
|   |   |   +-- NotificationService/       # Thong bao real-time & email
|   |   |
|   |   +-- Shared/
|   |       +-- Komorebi.Shared/           # DTOs, Constants, Enums dung chung
|   |       +-- Komorebi.Infrastructure/   # Base classes, Common middleware
|   |
|   +-- tests/                             # Unit & Integration tests
|
+-- komorebi-web/                          # Next.js 14 Frontend
|   +-- src/
|   |   +-- app/                           # App Router pages
|   |   +-- components/                    # React components (shadcn/ui)
|   |   +-- hooks/                         # Custom React hooks
|   |   +-- lib/                           # Utilities, API clients
|   |   +-- stores/                        # Zustand stores
|   |   +-- types/                         # TypeScript type definitions
|   |   +-- styles/                        # Global styles, TailwindCSS
|   +-- public/                            # Static assets, PWA manifest
|
+-- infra/                                 # Ha tang & deployment
|   +-- docker-compose.yml                 # Docker Compose cho toan bo he thong
|   +-- .env.example                       # Template bien moi truong
|   +-- terraform/                         # Terraform IaC (AWS/GCP)
|
+-- webtruyendb.sql                        # Database schema (PostgreSQL)
```

### Kien Truc Tung Service (Clean Architecture)

Moi service backend tuan theo Clean Architecture voi 4 layer:

```
Service/
+-- Domain/                # Core business logic, Entities, Value Objects
|                          # Khong phu thuoc vao bat ky layer nao khac
+-- Application/           # Use cases, CQRS Commands & Queries
|                          # MediatR Handlers, Validators (FluentValidation)
+-- Infrastructure/        # EF Core DbContext, Repositories
|                          # External services (Email, Storage, Message Queue)
+-- API/                   # ASP.NET Controllers, Middleware, DI Configuration
```

### Databases

| Service              | Database                  | Mo ta                                         |
| -------------------- | ------------------------- | --------------------------------------------- |
| Auth Service         | `komorebi_auth`           | Users, Roles, Refresh Tokens                  |
| Manga Service        | `komorebi_manga`          | Manga, Chapters, Genres, Views, Images        |
| Comment Service      | `komorebi_comment`        | Comments, Likes, Reports                      |
| User Service         | `komorebi_user`           | Profiles, Reading Lists, Preferences, Follows |
| Admin Service        | `komorebi_admin`          | Settings, Moderation Logs, Uploader Apps      |
| Notification Service | _(doc tu komorebi_manga)_ | Khong co database rieng                       |

---

## Dong Gop

Chung toi hoan nghenh moi dong gop cho du an Komorebi!

### Quy Trinh Dong Gop

1. **Fork** repository nay
2. Tao branch moi tu `main`:
   ```bash
   git checkout -b feature/ten-tinh-nang
   ```
3. Thuc hien thay doi va commit:
   ```bash
   git commit -m "feat: mo ta ngan gon thay doi"
   ```
4. Push len branch cua ban:
   ```bash
   git push origin feature/ten-tinh-nang
   ```
5. Tao **Pull Request** tren GitHub

### Quy Tac Commit Message

Su dung [Conventional Commits](https://www.conventionalcommits.org/):

- `feat:` -- Tinh nang moi
- `fix:` -- Sua loi
- `docs:` -- Cap nhat tai lieu
- `refactor:` -- Tai cau truc code
- `test:` -- Them/sua test
- `chore:` -- Cong viec bao tri

### Coding Standards

- **Backend:** Tuan theo [C# Coding Conventions](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)
- **Frontend:** ESLint + Prettier (da cau hinh san trong du an)
- Dat ten bien, ham bang tieng Anh
- Viet comment cho logic phuc tap
- Viet unit test cho cac business logic quan trong

---

## Giay Phep

Du an nay duoc phat hanh theo giay phep [MIT License](LICENSE).

```
MIT License

Copyright (c) 2024 Komorebi

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
