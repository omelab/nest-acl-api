<h1 align="center">
  <img src=".github/assets/images/img1.png" height="200" alt="Nest ACL Logo">
</h1>

<p align="center">
  <img src="https://img.shields.io/github/license/your-username/nest-acl?color=00b8d3&style=flat" alt="License" />
  <img src="https://img.shields.io/github/languages/top/your-username/nest-acl?style=flat" alt="Top Language" />
  <img src="https://img.shields.io/github/repo-size/your-username/nest-acl?style=flat" alt="Repo Size" />
  <img src="https://img.shields.io/github/last-commit/your-username/nest-acl?style=flat" alt="Last Commit" />
  <img src="https://wakatime.com/badge/user/your-wakatime-id/project/your-project-id.svg?style=flat" alt="Wakatime" />
</p>

<br>

<p align="center">
  <a href="README.md">English</a>
</p>

<p align="center">
  <a href="#bookmark-about">About</a>&nbsp;&nbsp;|&nbsp;&nbsp;
  <a href="#computer-technologies">Technologies</a>&nbsp;&nbsp;|&nbsp;&nbsp;
  <a href="#wrench-tools">Tools</a>&nbsp;&nbsp;|&nbsp;&nbsp;
  <a href="#package-installation">Installation</a>&nbsp;&nbsp;|&nbsp;&nbsp;
  <a href="#memo-license">License</a>
</p>

<br>

## :bookmark: About

**Nest ACL** is a **modular Access Control List API** built with **NestJS**, **PostgreSQL**, and **Prisma ORM**.  
It provides a robust foundation for **authentication**, **authorization**, and **role-based access control (RBAC)**.  
Following **clean architecture principles**, it ensures clear separation of concerns and serves as a reusable base for multiple projects.

---

### 🏗️ Architecture Overview

```mermaid
graph TB
    subgraph "Client Layer"
        WEB[Web Apps]
        MOB[Mobile Apps]
        API[External APIs]
    end

    subgraph "API Gateway - NestJS"
        ROUTES["/api/v1/*"]
        MW[Guards & Middleware]
    end

    subgraph "Modules"
        AUTH[Auth Module<br/>JWT, Guards]
        USER[User Module<br/>CRUD, Profile]
        ROLE[Role Module<br/>RBAC]
        PERM[Permission Module<br/>Context-aware]
        FILE[File Module<br/>Uploads]
        AUDIT[Audit Module<br/>Logs]
        HEALTH[Health Module<br/>Monitoring]
    end

    subgraph "Core Services"
        JWT[JWT Service]
        HASH[Bcrypt Service]
        VALIDATOR[Class-Validator]
        STORAGE[S3/Local Storage]
    end

    subgraph "Data Layer"
        DB[(PostgreSQL<br/>Prisma ORM)]
        REDIS[(Redis Cache)]
    end

    WEB --> ROUTES
    MOB --> ROUTES
    API --> ROUTES

    ROUTES --> MW
    MW --> AUTH
    MW --> USER
    MW --> ROLE
    MW --> PERM
    MW --> FILE
    MW --> AUDIT
    MW --> HEALTH

    AUTH --> JWT
    AUTH --> HASH
    USER --> VALIDATOR
    FILE --> STORAGE
    PERM --> REDIS

    USER --> DB
    ROLE --> DB
    PERM --> DB
    AUTH --> DB
    AUTH --> REDIS
    AUDIT --> DB
```

### 🔐 Authentication Flow

```mermaid
sequenceDiagram
    participant Client
    participant API
    participant Auth as AuthService
    participant DB as PostgreSQL
    participant Redis as Redis Cache

    Client->>API: POST /api/v1/auth/sign-in
    API->>Auth: Validate credentials
    Auth->>DB: Fetch user
    DB-->>Auth: User data
    Auth->>Auth: Verify password
    Auth->>Auth: Generate JWT
    Auth->>Redis: Store session
    Auth-->>Client: Access + Refresh Tokens

    Note over Client,API: Subsequent requests use Bearer token

    Client->>API: GET /api/v1/users
    API->>Auth: Verify JWT
    Auth->>Redis: Validate session
    Redis-->>Auth: Valid
    API-->>Client: Protected resource
```


### 📁 Folder Structure

```bash
src/
├── auth/           # Auth module (JWT, Guards)
├── users/          # User module (CRUD, profile)
├── roles/          # Role module (RBAC logic)
├── permissions/    # Permission module (context-aware)
├── audit/          # Audit logs
├── common/         # Shared utilities, interceptors, pipes
└── main.ts         # Bootstrap file

```



### 🌟 Key Features

    - 🔐 JWT Authentication with refresh tokens
    - 👥 Role-Based Access Control (RBAC)
    - 🎯 Context-aware permissions (own, any, team, department)
    - 🗄️ PostgreSQL database managed with Prisma ORM
    - 🧩 Modular architecture for scalability
    - 🚀 RESTful API following clean standards
    - 📤 File Upload Support (local/S3)
    - 🏥 Health Checks and monitoring
    - 📝 Validation with DTOs and class-validator
    - ⚡ Redis caching for permissions and sessions
    - 🕵 Audit Logs to track access and permission checks

### :computer: Technologies

    - NestJS
        – Node.js framework 
    - TypeScript 
    - PostgreSQL
        – Relational database 
    - Prisma ORM
        – Type-safe database access 
    - Redis
        – Session & permission caching 
    - Docker
        – Containerized environment


### :wrench: Tools

    - VS Code or WebStorm 
    - Insomnia – API testing 
    - PgAdmin – Database management