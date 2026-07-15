# 06. Domain Model & Object Structure

## 📌 Overview
The Domain Model represents the core conceptual schema and entity relationships operating inside the **Ruqi Store** ecosystem. It illustrates how business data flows and bridges the gap between the database architecture and the application logic layer.

---

## 🗺️ Visual Domain Entity Map

```text
  ┌──────────────┐             ┌──────────────┐             ┌──────────────┐
  │   Category   │1           *│   Product    │1           *│  OrderItem   │
  ├──────────────┤────────────►├──────────────┤────────────►├──────────────┤
  │ Id           │             │ Id           │             │ Id           │
  │ Name         │             │ Name         │             │ PriceSnapshot│
  └──────────────┘             │ Price        │             │ Quantity     │
                               │ SKU          │             └──────┬───────┘
                               │ IsActive     │                    │
                               └──────┬───────┘                    │ *
                                      │                            │
                                      │ 1                          │
                                      ▼ *                          │
  ┌──────────────┐1          1┌──────────────┐1           *┌──────▼───────┐
  │AppUser(Identity)─────────►│     Cart     │             │    Order     │
  ├──────────────┤            ├──────────────┤             ├──────────────┤
  │ Id           │            │ Id           │             │ Id           │
  │ Email        │            └──────┬───────┘             │ OrderStatus  │
  │ Role         │                   │ 1                   │ OrderDate    │
  └──────┬───────┘                   │                     └──────────────┘
         │                           │ *
         │ 1                  ┌──────▼───────┐
         ▼ *                  │   CartItem   │
  ┌──────────────┐            ├──────────────┤
  │   Wishlist   │            │ Id           │
  ├──────────────┤            │ Quantity     │
  │ Id           │            └──────────────┘
  │ ProductId    │
  └──────────────┘

## 👥 Core Domain Entities & Structural Attributes

### 1. User & Identity Management
* **`ApplicationUser` (Inherits from IdentityUser):** Represents any authenticated actor in the system[cite: 1]. Holds core registration properties and links directly to customer profiles, specific roles (Customer, Store Manager, Accountant, Administrator), persistent carts, and personal wishlists[cite: 1].

### 2. Catalog & Product Inventory
* **`Product`:** Defines a specific furniture item (e.g., Sofa, Table) with critical commercial attributes: `ProductId`, `Name`, `Price`, `SKU`, `StockQuantity`, and a status toggle `IsActive` to handle soft-deletes[cite: 1].
* **`Category`:** Groups related furniture products (e.g., Living Room, Bedroom Office). Holds `CategoryId` and `Name`[cite: 1].

### 3. Shopping Session & Customer Selection
* **`Cart`:** A unique, database-backed shopping instance bound directly to a single active user account (`ApplicationUser`)[cite: 1].
* **`CartItem`:** A junction entity pairing a specific `Product` with the requested `Quantity` inside an active cart session[cite: 1].
* **`Wishlist`:** A lightweight data structure linking an individual user profile (`UserId`) to multiple favorited `Products` for long-term purchasing consideration[cite: 1].

### 4. Transactions & Order Management
* **`Order`:** Captures a completed checkout event[cite: 1]. Enforces the structural lifecycle via state properties (`OrderStatus` mapping to Pending, Processing, Shipped, Delivered, or Cancelled) and stores absolute `OrderDate` records[cite: 1].
* **`OrderItem`:** Represents an immutable, frozen snapshot of an item at the exact moment of checkout, capturing `PriceSnapshot` and exact transaction quantities[cite: 1].

---

## 🔗 Key Data Relationships Defined

* **One-to-Many (`Category` ──► `Products`):** One furniture category houses multiple products; each independent product maps to exactly one category parent[cite: 1].
* **One-to-One (`ApplicationUser` ◄──► `Cart`):** Strict relationship constraint enforced by a unique index on the entity's `UserId`[cite: 1].
* **One-to-Many (`ApplicationUser` ──► `Wishlist`):** An authenticated user can catalog multiple desired items within their active wishlist database[cite: 1].
* **One-to-Many (`Order` ──► `OrderItems`):** A single completed purchase invoice captures and renders multiple distinct line items[cite: 1].
