# 05. User Stories & Acceptance Criteria

## 5.1 Customer & Guest Epics

- **As a Guest,** I want to register a new account easily so that I can access personalized features like the cart and wishlist.
  - **Acceptance Criteria:** Registration requires unique credentials and automatically assigns the initial role of **Customer**.

- **As a Customer,** I want to save items to a Wishlist so that I can keep track of furniture pieces I like without buying them immediately.
  - **Acceptance Criteria:** Users can view their wishlist and instantly move any item from the wishlist to the active shopping cart.

- **As a Customer,** I want to manage my showroom appointments online so that I can cancel or reschedule if my plans change.
  - **Acceptance Criteria:** The system prevents cancellations made less than **24 hours** before the scheduled appointment.

---

## 5.2 Operations & Management Epics

- **As a Store Manager,** I want an interactive dashboard with real-time sales and inventory metrics so that I can make quick restocking decisions.
  - **Acceptance Criteria:** The dashboard displays real-time charts for:
    - Sales performance
    - Inventory turnover
    - Upcoming showroom visits

- **As an Administrator,** I want to view immutable system audit logs so that I can monitor privileged actions and maintain system accountability.
  - **Acceptance Criteria:** Audit logs are read-only and record the following information for every administrative action:
    - User ID
    - Action type
    - Timestamp
    - Metadata
