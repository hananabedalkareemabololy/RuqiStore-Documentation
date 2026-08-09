# 02. Stakeholder Analysis

## 2.1 Stakeholder Overview

The Ruqi Store system has four primary user roles. Each role has specific responsibilities and permissions within the system.

The four roles are:

1. **Customer**
2. **Store Manager**
3. **Payment Officer**
4. **General Administrator**

These roles represent the main users who interact with the Ruqi Store system and its management functions.

---

# 2.2 Stakeholder Register

|   #   | Stakeholder               | Role                     | Interest                    | Main Responsibilities                                                                                                               |
| :---: | :------------------------ | :----------------------- | :-------------------------- | :---------------------------------------------------------------------------------------------------------------------------------- |
| **1** | **Customer**              | Buyer / System User      | Purchasing furniture online | Browse products, search and filter the catalog, add products to the cart, place orders, track delivery, and submit product reviews. |
| **2** | **Store Manager**         | Store Operations Manager | Managing store operations   | Manage the product catalog, inventory, orders, product categories, fulfillment status, and sales information.                       |
| **3** | **Payment Officer**       | Payment Management User  | Managing customer payments  | Review and confirm customer payments and maintain payment-related records.                                                          |
| **4** | **General Administrator** | System Administrator     | Managing the overall system | Manage users, permissions, reviews, reports, and audit logs.                                                                        |

The presentation explicitly defines these four roles and their main responsibilities.

---

# 2.3 Stakeholder Roles & Needs

---

## 👤 Customer

### Role

The Customer is the user who interacts with Ruqi Store to browse and purchase furniture products.

### Main Responsibilities

The Customer can:

* Browse furniture products.
* Search the product catalog.
* Filter products by:

  * Category
  * Price
  * Material
  * Availability
* Add products to the shopping cart.
* Complete an order.
* Select a delivery address.
* Select a payment method.
* Track the order.
* Submit a product review after delivery.

The customer journey presented in the system is:

**Browse & Search → Add to Cart → Checkout → Order Tracking → Product Review**.

### Customer Permissions

| Function                   | Permission |
| :------------------------- | :--------: |
| Browse Products            |      ✓     |
| Place Order                |      ✓     |
| Submit Product Review      |      ✓     |
| Manage Products            |      —     |
| Manage Inventory           |      —     |
| Confirm / Reject Payment   |      —     |
| Manage Users & Permissions |      —     |

These permissions are defined in the presentation's main permission matrix.

---

## 🪑 Store Manager

### Role

The Store Manager is responsible for managing the operational aspects of the furniture store.

### Main Responsibilities

The Store Manager can:

* Manage the product catalog.
* Add and edit products.
* Manage product images.
* Manage product categories.
* Manage inventory.
* Monitor low-stock alerts.
* Manage orders.
* Update order fulfillment status.
* View sales analytics.
* Monitor revenue.
* Monitor the most frequently sold products.
* Use soft deletion for products.

The presentation specifies that the Store Manager has permissions for catalog, inventory, and order management.

The Store Manager dashboard includes revenue information, active products, low-stock alerts, pending orders, catalog management, inventory management, category management, fulfillment status updates, and sales analytics.

### Store Manager Permissions

| Function                   | Permission |
| :------------------------- | :--------: |
| Browse Products            |      ✓     |
| Manage Products            |      ✓     |
| Manage Inventory           |      ✓     |
| Manage Orders              |      ✓     |
| Confirm / Reject Payment   |      —     |
| Manage Users & Permissions |      —     |

---

## 💳 Payment Officer

### Role

The Payment Officer is responsible for reviewing and confirming customer payment information.

### Main Responsibilities

The Payment Officer can:

* Review customer payment information.
* Review payment evidence.
* Confirm payment.
* Reject payment.
* Maintain payment-related records.

The current payment process is:

1. The customer submits an order online.
2. The order and customer information are stored securely.
3. The Payment Officer reviews the order and payment evidence.
4. The store communicates directly with the customer to confirm payment and delivery.

The currently available payment methods are:

* **Cash on Delivery**
* **Bank Transfer**

A direct online payment gateway such as Stripe may be integrated in a future version.

### Payment Officer Permissions

| Function                   | Permission |
| :------------------------- | :--------: |
| Browse Products            |      ✓     |
| Confirm / Reject Payment   |      ✓     |
| Place Order                |      —     |
| Manage Products            |      —     |
| Manage Inventory           |      —     |
| Manage Users & Permissions |      —     |

---

## 🔒 General Administrator

### Role

The General Administrator is responsible for overall system administration and management.

### Main Responsibilities

The General Administrator can:

* Manage users.
* Activate and deactivate user accounts.
* Search and filter users.
* Assign roles.
* Grant or revoke Store Manager and Payment Officer roles.
* Monitor product reviews.
* Hide or remove inappropriate reviews.
* Generate and access CSV reports.
* Review audit logs.
* View overall system information.

The administration dashboard includes user management, permission management, review monitoring, CSV reports, audit logs, and an overall system overview.

### General Administrator Permissions

| Function                   | Permission |
| :------------------------- | :--------: |
| Browse Products            |      ✓     |
| Manage Users               |      ✓     |
| Manage Roles & Permissions |      ✓     |
| Monitor Reviews            |      ✓     |
| View Reports               |      ✓     |
| View Audit Logs            |      ✓     |
| Manage Products            |      —     |
| Manage Inventory           |      —     |
| Confirm / Reject Payment   |      —     |

---

# 2.4 Main Permission Matrix

The following matrix summarizes the main permissions defined for the four system roles:

| Function                        | Customer | Store Manager | Payment Officer | General Administrator |
| :------------------------------ | :------: | :-----------: | :-------------: | :-------------------: |
| **Browse Products**             |     ✓    |       ✓       |        ✓        |           ✓           |
| **Place Order & Review**        |     ✓    |       —       |        —        |           —           |
| **Manage Products & Inventory** |     —    |       ✓       |        —        |           —           |
| **Confirm / Reject Payment**    |     —    |       —       |        ✓        |           —           |
| **Manage Users & Permissions**  |     —    |       —       |        —        |           ✓           |

This matrix follows the main permission structure presented in the Ruqi Store presentation.

---

# 2.5 Stakeholder Interaction Summary

The main interaction between the four roles can be summarized as follows:

**Customer**

↓

Places Order

↓

**Store Manager**

↓

Manages Order and Fulfillment

↓

**Payment Officer**

↓

Reviews and Confirms Payment

↓

**General Administrator**

↓

Manages Users, Permissions, Reviews, Reports, and Audit Logs

The Customer is primarily responsible for the purchasing journey, while the Store Manager handles store operations, the Payment Officer handles payment confirmation, and the General Administrator handles overall system administration.

---

# 2.6 Stakeholder Responsibilities Summary

| Stakeholder               | Primary Responsibility                                                 |
| :------------------------ | :--------------------------------------------------------------------- |
| **Customer**              | Browse, purchase, track orders, and review products.                   |
| **Store Manager**         | Manage catalog, inventory, orders, fulfillment, and sales information. |
| **Payment Officer**       | Review and confirm customer payments.                                  |
| **General Administrator** | Manage users, permissions, reviews, reports, and audit logs.           |

---

# 2.7 Stakeholder Summary

The Ruqi Store system is centered around four primary system roles:

**Customer → Store Manager → Payment Officer → General Administrator**

The Customer represents the purchasing side of the system, while the Store Manager manages store operations, the Payment Officer manages payment verification, and the General Administrator maintains system-wide administrative control.

---

# Navigation

[← Previous: Project Introduction](https://chatgpt.com/c/01-project-introduction.md) | [Next: Requirements →](https://chatgpt.com/c/03-requirements.md)
