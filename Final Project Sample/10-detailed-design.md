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
## 9.2 Technology Stack

| Layer | Technology | Justification |
| :--- | :--- | :--- |
| Frontend | React.js | Component-based structure, fast virtual DOM rendering, and a robust ecosystem for dynamic shopping carts. |
| Backend | Node.js + Express | Event-driven, asynchronous I/O that handles concurrent customer browsing and checkout operations efficiently. |
| Database | SQL Server (MSSQL) | Strict relational model, robust transaction handling (ACID) for order payments, and deep referential integrity. |
| Cache | Redis | Temporary session storage, caching frequently accessed catalog products, and maintaining current cart states. |
| File Storage | AWS S3 | Highly scalable and secure storage for product images, marketing assets, and invoice PDFs. |
| Authentication | JWT + bcrypt | Stateless secure authorization for active sessions and industry-standard salted password hashing. |
| API Style | RESTful | Clean resource-oriented HTTP routing, easy integration, and well-supported testing suites. |
| External Integrations | SendGrid & Stripe | Reliable transactional email notifications and secure, PCI-compliant payment card processing. |
