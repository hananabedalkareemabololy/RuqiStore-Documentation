# 4. Use Case Analysis

## 4.1 Use Case Diagram

```mermaid
graph TB

    subgraph RuqiStore["Ruqi Store E-Commerce System"]
        UC1((Login & Session Management))
        UC2((Browse Catalog & Filter))
        UC3((Manage Shopping Cart))
        UC4((Checkout & Place Order))
        UC5((Manage Curated Wishlist))
        UC6((Submit Verified Product Review))
        UC7((Book Showroom Visit))
        UC8((Manage Products & Categories))
        UC9((Approve / Reschedule Showroom Visits))
        UC10((Audit Payments & Billing Status))
        UC11((Manage User Roles & Security Audits))
        UC12((Send Transactional Email))
    end

    Customer[Customer]
    Manager[Store Manager]
    Accountant[Accountant]
    Admin[Administrator]
    Email[Email Service]

    Customer --> UC2
    Customer --> UC3
    Customer --> UC4
    Customer --> UC5
    Customer --> UC6
    Customer --> UC7

    Manager --> UC8
    Manager --> UC9

    Accountant --> UC10

    Admin --> UC11

    UC3 -.->|include| UC1
    UC4 -.->|include| UC1
    UC5 -.->|include| UC1
    UC6 -.->|include| UC1
    UC7 -.->|include| UC1

    UC4 -.->|extend| UC12
    UC8 -.->|extend| UC12

    UC12 --> Email

---

## 4.2 Relationships Explanation

### Include Relationship: Login & Session Management

Operations that modify personalized customer data require an active authenticated session before execution.

Included operations:

- Manage Shopping Cart
- Checkout & Place Order
- Wishlist Management
- Product Reviews
- Showroom Booking


### Extend Relationship: Transactional Email

Some system actions optionally trigger external email notifications:

- Successful orders generate confirmation emails and invoices.
- Inventory events such as low-stock alerts may generate notifications.

The Email Service acts as an external system.


---

# 4.3 Use Case Descriptions

---

# UC-004: Checkout & Place Order

## Fully Dressed Use Case

| Field | Detail |
|---|---|
| Use Case ID | UC-004 |
| Name | Checkout & Place Order |
| Actor | Customer |
| Description | A registered customer reviews their shopping cart, provides shipping information, and finalizes the purchase while locking prices and reducing stock. |

## Preconditions

- Customer is logged in.
- Cart contains at least one active item.
- All products have sufficient physical stock.

## Postconditions

- Database transaction is committed.
- Product prices are frozen in Order Items.
- Stock quantities are reduced.
- Cart is cleared.
- Invoice is generated.
- Confirmation email is queued.

## Trigger

Customer clicks **Proceed to Checkout**.
## Main Success Scenario

| Step | Action |
|---|---|
| 1 | Customer reviews cart and clicks Proceed to Checkout. |
| 2 | System requests shipping address and billing information. |
| 3 | Customer enters valid information and confirms order. |
| 4 | System starts a database transaction. |
| 5 | System locks product records and validates stock availability. |
| 6 | System deducts purchased quantities from inventory. |
| 7 | System stores current prices as PriceSnapshot values. |
| 8 | System removes purchased items from the cart. |
| 9 | System commits transaction and creates invoice with Pending Payment status. |
| 10 | System displays confirmation number and sends email notification. |


---

## Alternative Flow

| ID | Condition | Steps |
|---|---|---|
| A1 | Customer has no saved address | System requests a new address, validates it, saves it, and continues checkout. |


---

## Exception Flows

| ID | Condition | Steps |
|---|---|---|
| E1 | Insufficient Stock | Transaction is rolled back and user receives stock warning. |
| E2 | Payment Gateway Failure | Order transaction is cancelled and user receives failure message. |


---

## Business Rules

- Product prices must be frozen during checkout.
- Historical orders must not change after catalog price updates.
- Checkout must run inside a database transaction to prevent overselling.


---

# UC-007: Book Showroom Visit

## Fully Dressed Use Case

| Field | Detail |
|---|---|
| Use Case ID | UC-007 |
| Name | Book Showroom Visit |
| Actor | Customer |
| Description | Customer schedules a showroom appointment to view furniture products physically. |

## Preconditions

- Customer is authenticated.
- Selected time slot is available.
- Booking is within business hours.

## Postconditions

- Appointment record is created.
- Status becomes Pending Approval.
- Manager receives notification.

## Trigger

Customer selects **Book Showroom Visit**.
## UC-007 Main Success Scenario

| Step | Action |
|---|---|
| 1 | Customer selects showroom location. |
| 2 | System displays available dates and time slots. |
| 3 | Customer chooses a slot and confirms booking. |
| 4 | System validates slot availability. |
| 5 | System creates appointment record in the database. |
| 6 | System displays booking confirmation details. |


---

## Alternative Flow

| ID | Condition | Steps |
|---|---|---|
| A1 | Customer edits booking | Customer selects another available slot and updates appointment details. |


---

## Exception Flow

| ID | Condition | Steps |
|---|---|---|
| E1 | Double booking detected | System rejects the request and asks customer to select another available slot. |


---

# UC-006: Submit Verified Product Review

## Brief Description

| Field | Detail |
|---|---|
| Use Case ID | UC-006 |
| Name | Submit Verified Product Review |
| Actor | Customer |
| Description | Customer submits a rating and review only for products they have purchased. |


---

## Main Flow

1. Customer opens the purchased product page.
2. System verifies that the customer has a matching delivered order.
3. Customer enters rating (1-5 stars) and review text.
4. System checks for existing reviews to prevent duplicates.
5. Customer submits the review for Administrator moderation.


---

## Preconditions

- Customer is logged in.
- Product exists in a Delivered order.
- Customer has not reviewed this product before.


---

## Postconditions

- Review record is saved.
- Review status becomes Pending Moderation.


---

## Navigation

⬅ Previous: Requirements Specification

🏠 Back to Index

➡ Next: User Stories
