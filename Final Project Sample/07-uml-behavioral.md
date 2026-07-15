# 07. UML Behavioral Diagrams

## 📌 Overview

This document outlines the dynamic behavior and interactions within the **Ruqi Store** system. While the Domain Model defines the static structure, these behavioral diagrams illustrate how system components collaborate over time to support two key business use cases:

- **User Checkout & Order Placement**
- **Inventory Synchronization After Purchase**

---

## 🔄 1. Checkout & Order Placement (Sequence Diagram)

This Sequence Diagram illustrates the interactions between the customer, the user interface, the shopping cart, and the order controller during the checkout process.

```mermaid
sequenceDiagram
    autonumber

    actor Customer as Customer (AppUser)
    participant UI as Client Interface / Cart Page
    participant CartCtrl as Cart Controller
    participant OrderCtrl as Order Controller
    participant DB as System Database

    Customer->>UI: Click "Proceed to Checkout"
    activate UI

    UI->>CartCtrl: GetActiveCart(UserId)
    activate CartCtrl

    CartCtrl->>DB: Query Cart & CartItems
    DB-->>CartCtrl: Return Cart Details (Products, Quantities)
    CartCtrl-->>UI: Return Cart Data
    deactivate CartCtrl

    UI->>Customer: Display Order Summary & Price Snapshot
    Customer->>UI: Confirm Order (Payment Information)

    UI->>OrderCtrl: CreateOrder(UserId, CartId)
    activate OrderCtrl

    OrderCtrl->>DB: Verify Product Stock
    DB-->>OrderCtrl: Stock Available

    OrderCtrl->>DB: Save Order (Pending)
    OrderCtrl->>DB: Copy CartItems to OrderItems (Store PriceSnapshot)
    OrderCtrl->>DB: Clear Active Cart
    DB-->>OrderCtrl: Transaction Committed

    OrderCtrl-->>UI: Order Created Successfully (OrderId)
    deactivate OrderCtrl

    UI-->>Customer: Display Order Success Screen & Receipt
    deactivate UI
```

---

## 📦 2. Inventory Adjustment System (Sequence Diagram)

This workflow illustrates how inventory is automatically updated after an order is successfully placed, preventing overselling.

```mermaid
sequenceDiagram
    autonumber

    participant OrderCtrl as Order Controller
    participant StockService as Inventory Manager
    participant DB as System Database
    actor Admin as Store Manager

    OrderCtrl->>StockService: DeductStock(List<OrderItems>)
    activate StockService

    loop For Each Order Item
        StockService->>DB: Update Product StockQuantity = StockQuantity - OrderedQty
        DB-->>StockService: Stock Updated
    end

    alt Stock falls below threshold (< 5)
        StockService->>DB: Create Low Stock Alert
        StockService-->>Admin: Send Low Stock Notification
    end

    StockService-->>OrderCtrl: Inventory Updated Successfully
    deactivate StockService
```

---

## 📑 3. Behavioral Rules & Constraints

To ensure data integrity and provide a seamless checkout experience, the system enforces the following rules:

- **Price Snapshot:** During checkout, each `OrderItem` stores a fixed `PriceSnapshot`. Future changes to the associated `Product` price do not affect historical orders.

- **Atomic Transaction:** Stock validation, order creation, inventory deduction, and cart clearance are executed within a single database transaction to prevent race conditions and overselling.

- **Automatic Cart Clearance:** After a successful order, all `CartItem` records associated with the user's active cart are automatically removed.

- **Low Stock Monitoring:** When a product's stock falls below the predefined threshold, the system automatically generates a low-stock alert for the administrator.
