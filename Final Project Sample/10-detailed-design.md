# 10. Detailed Class Design & API Contracts

## 10.1 Design Patterns Applied

The Ruqi Store architecture applies established software design patterns to solve common architectural challenges and maintain a clean separation of concerns.

| Pattern | Category | Where Applied | Problem Solved |
|---|---|---|---|
| **Repository** | Structural | Data Access Layer | Abstracts database operations behind a consistent interface. Isolates business services from raw database queries, allowing caching layers such as Redis to be injected transparently. |
| **MVC** | Architectural | System Presentation & Route Control | Separates UI, business logic, and request orchestration. Controllers process requests, invoke backend services, and return normalized responses or views. |
| **Observer** | Behavioral | Order Notification & Audit Logging | Triggers asynchronous background processes such as sending order confirmation emails, reducing stock, and writing audit logs when an order status changes without tight coupling. |
| **Factory** | Creational | Showroom Appointment Scheduler | A `BookingFactory` creates the correct appointment and notification structure depending on the selected showroom category. |
| **Strategy** | Behavioral | Discount & Promotion System | Different discount calculation strategies can be executed dynamically based on active shopping cart contents. |

---

## 10.2 SOLID Principles in Practice

### Single Responsibility Principle (SRP)

Each service has responsibility for one cohesive part of the business flow.

- `CartService` manages shopping cart operations, item additions, quantity updates, and cart state.
- `InventoryService` manages warehouse stock levels and product availability.
- `PaymentService` processes secure checkout payments.
- `InvoiceService` generates receipt documentation and invoice files.

Each class has only one reason to change.

---

### Open/Closed Principle (OCP)

The promotions and pricing engine supports adding new discount methods without modifying existing checkout logic.

Adding a new discount type only requires creating a new strategy class that implements the shared interface.

```csharp
public interface IDiscountStrategy
{
    decimal ApplyDiscount(
        IEnumerable<CartItemDto> cartItems,
        decimal originalTotal
    );
}

public class PercentageDiscountStrategy : IDiscountStrategy
{
    public decimal ApplyDiscount(
        IEnumerable<CartItemDto> cartItems,
        decimal originalTotal)
    {
        return originalTotal * 0.90m;
    }
}

public class FixedAmountDiscountStrategy : IDiscountStrategy
{
    public decimal ApplyDiscount(
        IEnumerable<CartItemDto> cartItems,
        decimal originalTotal)
    {
        return originalTotal - 50m;
    }
}
```

```csharp
// New strategy can be added without modifying existing checkout logic.

public class FlashSaleDiscountStrategy : IDiscountStrategy
{
    public decimal ApplyDiscount(
        IEnumerable<CartItemDto> cartItems,
        decimal originalTotal)
    {
        return originalTotal * 0.80m;
    }
}
```
