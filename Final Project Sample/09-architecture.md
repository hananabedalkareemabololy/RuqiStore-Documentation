# 09. Architectural Design

## 9.1 Architecture Pattern: Three-Tier

Ruqi Store uses a **three-tier (layered) architecture**, separating the system into Presentation, Business Logic, and Data tiers. This pattern guarantees clean separation of concerns, allows independent scaling of individual tiers, and matches the requirements of a high-performance e-commerce catalog and transaction system.

```mermaid
graph TB
    subgraph Presentation["Presentation Tier (Client)"]
        Browser[Web/Mobile Browser]
        React[React.js SPA / Client UI]
    end

    subgraph Logic["Business Logic Tier (Server)"]
        API[REST API - Node.js / Express]
        Auth[Authentication Middleware - JWT]
        BL[Business Services & Workflows]
        Val[Validation & Policy Layer]
    end

    subgraph Data["Data Tier"]
        DB[(SQL Server Database)]
        FileStore[(File Storage - AWS S3)]
        Cache[(Redis Cache)]
    end

    subgraph External["External Services"]
        SMTP[Email Service - SendGrid]
        Pay[Payment Gateway - Stripe]
    end

    Browser --> React
    React -->|HTTPS / REST API| API
    API --> Auth
    Auth --> BL
    BL --> Val
    Val --> DB
    BL --> FileStore
    BL --> Cache
    BL --> SMTP
    BL --> Pay
## 9.2 Technology Stack

| Layer | Technology | Justification |
|---|---|---|
| Frontend | React.js | Component-based structure, fast virtual DOM rendering, and a robust ecosystem for dynamic shopping carts. |
| Backend | Node.js + Express | Event-driven, asynchronous I/O that handles concurrent customer browsing and checkout operations efficiently. |
| Database | SQL Server (MSSQL) | Strict relational model, robust transaction handling (ACID) for order payments, and deep referential integrity. |
| Cache | Redis | Temporary session storage, caching frequently accessed catalog products and current cart states. |
| File Storage | AWS S3 | Highly scalable, secure storage for product images, marketing assets, and invoice PDFs. |
| Authentication | JWT + bcrypt | Stateless secure authorization for active sessions and industry-standard salted password hashing. |
| API Style | RESTful | Clean resource-oriented HTTP routing, easy integration, and well-supported testing suites. |
| External Integrations | SendGrid & Stripe | Reliable transactional email notifications and secure, PCI-compliant payment card processing. |
## 9.3 Component Diagram

This diagram displays the structural components of Ruqi Store and how requests propagate from public UI components down to database repositories.

```mermaid
graph LR

    subgraph Frontend["Frontend Client (React)"]
        Login[Login / Profile]
        Home[Catalog Home]
        CartUI[Shopping Cart View]
        CheckoutUI[Checkout Page]
        ShowroomUI[Showroom Scheduler]
    end

    subgraph Backend["Backend API Controllers"]
        AuthC[Auth Controller]
        ProdC[Product Controller]
        CartC[Cart Controller]
        OrdC[Order Controller]
        ShowC[Showroom Controller]
    end

    subgraph Services["Business Logic Services"]
        AuthS[Identity Service]
        StockS[Inventory Service]
        CartS[Cart Service]
        OrderS[Order Processing Service]
        BookS[Appointment Service]
        MailS[Notification Service]
        PayS[Payment Integration Service]
    end

    subgraph Data["Data Access Repositories"]
        UserR[User Repository]
        ProdR[Product Repository]
        CartR[Cart Repository]
        OrderR[Order Repository]
        BookR[Appointment Repository]
    end

    Frontend -->|REST API Calls| Backend
    Backend --> Services
    Services --> Data

    MailS -->|SMTP| SendGrid[SendGrid API]
    PayS -->|HTTPS| Stripe[Stripe SDK]
    ProdC -->|S3 Upload| S3[(S3 Bucket)]
    Data --> DB[(SQL Server Database)]
## 9.4 Architectural Decisions

| Decision Topic | Selected Approach | Alternatives Considered | Rationale |
|---|---|---|---|
| Architecture Pattern | Three-Tier Monolith | Microservices | Monolith offers rapid deployment and low operational overhead for a mid-sized e-commerce store, avoiding network latency and complex transaction routing. |
| Frontend Rendering | Single Page App (SPA) | Server-Side Rendering (SSR) | SPA provides seamless state transitions (ideal for shopping carts and showroom calendars) and isolates UI processing from the server. |
| Database System | Relational (SQL Server) | NoSQL (MongoDB) | E-commerce checkout requires absolute transaction safety (ACID) to update stock levels and record payments securely without collisions. |
| Authentication Style | Stateless JWT Tokens | Session Cookies | JWT tokens support horizontal backend scaling without sticky-session overhead and integrate easily with future native mobile apps. |
| File Hosting | External Storage (AWS S3) | Local Disk Storage | Offloads heavy asset delivery (high-resolution product and showroom media) from the app server, reducing disk load and improving server response times. |
## 9.5 Deployment View

This physical blueprint displays the production network topography of the Ruqi Store system.

```mermaid
graph TB

    subgraph Client["Client Tier"]
        PC[Desktop Browser]
        Tablet[Tablet Browser]
        Mobile[Mobile Device]
    end

    subgraph CDN["Content Delivery Network"]
        CloudFront[AWS CloudFront CDN]
    end

    subgraph WebServer["Application Server Layer"]
        LB[AWS Application Load Balancer]
        App1[Node.js Instance 1]
        App2[Node.js Instance 2]
    end

    subgraph DataStores["Data Storage Layer"]
        DB_Primary[(SQL Server Primary)]
        DB_Replica[(SQL Server Replica - Read Only)]
        Redis[(Redis Cluster)]
        S3[(AWS S3 Bucket)]
    end

    Client -->|HTTPS Request| CloudFront
    CloudFront -->|Static Media & Assets| S3
    CloudFront -->|Dynamic API Requests| LB

    LB --> App1
    LB --> App2

    App1 --> DB_Primary
    App2 --> DB_Primary

    DB_Primary -.->|Asynchronous Replication| DB_Replica

    App1 --> Redis
    App2 --> Redis

    App1 --> S3
    App2 --> S3
