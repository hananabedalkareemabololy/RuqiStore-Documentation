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
