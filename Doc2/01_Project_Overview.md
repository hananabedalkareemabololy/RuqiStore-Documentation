# 01 — Project Overview
## Ruqi Store System

---

## 1. Project Idea

The **Ruqi Store System** is a single-vendor, full-stack web application designed exclusively for furniture retail. It enables Ruqi Store to sell its furniture products online through a professional digital storefront.

### Problem
Single-store furniture retailers lack a digital presence, causing lost sales from online and remote buyers. High-ticket furniture items suffer from high cart-abandonment because customers cannot verify material quality, dimensions, or style remotely.

### Solution
A complete online furniture store where:
- **Customers** browse the catalog, filter by category/price/material, place orders, and track delivery.
- **Store Manager** manages the product catalog, inventory, and order fulfillment.
- **Payment Officer** reviews and confirms customer payments.
- **Administrator** manages users, roles, reviews, audit logs, and generates reports.

---

## 2. Project Objectives

### 2.1 Functional Objectives
- Searchable, filterable catalog of all furniture products.
- End-to-end order processing (cart → checkout → order tracking).
- Verified-purchase review system (one review per delivered product per customer).
- Store Manager dashboard for real-time inventory and order management.
- Payment Officer dashboard for payment confirmation workflow.
- Admin panel for complete system oversight.

### 2.2 Non-Functional Objectives
- **Performance:** Page render < 2 seconds; controller action < 500ms at p95.
- **Security:** ASP.NET Core Identity authentication, PBKDF2 password hashing, CSRF protection, parameterized EF Core queries.
- **Reliability:** 99.5% uptime target; automated daily database backups; EF Core transaction rollback on failure.
- **Usability:** Responsive design (320px–2560px); WCAG 2.1 AA compliance; full Arabic RTL support.
- **Scalability:** Clean Service Layer + Repository Pattern enables future scaling; Redis caching planned as a future enhancement.

---

## 3. Technology Stack

| Layer | Technology | Responsibility |
|---|---|---|
| **Presentation** | HTML5, CSS3, Vanilla JavaScript + Razor Views (.cshtml) | Server-rendered UI; no frontend framework |
| **Application Logic** | ASP.NET Core MVC (C#) | Controllers, routing, business logic, security |
| **Data Access** | Entity Framework Core (Code-First) | LINQ-to-SQL; parameterized queries; migrations |
| **Database (Dev)** | SQLite | Zero setup; single `.db` file; easy team sharing |
| **Database (Prod)** | SQL Server | High concurrency; enterprise-scale data volumes |
| **Authentication** | ASP.NET Core Identity | PBKDF2 hashing, UserManager, SignInManager, role claims |

> **Architecture:** Monolithic ASP.NET Core MVC — Controller → Service Layer → Repository → EF Core → Database. There is no separate API server. There is no Node.js. There is no React.

---

## 4. The Four Roles

| Role | Description |
|---|---|
| **Customer** | Registers, browses furniture, places orders, tracks delivery, submits verified reviews |
| **Store Manager** | Manages product catalog, inventory, categories, and order fulfillment |
| **Payment Officer** | Reviews payment submissions; marks orders Paid or Rejected; maintains payment log |
| **Administrator** | Full system oversight — user management, role assignment, review moderation, audit logs, CSV reports |

---

## 5. Core Features

| # | Feature | Description |
|---|---|---|
| 1 | Furniture Product Catalog | Searchable and filterable (category, price, material, stock) |
| 2 | Shopping Cart | Persistent DB-backed cart with real-time stock validation |
| 3 | Checkout & Order Placement | Atomic EF Core transaction; price snapshot; stock deduction |
| 4 | Order Tracking | Pending → Processing → Shipped → Delivered → Cancelled |
| 5 | Payment Management | Payment Officer marks orders Paid or Rejected; full log maintained |
| 6 | Product Review System | Verified-purchase only; one review per product per customer |
| 7 | Store Manager Dashboard | Product CRUD, inventory control, order processing, sales analytics |
| 8 | Admin Panel | User management, role assignment, review moderation, audit logs, reports |
| 9 | File Upload | Up to 8 product images; JPEG/PNG/WebP; max 5MB; magic-byte validated |
| 10 | Address Book | Up to 5 saved delivery addresses per customer |
| 11 | Bilingual UI | Arabic (RTL) + English (LTR); language stored in cookie |

---

## 6. What This System Does NOT Include

| Excluded | Reason |
|---|---|
| Multi-vendor marketplace | Single-store model only |
| Non-furniture categories | Furniture domain only |
| Showroom / appointment booking | Online-only system |
| Online payment gateway (Stripe, PayPal) | Current model: Cash on Delivery + Bank Transfer; direct customer contact |
| Native mobile application | Web-responsive only; future phase |
| Wishlist | Planned future feature under development |
| Redis caching | Future enhancement — planned to improve performance at scale |
| AR room visualization | Future phase |

---

## 7. Payment Philosophy

The current purchasing workflow is intentionally designed for high-ticket furniture retail:

1. Customer places an order online.
2. Customer information and order details are stored securely in the database.
3. The Payment Officer reviews the order and payment submission.
4. The store contacts the customer directly to confirm payment details, product availability, and delivery arrangements.

This approach is common in premium furniture retail because:
- Furniture purchases often require personalized consultation before final payment.
- Direct communication builds customer trust and allows the store to provide tailored recommendations.
- It enables discussion of delivery logistics, assembly, and customization options.
- It creates a personal shopping experience that online-only transactions cannot provide.

> Stripe or another online payment gateway may be integrated in a future version of the system.

---

## 8. Future Enhancements

| Enhancement | Description |
|---|---|
| Wishlist | Allow customers to save favorite products for future purchases |
| Redis Caching | Improve performance and response time at scale |
| Stripe / Payment Gateway | Enable direct online payment processing |
| AR Room Visualizer | Allow customers to visualize furniture in their space |
| Native Mobile App | iOS and Android clients consuming the same backend |

