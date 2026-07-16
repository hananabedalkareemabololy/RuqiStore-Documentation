# 10. Detailed Class Design & API Contracts

## 10.1 Design Patterns Applied

The Ruqi Store architecture applies established software design patterns to solve common architectural challenges and maintain a clean separation of concerns.

| Pattern | Category | Where Applied | Problem Solved |
|---|---|---|---|
| **Repository** | Structural | Data Access Layer | Abstracts database operations behind a consistent interface. Isolates business services from raw database queries, allowing caching layers such as Redis to be injected transparently. |
| **MVC** | Architectural | System Presentation & Route Control | Separates UI, business logic, and request orchestration. Controllers process requests, invoke backend services, and return normalized responses or views. |
| **Observer** | Behavioral | Order Notification & Audit Logging | Triggers asynchronous background processes such as sending order confirmation emails, reducing stock, and writing audit logs when an order status changes without tight coupling. |
| **Factory** | Creational | Showroom Appointment Scheduler | A BookingFactory creates the correct appointment and notification structure depending on the selected showroom category. |
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

---

### Dependency Inversion Principle (DIP)

High-level application controllers depend on abstract interfaces rather than concrete service implementations.

This improves separation of concerns and allows easy unit testing using mock services.

```csharp
public class OrderController : ControllerBase
{
    private readonly IOrderService _orderService;

    public OrderController(IOrderService orderService)
    {
        _orderService = orderService;
    }
}
Production runtime:

```csharp
new OrderController(
    new OrderService(databaseContext)
);
new OrderController(
    new MockOrderService()
);
# 10.3 Service Contracts & Interfaces

These interfaces define the communication contracts between the Presentation Layer and the Business Logic Layer.

Controllers communicate only through interfaces, allowing implementations to be replaced without affecting higher system layers.

---

## A. IOrderService

**Responsibility:**  
Handles the complete order lifecycle, including checkout execution, order retrieval, and order status management.

```csharp
public interface IOrderService
{
    // Executes checkout process.
    Task<OrderResponseDto> PlaceOrderAsync(
        string userId,
        CheckoutDto checkoutDetails
    );

    // Retrieves order details.
    Task<OrderDetailsDto> GetOrderByIdAsync(
        int orderId,
        string userId
    );

    // Returns customer's order history.
    Task<IEnumerable<OrderSummaryDto>> GetUserOrderHistoryAsync(
        string userId
    );

    // Updates order status (Admin only).
    Task<bool> UpdateOrderStatusAsync(
        int orderId,
        string status
    );
}

## B. ICartService

**Responsibility:**  
Manages the user's active shopping cart and validates inventory before checkout.

```csharp
public interface ICartService
{
    // Retrieves active shopping cart.
    Task<CartDto> GetActiveCartAsync(
        string userId
    );

    // Adds product to shopping cart.
    Task<bool> AddToCartAsync(
        string userId,
        int productId,
        int quantity
    );

    // Updates existing cart item quantity.
    Task<bool> UpdateCartItemQuantityAsync(
        string userId,
        int cartItemId,
        int newQuantity
    );

    // Removes item from shopping cart.
    Task<bool> RemoveFromCartAsync(
        string userId,
        int cartItemId
    );

    // Clears active cart.
    Task<bool> ClearCartAsync(
        string userId
    );
}
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
## C. CartItemDto

**Purpose:**  
Represents a single product displayed in the shopping cart.

```csharp
public class CartItemDto
{
    public int CartItemId { get; set; }

    public int ProductId { get; set; }

    public string ProductName { get; set; }

    public decimal UnitPrice { get; set; }

    public int Quantity { get; set; }

    public decimal SubTotal => UnitPrice * Quantity;
}
# 10.5 System Validation & Business Rules

Before any checkout transaction is committed, the following validation rules and pipeline operations are strictly enforced.

## Checkout Validation Pipeline

| Step | Validation Action |
|---|---|
| 1 | Verify product stock availability in the warehouse. |
| 2 | Read the live product price from the catalog. |
| 3 | Save and freeze the price inside `OrderItem.PriceSnapshot`. |
| 4 | Generate the base `Order` record. |
| 5 | Generate all associated `OrderItem` records. |
| 6 | Deduct physical inventory stock quantities. |
| 7 | Flush and clear the user's active shopping cart session. |
| 8 | Commit the transaction atomically. |

---
## Checkout Workflow

```mermaid
flowchart TD
    A[Checkout Request] --> B[Verify Stock]
    B --> C[Freeze Product Prices]
    C --> D[Create Order]
    D --> E[Create Order Items]
    E --> F[Update Inventory]
    F --> G[Clear Shopping Cart]
    G --> H[Commit Transaction]
    H --> I[Return Success Response]

    style A fill:#e3f2fd,stroke:#1e88e5,stroke-width:2px
    style H fill:#e8f5e9,stroke:#43a047,stroke-width:2px
    style I fill:#e8f5e9,stroke:#43a047,stroke-width:2px
### Stock Verification

Before creating an order, the application verifies the available inventory for every product in the shopping cart.

If the requested quantity exceeds the available stock, the checkout process is cancelled and a validation error is returned.

---

### Price Snapshot Pattern

During checkout, the current product price is copied into the `OrderItem.PriceSnapshot` field.

This ensures historical transaction and financial data remains accurate even if catalog prices change later.

---

### Atomic Transactions

All checkout operations run within a single shared database transaction.

If any operation fails, the transaction is rolled back automatically to preserve database consistency.

# 10.6 API Design (Sample Endpoints)

All endpoints follow RESTful conventions.

Authentication is required via a secure JSON Web Token (JWT) provided in the `Authorization` header.

---

## POST /api/carts/items

**Purpose:**  
Customer adds a product to their active shopping cart.

| Field | Value |
|---|---|
| Method | POST |
| URL | `/api/carts/items` |
| Authentication | Bearer JWT (Customer role) |
| Content-Type | `application/json` |

### Request Body

```json
{
  "productId": 204,
  "quantity": 2
}
### Success Response (201 Created)

```json
{
  "cartItemId": 982,
  "cartId": 55,
  "productId": 204,
  "quantity": 2,
  "addedAt": "2026-07-16T11:00:00Z"
}
