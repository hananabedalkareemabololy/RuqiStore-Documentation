# 10. Detailed Class Design & API Contracts

## 10.1 Design Patterns Applied

The Ruqi Store architecture applies established software design patterns to solve common architectural challenges and maintain a clean separation of concerns.

| Pattern | Category | Where Applied | Problem Solved |
|---------|----------|---------------|----------------|
| **Repository** | Structural | Data Access Layer | Abstracts database operations behind a consistent interface. Isolates business services from raw database queries, allowing caching layers such as Redis to be injected transparently. |
| **MVC** | Architectural | System Presentation & Route Control | Separates UI, business logic, and request orchestration. Controllers process requests, invoke appropriate backend services, and return normalized responses or views. |
| **Observer** | Behavioral | Order Notification & Audit Logging | Triggers asynchronous background processes such as sending order confirmation emails, reducing stock, and writing audit logs when an order transitions to "Paid" without tight coupling. |
| **Factory** | Creational | Showroom Appointment Scheduler | A `BookingFactory` creates the correct appointment and notification layout depending on the selected showroom category. |
| **Strategy** | Behavioral | Discount & Promotion System | Different calculation strategies such as coupon discount, seasonal discount, and bulk order discount can be executed dynamically based on the active shopping cart contents. |

---

## 10.2 SOLID Principles in Practice

### Single Responsibility Principle (SRP)

Each service has one clear responsibility within the business workflow.

- `CartService` → Manages user shopping carts, item additions, quantity updates, and cart state.
- `InventoryService` → Manages warehouse quantities and product availability.
- `PaymentService` → Processes secure checkout payments.
- `InvoiceService` → Generates receipt documentation and invoice PDFs.

Each class has only one reason to change, which improves maintainability and testing.

---

### Open/Closed Principle (OCP)

The promotions and pricing engine can introduce new discount methods without modifying existing core logic.

Adding a new discount type only requires creating a new strategy class implementing the shared interface.

```csharp
public interface IDiscountStrategy
{
    decimal ApplyDiscount(IEnumerable<CartItemDto> cartItems, decimal originalTotal);
}

public class PercentageDiscountStrategy : IDiscountStrategy
{
    public decimal ApplyDiscount(IEnumerable<CartItemDto> cartItems, decimal originalTotal)
    {
        return originalTotal * 0.10m;
    }
}

public class FixedAmountDiscountStrategy : IDiscountStrategy
{
    public decimal ApplyDiscount(IEnumerable<CartItemDto> cartItems, decimal originalTotal)
    {
        return 50m;
    }
}

// New strategies can be added without modifying existing logic
public class FlashSaleDiscountStrategy : IDiscountStrategy
{
    public decimal ApplyDiscount(IEnumerable<CartItemDto> cartItems, decimal originalTotal)
    {
        return originalTotal * 0.20m;
    }
}

### Dependency Inversion Principle (DIP)

High-level application controllers depend on abstractions rather than concrete service implementations.

This separates dependencies and allows easy unit testing using mock services.

```csharp
public class OrderController : ControllerBase
{
    private readonly IOrderService _orderService;

    public OrderController(IOrderService orderService)
    {
        _orderService = orderService;
    }
}

// Production runtime:
// new OrderController(new OrderService(databaseContext))

// Testing runtime:
// new OrderController(new MockOrderService())

# 10.4 Data Transfer Objects (DTOs)

DTOs are lightweight classes used to transfer data between application layers without exposing Entity Framework models directly.

---

## A. CheckoutDto

**Purpose:**  
Carries checkout information from the Presentation Layer to the Business Logic Layer.

```csharp
public class CheckoutDto
{
    public string ShippingAddress { get; set; }
    public string PaymentMethod { get; set; }
    public string ContactPhoneNumber { get; set; }
}
