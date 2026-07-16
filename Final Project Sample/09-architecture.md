09. Architectural Design

9.1 Architecture Pattern: Three-Tier Architecture

Ruqi Store follows a Three-Tier Layered Architecture that separates the system into three independent layers: Presentation Layer, Business Logic Layer, and Data Access Layer.

This architecture improves maintainability, scalability, testing, and separation of responsibilities. It also supports the requirements of an e-commerce platform by isolating user interaction, business rules, and database operations.

graph TB

    subgraph Presentation["Presentation Tier"]
        Browser["Web Browser"]
        MVC["ASP.NET Core MVC Views / Controllers"]
        UI["HTML + CSS + Bootstrap 5 + JavaScript"]
    end

    subgraph Business["Business Logic Tier"]
        Services["Application Services"]
        Validation["Business Validation Layer"]
        Identity["ASP.NET Core Identity"]
    end

    subgraph Data["Data Access Tier"]
        Repository["Repository Layer"]
        EF["Entity Framework Core"]
        DB[("SQL Server Database")]
    end

    subgraph External["External Services"]
        Email["Email Notification Service"]
        Payment["Payment Gateway"]
        Storage["File Storage"]
    end


    Browser --> MVC
    MVC --> UI

    MVC --> Services
    Services --> Validation
    Services --> Identity

    Services --> Repository
    Repository --> EF
    EF --> DB

    Services --> Email
    Services --> Payment
    Services --> Storage

---

9.2 Technology Stack

Layer| Technology| Justification
Frontend| ASP.NET Core MVC Razor Views + Bootstrap 5 + JavaScript| Provides structured UI development, responsive layouts, and strong integration with backend services.
Backend| ASP.NET Core MVC| Provides secure routing, controllers, dependency injection, and scalable server-side processing.
ORM| Entity Framework Core| Simplifies database communication using object-oriented models and LINQ queries.
Database| SQL Server| Provides ACID transactions, relational integrity, and reliable order/inventory management.
Authentication| ASP.NET Core Identity| Provides secure user management, password hashing, roles, and authorization.
API Communication| RESTful Services| Enables structured communication between system components and external services.
File Storage| Server Storage / Cloud Storage (Optional)| Stores product images, invoices, and marketing resources.
External Services| Email Provider and Payment Gateway| Handles notifications and secure payment processing.

---

9.3 Component Diagram

This diagram represents the main software components and the communication flow inside Ruqi Store.

graph LR

    subgraph UI["Presentation Layer"]
        Login["Login/Register Pages"]
        Catalog["Product Catalog"]
        CartUI["Shopping Cart"]
        Checkout["Checkout"]
        Booking["Showroom Booking"]
    end


    subgraph Controllers["MVC Controllers"]
        AccountC["Account Controller"]
        ProductC["Product Controller"]
        CartC["Cart Controller"]
        OrderC["Order Controller"]
        BookingC["Booking Controller"]
    end


    subgraph Services["Business Services"]
        UserS["User Service"]
        ProductS["Product Service"]
        CartS["Cart Service"]
        OrderS["Order Service"]
        InventoryS["Inventory Service"]
        BookingS["Appointment Service"]
    end


    subgraph Data["Data Access Layer"]
        UserRepo["User Repository"]
        ProductRepo["Product Repository"]
        CartRepo["Cart Repository"]
        OrderRepo["Order Repository"]
        BookingRepo["Booking Repository"]
    end


    DB[("SQL Server")]


    UI --> Controllers
    Controllers --> Services
    Services --> Data
    Data --> DB

---

9.4 Architectural Decisions

Decision Topic| Selected Approach| Alternatives Considered| Rationale
Architecture Pattern| Three-Tier Architecture| Microservices| Suitable for a medium-sized e-commerce system and reduces deployment complexity.
Backend Framework| ASP.NET Core MVC| Node.js Express| Provides strong enterprise support, security features, and integration with SQL Server.
Database System| SQL Server| MongoDB| Relational databases provide stronger consistency for orders, payments, and inventory.
Data Access| Entity Framework Core| Raw SQL Queries| Reduces database complexity and improves maintainability through ORM mapping.
Authentication| ASP.NET Core Identity| Custom Authentication| Provides built-in secure authentication, password hashing, and role management.
Application Structure| MVC + Service Layer| Monolithic Controllers| Keeps controllers lightweight and separates business rules from presentation logic.

---

9.5 Deployment View

The production deployment architecture of Ruqi Store separates the client environment, application server, and database server.

graph TB

    subgraph Client["Client Devices"]
        Desktop["Desktop Browser"]
        Mobile["Mobile Browser"]
        Tablet["Tablet Browser"]
    end


    subgraph Server["Application Server"]
        IIS["IIS Web Server"]
        App["ASP.NET Core MVC Application"]
    end


    subgraph Database["Database Layer"]
        SQL[("SQL Server Database")]
    end


    subgraph Storage["Storage Services"]
        Files["Product Images / Documents Storage"]
    end


    Desktop --> IIS
    Mobile --> IIS
    Tablet --> IIS

    IIS --> App

    App --> SQL
    App --> Files

---

9.6 Scalability Considerations

The architecture supports future growth by maintaining a separated and modular structure.

The system can be scaled by:

- Increasing server resources when traffic grows.
- Adding caching mechanisms for frequently accessed products.
- Optimizing SQL Server indexes for catalog and order queries.
- Separating heavy background operations such as email notifications into background services.
- Expanding storage capacity for product images and documents.

The layered design allows future migration to distributed services if business requirements increase.

---

9.7 Security Considerations

The architecture applies multiple security practices:

Security Aspect| Implementation
Authentication| ASP.NET Core Identity with secure password hashing.
Authorization| Role-Based Access Control (Customer, Administrator).
Data Protection| HTTPS communication and secure database access.
Input Validation| Server-side validation before processing requests.
Audit Tracking| Recording sensitive administrative operations through Audit Logs.
Database Security| Entity Framework Core parameterized queries to prevent SQL Injection.

---

"← Previous: Database Design" (./08-database-design.md) | "Back to Index" (./00-index.md) | "Next: Detailed Design →" (./10-detailed-design.md)
