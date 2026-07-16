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
