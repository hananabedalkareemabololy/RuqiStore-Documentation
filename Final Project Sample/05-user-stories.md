# 05. User Stories & Acceptance Criteria

## 5.1 Customer & Guest Epics

* **As a Guest,** I want to register a new account easily so that I can access personalized features like the cart and wishlist.
  * **Acceptance Criteria:** Registration requires unique credentials and automatically sets the initial role to 'Customer'.

* **As a Customer,** I want to save items to a Wishlist so that I can keep track of furniture pieces I like without buying them immediately.
  * **Acceptance Criteria:** Users must be able to view their wishlist and instantly transfer any item from the wishlist directly into the active cart.

* **As a Customer,** I want to manage my showroom appointments online so that I can cancel or reschedule if my plans change.
  * **Acceptance Criteria:** Cancellations are strictly blocked by the system if the request is made less than 24 hours before the scheduled time slot.

## 5.2 Operations & Management Epics

* **As a Store Manager,** I want an interactive dashboard with real-time sales and inventory metrics so that I can make quick restocking decisions.
  * **Acceptance Criteria:** The dashboard must display active charts for sales performance, inventory turnover, and upcoming showroom visits.

* **As an Administrator,** I want to view immutable system audit logs so that I can monitor privileged actions and maintain system accountability.
  * **Acceptance Criteria:** Audit logs must be read-only and capture user ID, action type, timestamp, and metadata for every administrative change.
