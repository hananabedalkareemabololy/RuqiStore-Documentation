# 03. System Requirements Specification

## 3.1 Functional Requirements (The 7 Core Features)

1. **Dynamic Catalog Management:** A highly searchable and filterable database enabling users to locate furniture items by category, price, material composition, and real-time stock levels.
2. **Persistent Cart System:** A database-backed shopping cart that preserves chosen items seamlessly across multiple login sessions for authenticated users.
3. **Atomic Order Processing:** A transaction engine that captures price snapshots at the exact moment of order placement and executes isolated, safe stock deductions.
4. **Order Pipeline Tracking:** A transparent, step-by-step order tracking state pipeline: `Pending` → `Processing` → `Shipped` → `Delivered` → `Cancelled`.
5. **Verified Review System:** A trust-building feature restricting product review submissions exclusively to verified buyers, limiting feedback to exactly one review per customer per product.
6. **Managerial Showroom Operations:** Internal back-office tools enabling managers to modify products, control stock levels, fulfill purchases, and book physical showroom visits.
7. **Global System Administration:** High-level dashboards providing user access control, review moderation queues, and secure CSV report compilation.

## 3.2 Non-Functional Requirements & Security Foundations

* **Data Integrity (SQLi Protection):** Automatic query parameterization via Object-Relational Mapping (ORM) to eliminate raw string concatenation vulnerabilities.
* **Cross-Site Scripting (XSS) Defense:** Automated server-side output encoding to render all user-generated strings safely as plain text.
* **Cross-Site Request Forgery (CSRF) Mitigation:** Automatic security token injection into every state-changing HTML form submission.
* **Session Governance:** Cryptographically signed, encrypted, and `HttpOnly` cookie-driven authentication sessions to prevent hijacking.
