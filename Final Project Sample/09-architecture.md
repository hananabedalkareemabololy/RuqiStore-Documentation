09. Architectural Design

9.1 Architecture Pattern: Three-Tier Architecture

Ruqi Store follows a Three-Tier Layered Architecture that separates the system into three main layers:

1. Presentation Layer
2. Business Logic Layer
3. Data Access Layer

This architecture improves maintainability, scalability, testing, and separation of responsibilities.

graph TB

    subgraph Presentation["Presentation Layer"]
        UI["ASP.NET Core MVC Views"]
        Client["Web Browser / Mobile Browser"]
    end

    subgraph Business["Business Logic Layer"]
        Controllers["MVC Controllers"]
        Services["Business Services"]
        Validation["Validation & Business Rules"]
    end

    subgraph Data["Data Access Layer"]
        Repository["Repository Layer"]
        EF["Entity Framework Core"]
        Database[("SQL Server Database")]
    end

    subgraph External["External Services"]
        Payment["Payment Gateway"]
        Email["Email Notification Service"]
        Storage["File Storage"]
    end


    Client --> UI
    UI --> Controllers
    Controllers --> Services
    Services --> Validation
    Services --> Repository
    Repository --> EF
    EF --> Database

    Services --> Payment
    Services --> Email
    Services --> Storage

---

9.2 Layer Responsibilities

Layer| Technology| Responsibility
Presentation Layer| ASP.NET Core MVC + Razor Views| Handles user interaction, pages, forms, and displaying data.
Business Logic Layer| C# Services| Contains business rules, checkout workflow, inventory validation, and booking logic.
Data Access Layer| Entity Framework Core + Repository Pattern| Handles database communication and CRUD operations.
Database| SQL Server| Stores users, products, carts, orders, reviews, and transactions.
Authentication| ASP.NET Identity| Provides secure login, registration, and role management.
File Storage| Local Storage / Cloud Storage| Stores product images and uploaded assets.

---

9.3 Component Diagram

The component diagram represents the internal software components and their communication flow.

graph LR

subgraph Frontend["Presentation Layer"]

Login["Login/Register Views"]

Catalog["Product Catalog Views"]

Cart["Shopping Cart Views"]

Checkout["Checkout Views"]

Showroom["Showroom Booking Views"]

end


subgraph Controllers["MVC Controllers"]

AuthController["Account Controller"]

ProductController["Product Controller"]

CartController["Cart Controller"]

OrderController["Order Controller"]

BookingController["Showroom Controller"]

end


subgraph Services["Business Services"]

UserService["User Service"]

ProductService["Product Service"]

CartService["Cart Service"]

OrderService["Order Service"]

InventoryService["Inventory Service"]

BookingService["Appointment Service"]

end


subgraph Repository["Repository Layer"]

UserRepo["User Repository"]

ProductRepo["Product Repository"]

OrderRepo["Order Repository"]

CartRepo["Cart Repository"]

BookingRepo["Booking Repository"]

end


Database[("SQL Server")]


Frontend --> Controllers

Controllers --> Services

Services --> Repository

Repository --> Database

---

9.4 Architectural Decisions

Decision| Selected Approach| Alternative| Reason
Architecture Pattern| Three-Tier Architecture| Microservices| Suitable for a medium-size e-commerce system and easier deployment.
Backend Framework| ASP.NET Core MVC| Node.js Express| Matches project requirements and provides strong integration with SQL Server and Entity Framework.
Database| SQL Server| MongoDB| Orders and payments require ACID transactions and relational consistency.
Data Access| Repository Pattern + EF Core| Direct SQL Queries| Separates business logic from database implementation.
Authentication| ASP.NET Identity + Roles| Custom Authentication| Provides secure and tested identity management.
API Communication| RESTful Services| SOAP| Lightweight and easier integration with frontend/mobile applications.

---

9.5 Deployment View

The deployment diagram describes the physical deployment environment of the Ruqi Store system.

The system is deployed using a web server environment that hosts the ASP.NET Core application and connects to the SQL Server database.

graph TB

subgraph Client["Client Devices"]

Desktop["Desktop Browser"]

Mobile["Mobile Browser"]

Tablet["Tablet Browser"]

end


subgraph Server["Application Server"]

WebServer["ASP.NET Core Web Application"]

MVC["MVC Controllers + Views"]

Services["Business Services"]

Repositories["Repository Layer"]

end


subgraph Database["Database Server"]

SQL[("SQL Server Database")]

end


subgraph Storage["File Storage"]

Images[("Product Images Storage")]

end


subgraph External["External Services"]

Payment["Payment Gateway"]

Email["Email Service"]

end


Desktop --> WebServer

Mobile --> WebServer

Tablet --> WebServer


WebServer --> MVC

MVC --> Services

Services --> Repositories

Repositories --> SQL


Services --> Images

Services --> Payment

Services --> Email

---

9.6 Deployment Components Description

Component| Responsibility
Client Devices| Allow customers and administrators to access the system through browsers.
ASP.NET Core Application Server| Hosts the web application, controllers, views, and business logic.
Business Services| Execute application rules such as checkout, inventory validation, and booking management.
Repository Layer| Provides controlled access to database operations.
SQL Server Database| Stores persistent application data including users, products, carts, and orders.
File Storage| Stores product images and uploaded media files.
Payment Gateway| Handles secure payment processing.
Email Service| Sends order confirmations and booking notifications.

---

9.7 Scalability Considerations

Ruqi Store is designed to support future growth and increased customer traffic.

Horizontal Scaling

The application layer can be expanded by adding additional application servers.

Example:

Application Server 1
Application Server 2
Application Server 3

A load balancer can distribute incoming requests between servers.

---

Database Optimization

The system improves database performance through:

- Proper indexing on frequently searched fields.
- Foreign key optimization.
- Query optimization using Entity Framework Core.
- Separation of read and write operations when required.

---

Caching Strategy

Frequently accessed information can be cached:

Examples:

- Product catalog data.
- Category lists.
- Popular furniture items.

A caching mechanism such as Redis can be integrated in future versions.

---

9.8 Security Architecture

The system applies multiple security layers:

Security Area| Implementation
Authentication| ASP.NET Identity with secure password hashing.
Authorization| Role-Based Access Control (Customer / Administrator).
Data Protection| HTTPS encrypted communication.
Database Security| Parameterized queries through Entity Framework Core.
Password Security| BCrypt/PBKDF2 hashing algorithms.
Audit Tracking| Audit logs for sensitive administrative actions.

---

9.9 Architectural Quality Attributes

Attribute| Implementation
Maintainability| Separation into Controllers, Services, and Repository layers.
Scalability| Independent scaling of application components.
Security| Identity management, authorization, and encrypted communication.
Reliability| Database transactions prevent inconsistent orders.
Testability| Services can be tested independently using dependency injection.
Performance| Database indexing and optional caching improve response time.

---

9.10 Summary

The selected three-tier architecture provides a balanced solution for Ruqi Store by combining:

- Clean separation of responsibilities.
- Strong database consistency.
- Secure authentication and authorization.
- Easy future expansion.
- Maintainable software structure.

This architecture supports the current e-commerce requirements while allowing future growth without major redesign.

---
"← Previous: Database Design" (./08-database-design.md) | "Back to Index" (./00-index.md) | "Next: Detailed Design →" (./10-detailed-design.md)
