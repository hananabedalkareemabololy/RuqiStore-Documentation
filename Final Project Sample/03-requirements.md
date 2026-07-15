# 03. System Requirements Specification

## 3.1 Functional Requirements (Core Features)

1. **Dynamic Catalog Management:** A highly searchable and filterable database enabling users to locate furniture items by category, price, material composition, and real-time stock levels.
2. **Persistent Cart & Wishlist System:** A database-backed shopping cart that preserves chosen items across sessions, complemented by a Wishlist feature for saving items to purchase later.
3. **Atomic Order Processing:** A transaction engine that captures price snapshots at the exact moment of order placement and executes isolated, safe stock deductions.
4. **Order Pipeline Tracking:** A transparent, step-by-step order tracking state pipeline: `Pending` → `Processing` → `Shipped` → `Delivered` → `Cancelled`.
5. **Verified Review System:** A trust-building feature restricting product review submissions exclusively to verified buyers, limiting feedback to exactly one review per customer per product.
6. **Managerial Operations & Analytics:** Back-office tools enabling managers to modify products, control stock, manage physical showroom appointments, and view real-time sales/analytics dashboards.
7. **Global System Administration & Auditing:** High-level dashboards providing user access control, review moderation queues, and inspection of immutable system audit logs.

## 3.2 Non-Functional Requirements & Technical Constraints

* **Performance Metrics:** The system infrastructure must ensure API response windows remain under `500ms` and overall frontend page loading renders in less than `2s`.
* **Data Integrity & Security:** Automatic query parameterization via EF Core to eliminate SQLi, server-side HTML encoding against XSS, and anti-forgery tokens against CSRF.
* **Session Governance:** Cryptographically signed, encrypted, and `HttpOnly` cookie-driven authentication sessions to prevent hijacking.
* **Backup & Recovery:** Automated daily database backups combined with strict validation procedures to ensure rapid system restoration.
