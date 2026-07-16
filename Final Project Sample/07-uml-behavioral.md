# 07. UML Behavioral Models

## 7.1 Sequence Diagram: Checkout & Place Order

This diagram illustrates the step-by-step API interactions and database transactions when a customer completes a purchase (UC-004).

```mermaid
sequenceDiagram
    autonumber
    actor C as Customer
    participant UI as Web/Mobile Client
    participant CC as CartController
    participant OC as OrderController
    participant IS as InventoryService
    participant DB as Database
    participant ES as EmailService

    C->>UI: Click "Proceed to Checkout"
    UI->>CC: GET /api/v1/carts/active
    activate CC
    CC->>DB: SELECT cart & cart_items WHERE userId = active
    DB-->>CC: Active cart data with products
    CC-->>UI: 200 OK (Cart payload)
    deactivate CC

    UI->>C: Display Order Summary & Price Snapshot
    C->>UI: Select shipping & enter payment, click "Confirm Order"

    UI->>OC: POST /api/v1/orders
    activate OC

    OC->>IS: validateStockAndReserve(cartItems)
    activate IS

    IS->>DB: SELECT stock_quantity FROM products WHERE id IN (items)
    DB-->>IS: Available stock counts

    alt Stock unavailable for any item
        IS-->>OC: StockValidationException (Insufficient Stock)
        OC-->>UI: 400 Bad Request (Error message)
    else Stock Available
        IS-->>OC: Stock Reserved
    end

    deactivate IS

    OC->>DB: BEGIN TRANSACTION
    OC->>DB: INSERT INTO orders (userId, total, status=PENDING, shipping_address)
    DB-->>OC: generated orderId

    loop For each item in cart
        OC->>DB: INSERT INTO order_items (orderId, productId, priceSnapshot, quantity)
    end

    OC->>DB: UPDATE products SET stock_quantity = stock_quantity - ordered_qty
    OC->>DB: DELETE FROM cart_items WHERE cartId = activeCartId
    OC->>DB: COMMIT TRANSACTION
    DB-->>OC: Transaction committed successfully

    OC->>ES: sendOrderConfirmationEmail(userEmail, orderId)

    OC-->>UI: 201 Created (Order success payload)
    deactivate OC

    UI-->>C: Show confirmation with Order ID & receipt
```

## 7.2 Sequence Diagram: Book Showroom Visit

This diagram tracks the interactions when a customer schedules an appointment to view premium furniture in the physical showroom (UC-007).

```mermaid
sequenceDiagram
    autonumber

    actor C as Customer
    participant UI as Web/Mobile Client
    participant SC as ShowroomController
    participant AS as AppointmentService
    participant DB as Database
    participant NS as NotificationService

    C->>UI: Select showroom & date, click "View Available Times"

    UI->>SC: GET /api/v1/showrooms/{id}/available-slots?date=YYYY-MM-DD

    activate SC

    SC->>AS: getAvailableTimeSlots(showroomId, date)

    activate AS

    AS->>DB: SELECT booked_slots FROM appointments WHERE showroomId = id AND date = selected
    DB-->>AS: Booked appointments array

    AS-->>SC: Filtered list of open 1-hour slots

    deactivate AS

    SC-->>UI: 200 OK (Available slots array)

    deactivate SC

    UI-->>C: Render dynamic time-slot grid

    C->>UI: Select slot & click "Confirm Booking"

    UI->>SC: POST /api/v1/showrooms/appointments

    activate SC

    SC->>AS: scheduleAppointment(userId, showroomId, date, slot)

    activate AS

    AS->>DB: Check double-booking (SELECT COUNT WHERE slot = selected)
    DB-->>AS: 0 (No existing booking for this customer/slot)

    AS->>DB: INSERT INTO appointments (userId, showroomId, date, slot, status=PENDING_APPROVAL)
    DB-->>AS: appointmentId

    AS->>NS: triggerAdminReviewNotification(appointmentId)

    activate NS
    NS-->>AS: Event queued
    deactivate NS

    AS-->>SC: Booking confirmation details

    deactivate AS

    SC-->>UI: 201 Created (Appointment receipt payload)

    deactivate SC

    UI-->>C: Show booking pending screen with SMS details
```
## 7.3 Activity Diagram: Order Processing & Checkout

This flow chart details the complete procedural path of the checkout process, covering inventory verification, rollback scenarios, and error handling.

```mermaid
flowchart TD
    Start([Start Checkout]) --> A[Retrieve active cart items]

    A --> B{Is cart empty?}

    B -->|Yes| C[Display "Empty Cart" warning]
    C --> End1([End])

    B -->|No| D{Are all items in stock?}

    D -->|No| E[Highlight out-of-stock items]
    E --> F[Prompt user to update quantities or remove items]
    F --> A

    D -->|Yes| G[Lock stock quantities temporarily]
    G --> H[Render order summary with Price Snapshot]

    H --> I{User completes payment?}

    I -->|No / Cancelled| J[Release locked stock back to inventory]
    J --> End2([End])

    I -->|Yes| K[Create Order & OrderItem records]
    K --> L[Update physical database stock count]
    L --> M[Clear user active shopping cart]
    M --> N[Send receipt and tracking details to customer]
    N --> End3([End])

    style B fill:#ff9800,color:#fff
    style D fill:#ff9800,color:#fff
    style I fill:#ff9800,color:#fff
    style K fill:#4caf50,color:#fff
```

---

## 7.4 State Machine Diagram: Order Lifecycle

This state machine tracks the dynamic transitions of an order's status from initial cart checkout up to successful shipping, delivery, or cancellation.

```mermaid
stateDiagram-v2
    [*] --> PendingPayment : Checkout initiated

    PendingPayment --> Paid : Payment gateway authorization success
    PendingPayment --> Cancelled : Customer cancel or payment timeout (30 min)

    Paid --> Processing : Warehouse receives order for packaging
    Paid --> Refunded : Item unavailable / immediate cancel request

    Processing --> Shipped : Shipping label generated & courier assigned
    Processing --> Refunded : Quality control failure / stock discrepancy

    Shipped --> Delivered : Courier confirms dropoff at customer shipping address
    Shipped --> Returned : Customer rejects delivery or address unreachable

    Returned --> Refunded : Inventory returned to stock and processed

    Delivered --> [*]
    Cancelled --> [*]
    Refunded --> [*]
```

### State Descriptions & Rules

| State | Description | Allowed Transitions |
|-------|-------------|---------------------|
| **PendingPayment** | Order is registered but payment verification is pending. Stock is temporarily locked. | Paid, Cancelled |
| **Paid** | Payment confirmed by gateway. Order is queued for warehouse processing. | Processing, Refunded |
| **Processing** | Warehouse operators are picking, packing, and preparing the package. | Shipped, Refunded |
| **Shipped** | Package has been handed over to the courier partner; tracking ID is active. | Delivered, Returned |
| **Delivered** | Customer has signed for the delivery. Terminal successful state. | None |
| **Returned / Cancelled** | Orders that failed to deliver or were abandoned. Stock is returned to active catalog. | Refunded (if paid) |

## 7.5 Activity Diagram: Dynamic Price & Discount Calculation

This diagram models the business logic engine for compiling cart subtotals, applying categorical/coupon discounts, calculating region-based shipping, and generating the final billing total.

```mermaid
flowchart TD
    Start([Begin Price Calculation]) --> A[Sum raw unit prices of CartItems]

    A --> B[Apply active category discounts]

    B --> C{Has promotional coupon code?}

    C -->|Yes| D{Is promo code valid?}

    D -->|Yes| E[Apply percentage or flat discount amount]
    D -->|No| F[Flag invalid code & ignore]

    C -->|No| G[Identify delivery region]
    E --> G
    F --> G

    G --> H[Calculate shipping costs based on distance and weight]

    H --> I{Does subtotal exceed free shipping threshold?}

    I -->|Yes| J[Apply free shipping rebate]
    I -->|No| K[Append calculated shipping fee to total]

    J --> L[Calculate applicable local sales tax]
    K --> L

    L --> M[Compile and round grand total to 2 decimal places]
    M --> N[Log static PriceSnapshot for order verification]
    N --> End([End])

    style B fill:#2196f3,color:#fff
    style I fill:#ff9800,color:#fff
    style M fill:#4caf50,color:#fff
```

### Scenario Calculation Example

| Step | Item | Rule Type | Calculation Process | Resulting Subtotal |
|------|------|-----------|---------------------|-------------------:|
| 1 | Raw Cart Total | 3 Premium Items | $400.00 + $250.00 + $150.00 | **$800.00** |
| 2 | Active Promo Code | SUMMER15 (15%) | Subtract 15% of cart subtotal | **$680.00** |
| 3 | Shipping Assessment | Standard Delivery | Distance exceeds zone limit (+$45.00) | **$725.00** |
| 4 | Free Shipping Rule | Threshold Check | Since subtotal ($680.00) > $500.00, shipping rebate applied | **-$45.00** |
| 5 | Sales Tax | Regional Rate (8.25%) | Apply tax to taxable subtotal | **+$56.10** |
| 6 | Final Price Snapshot | Order Grand Total | Rounded final billing amount | **$736.10** |

---

**← Previous:** Domain Model  
**Back to Index**  
**Next:** Database Design →
