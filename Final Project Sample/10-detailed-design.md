# 10. Detailed Class Design & API Contracts

## 📌 Overview

This document provides a detailed technical specification of the service interfaces, Data Transfer Objects (DTOs), and API contracts that support the **Ruqi Store** application. It serves as a reference for backend implementation by defining the required methods, parameters, and business rules.

---

## 🛠️ 1. Service Contracts & Interfaces

The following interfaces define the core business services within the Business Logic Layer (BLL). Controllers depend on these interfaces rather than concrete implementations, promoting loose coupling and testability.

### A. `IOrderService`

Responsible for checkout, order management, and order history retrieval.

```csharp
public interface IOrderService
{
    // Executes the checkout process.
    Task<OrderResponseDto> PlaceOrderAsync(
        string userId,
        CheckoutDto checkoutDetails);

    // Retrieves detailed information about a specific order.
    Task<OrderDetailsDto> GetOrderByIdAsync(
        int orderId,
        string userId);

    // Returns all orders belonging to a user.
    Task<IEnumerable<OrderSummaryDto>> GetUserOrderHistoryAsync(
        string userId);

    // Updates the order status (Admin only).
    Task<bool> UpdateOrderStatusAsync(
        int orderId,
        string status);
}
```

---

### B. `ICartService`

Responsible for managing the user's active shopping cart.

```csharp
public interface ICartService
{
    // Retrieves the user's active cart.
    Task<CartDto> GetActiveCartAsync(string userId);

    // Adds a product to the cart.
    Task<bool> AddToCartAsync(
        string userId,
        int productId,
        int quantity);

    // Updates the quantity of an existing cart item.
    Task<bool> UpdateCartItemQuantityAsync(
        string userId,
        int cartItemId,
        int newQuantity);

    // Removes an item from the cart.
    Task<bool> RemoveFromCartAsync(
        string userId,
        int cartItemId);

    // Clears the active cart.
    Task<bool> ClearCartAsync(string userId);
}
```

---

## 📦 2. Data Transfer Objects (DTOs)

DTOs transfer data between layers without exposing Entity Framework models directly to the presentation layer.

### `CheckoutDto`

```csharp
public class CheckoutDto
{
    public string ShippingAddress { get; set; }
    public string PaymentMethod { get; set; }
    public string ContactPhoneNumber { get; set; }
}
```

---

### `OrderResponseDto`

```csharp
public class OrderResponseDto
{
    public int OrderId { get; set; }
    public bool IsSuccess { get; set; }
    public string Message { get; set; }
    public DateTime OrderDate { get; set; }
    public decimal TotalAmount { get; set; }
}
```

---

### `CartItemDto`

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
```

---

## 🔒 3. System Validation & Business Rules

Before any transaction is committed, the application enforces the following validation rules.

### Checkout Validation Pipeline

#### Stock Verification

Before creating an order, the system verifies the available quantity of each product.

- If `OrderedQuantity > StockQuantity`, the checkout process is cancelled.
- An `InvalidOperationException` (or an equivalent validation error) is returned to notify the user that the requested quantity is unavailable.

---

#### Price Snapshot

During checkout, the current product price is copied into `OrderItem.PriceSnapshot`.

This preserves historical pricing even if the product price changes after the order is completed.

---

#### Atomic Transaction

The following operations execute within a single database transaction (`IDbContextTransaction`):

1. Create the `Order`.
2. Create all `OrderItems`.
3. Copy the current product price into `PriceSnapshot`.
4. Deduct inventory quantities.
5. Clear the user's shopping cart.

If any operation fails, the transaction is rolled back to maintain data consistency.

---

## ✅ Design Principles Applied

- **Interface-Based Programming** – Controllers depend on interfaces rather than concrete implementations.
- **Dependency Injection (DI)** – Services are resolved through ASP.NET Core's built-in dependency injection container.
- **DTO Pattern** – Separates domain entities from client-facing models.
- **Asynchronous Programming** – All service methods use asynchronous operations (`Task`) to improve scalability and responsiveness.
