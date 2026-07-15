# 07. UML Behavioral Diagrams

## 📌 Overview
This document outlines the dynamic behavior and interactions within the **Ruqi Store** system. While the Domain Model defines the static structure, these behavioral diagrams illustrate how entities interact over time to fulfill key business use cases: **User Checkout & Order Placement** and **Inventory Sync on Purchase**.

---

## 🔄 1. Checkout & Order Placement (Sequence Diagram)

This Sequence Diagram illustrates the step-by-step interaction between the Customer, the Application Interface, the Shopping Cart, and the Order Controller when a purchase is finalized.

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
    Customer->>UI: Confirm and Place Order (Payment Info)
    
    UI->>OrderCtrl: CreateOrder(UserId, CartId)
    activate OrderCtrl
    OrderCtrl->>DB: Check Product Stock Levels
    DB-->>OrderCtrl: Stock Available

    OrderCtrl->>DB: 1. Save Order (Pending)
    OrderCtrl->>DB: 2. Copy CartItems to OrderItems (Freeze PriceSnapshot)
    OrderCtrl->>DB: 3. Clear User's Active Cart
    DB-->>OrderCtrl: Transaction Success
    
    OrderCtrl-->>UI: Order Placed Successfully (OrderId)

``` ​
## 📦 2. Inventory Adjustment System (Sequence Diagram)

This workflow shows how the system automatically manages stock levels immediately after an order is successfully processed to prevent overselling.

```mermaid
sequenceDiagram
    autonumber
    participant OrderCtrl as Order Controller
    participant StockService as Inventory Manager
    participant DB as System Database
    actor Admin as Store Manager / Admin

    OrderCtrl->>StockService: DeductStock(List<OrderItems>)
    activate StockService
    
    loop For Each Item in Order
        StockService->>DB: Update Product Set StockQuantity = StockQuantity - OrderedQty
        DB-->>StockService: Row Updated
    end

    alt Stock falls below Threshold (e.g., < 5)
        StockService->>DB: Create Low Stock Alert (ProductId)
        StockService-->>Admin: Send Low Stock Notification (SKU Alert)
    end

    StockService-->>OrderCtrl: Inventory Deducted & Verified
    deactivate StockService
```
## 📑 3. Behavioral Rules & Constraints

To ensure data integrity and a seamless user experience during these operations, the system enforces the following rules:

* **Price Freeze Constraint:** When a user checks out, the `OrderItem` price must be saved as a static snapshot (`PriceSnapshot`). If the price of the actual `Product` changes in the catalog later, the historical order invoice remains unaffected.
* **Concurrency Lock:** During checkout, stock verification and stock deduction happen in a single database transaction. This prevents two users from checking out the last available item simultaneously.
* **Automatic Cart Clearance:** Upon successful order confirmation, the application must immediately purge all related `CartItem` records linked to the user's active `Cart` ID.
    deactivate OrderCtrl
    UI-->>Customer: Display Order Success Screen & Receipt
    deactivate UI
