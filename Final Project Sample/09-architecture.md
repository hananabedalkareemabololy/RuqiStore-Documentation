# 09. Architectural Design

## 9.1 Architecture Pattern: Three-Tier

Ruqi Store uses a **three-tier (layered) architecture**, separating the system into Presentation, Business Logic, and Data tiers. This pattern guarantees clean separation of concerns, allows independent scaling of individual tiers, and matches the requirements of a high-performance e-commerce catalog and transaction system.

```mermaid
graph TB

    subgraph Presentation["Presentation Tier (Client)"]
        Browser["Web/Mobile Browser"]
        React["React.js SPA / Client UI"]
    end

    subgraph Logic["Business Logic Tier (Server)"]
        API["REST API - Node.js / Express"]
        Auth["Authentication Middleware - JWT"]
        BL["Business Services & Workflows"]
        Val["Validation & Policy Layer"]
    end

    subgraph Data["Data Tier"]
        DB[("SQL Server Database")]
        FileStore[("File Storage - AWS S3")]
        Cache[("Redis Cache")]
    end

    subgraph External["External Services"]
        SMTP["Email Service - SendGrid"]
        Pay["Payment Gateway - Stripe"]
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

    style Presentation fill:#e3f2fd,stroke:#1e88e5,stroke-width:2px
    style Logic fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    style Data fill:#e8f5e9,stroke:#43a047,stroke-width:2px
    style External fill:#fce4ec,stroke:#d81b60,stroke-width:2px
```
| Layer | Technology | Justification |
|---|---|---|
| Frontend | React.js | Component-based structure, fast virtual DOM rendering, and a robust ecosystem for dynamic shopping carts. |
| Backend | Node.js + Express | Event-driven asynchronous I/O that efficiently handles concurrent customer browsing and checkout operations. |
| Database | SQL Server (MSSQL) | Strong relational model, ACID transaction support for order payments, and reliable referential integrity. |
| Cache | Redis | Temporary session storage and caching frequently accessed catalog products and active cart states. |
| File Storage | AWS S3 | Highly scalable and secure storage for product images, marketing assets, and invoice PDFs. |
| Authentication | JWT + bcrypt | Stateless secure authorization and industry-standard salted password hashing. |
| API Style | RESTful API | Clean resource-oriented HTTP routing with easy integration and strong testing support. |
| External Integrations | SendGrid & Stripe | Reliable transactional email notifications and secure PCI-compliant payment processing. |

## 9.3 Component Diagram

This diagram displays the structural components of Ruqi Store and how requests propagate from public UI components down to database repositories.

```mermaid
graph LR

    subgraph Frontend["Frontend Client (React)"]
        Login["Login / Profile"]
        Home["Catalog Home"]
        CartUI["Shopping Cart View"]
        CheckoutUI["Checkout Page"]
        ShowroomUI["Showroom Scheduler"]
    end

    subgraph Backend["Backend API Controllers"]
        AuthC["Auth Controller"]
        ProdC["Product Controller"]
        CartC["Cart Controller"]
        OrdC["Order Controller"]
        ShowC["Showroom Controller"]
    end

    subgraph Services["Business Logic Services"]
        AuthS["Identity Service"]
        StockS["Inventory Service"]
        CartS["Cart Service"]
        OrderS["Order Processing Service"]
        BookS["Appointment Service"]
        MailS["Notification Service"]
        PayS["Payment Integration Service"]
    end

    subgraph Data["Data Access Repositories"]
        UserR["User Repository"]
        ProdR["Product Repository"]
        CartR["Cart Repository"]
        OrderR["Order Repository"]
        BookR["Appointment Repository"]
    end

    Frontend -->|REST API Calls| Backend
    Backend --> Services
    Services --> Data

    MailS -->|SMTP| SendGrid["SendGrid API"]
    PayS -->|HTTPS| Stripe["Stripe SDK"]

    ProdC -->|S3 Upload| S3[("AWS S3 Bucket")]
    Data --> DB[("SQL Server")]

    style Frontend fill:#e3f2fd,stroke:#1e88e5
    style Backend fill:#fff3e0,stroke:#f57c00
    style Services fill:#e8f5e9,stroke:#43a047
    style Data fill:#f5f5f5,stroke:#9e9e9e
