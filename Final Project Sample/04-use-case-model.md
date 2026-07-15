# 04. Use Case Model & System Interactions

## 📌 Overview
This document consolidates the primary use cases for the **Ruqi Store** Furniture system. It details the functional interactions derived from the core feature specifications, logically grouped by actor type and system boundaries.

---

### 1. Guest (Unauthenticated User) Use Cases
* **Browse Catalog:** View paginated product listings, search by keywords, and apply advanced filters (price, material, availability).
* **View Product Details:** Inspect comprehensive product information, specification tables, image galleries, and verified customer reviews.
* **Register Account:** Create a new user profile by providing secure credentials to transition from a Guest to a Registered Customer.

---

### 2. Registered Customer Use Cases
* **Cart Management (Add / Update / Remove):** Insert furniture items into a persistent shopping cart, modify item quantities, or remove products.
* **Secure Checkout:** Execute a structured multi-step pipeline (Review Cart → Shipping Address → Order Summary → Place Order) backed by atomic transaction processing and immediate stock deduction.
* **Order History & Tracking:** Access a historical log of all personal purchases and monitor the live tracking pipeline status (`Pending` → `Processing` → `Shipped` → `Delivered`).
* **Book Showroom Appointment:** Select an available date/time slot from the showroom calendar, specify furniture items of interest, and submit a high-ticket buyer consultation request.
* **Manage Appointments:** View upcoming scheduled visits, cancel appointments (permitted only if >24 hours before the slot), or request a reschedule.
* **Submit Verified Review:** Write a text review and assign a rating restricted exclusively to items that have been successfully marked as `Delivered` (Limit: 1 review per product).
* **Wishlist Management:** Save favorite furniture pieces to a personal wishlist for later consideration, with the ability to instantly transfer items to the active cart.

---

### 3. Store Manager Use Cases
* **Dashboard Overview:** Monitor real-time business KPIs, tracking daily sales volumes, critical inventory shortages, and upcoming showroom visits.
* **Product Management:** Complete CRUD operations over the furniture catalog—create new items, edit specifications, execute safe soft-deletes, and restock inventory levels.
* **Order Fulfillment:** Progress customer orders through the shipping pipeline, initiate structured financial refunds, and compile printable customer invoices.
* **Appointment Management:** Review incoming showroom visit requests to officially confirm, reject, or propose modifications to bookings based on actual showroom availability.
* **Analytics & Reporting:** Export comprehensive dynamic reports regarding sales performance, inventory turnover rates, and appointment engagement metrics.

---

### 4. Administrator Use Cases
* **User Administration:** Complete operational oversight of accounts—activate or deactivate user profiles, and safely assign or revoke security roles (e.g., elevating a user to Accountant or Store Manager).
* **System Configuration:** Manage global e-commerce application variables, adjust localization defaults, and control administrative feature toggles.
* **Review Moderation:** Inspect the global review queue to approve constructive customer feedback or delete inappropriate, spam, or terms-of-service violating submissions.
* **Audit Log Inspection:** Access and review immutable system logs capturing details of all privileged administrative actions for security compliance.

---

### 5. Non-Functional System Use Cases
* **Performance Monitoring:** Enforce underlying infrastructure constraints ensuring API response windows remain `< 500ms` and overall frontend page compilation renders in `< 2s`.
* **Security Governance & Auditing:** Validate secure token handling, verify cross-layer role enforcement attributes, and check data encryption metrics at rest and in transit.
* **Backup & Disaster Recovery:** Execute automated daily database state backups combined with strict validation procedures to ensure rapid system restoration in an emergency.
