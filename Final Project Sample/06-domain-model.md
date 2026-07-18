# 06. Domain Model & Class Diagram

## 6.1 Identifying Classes from Use Cases

Domain classes are discovered by extracting nouns from use case descriptions, database structures, and e-commerce requirements. The key technique is to examine actors, objects mentioned in purchasing/booking scenarios, and persistent data.

| Source | Nouns Extracted | Candidate Class |
| :--- | :--- | :--- |
| **UC-004: Checkout & Place Order** | customer, shopping cart, product, order, order item, price snapshot, inventory stock | Customer, Cart, CartItem, Product, Order, OrderItem |
| **UC-007: Book Showroom Visit** | customer, showroom, appointment, booking date, time slot, status | ShowroomAppointment |
| **UC-006: Submit Product Review** | customer, product, review rating, comments, verified status | ProductReview |
| **FR-002: Role-Based Access Control** | user, identity, role, email, password | ApplicationUser, Role |
| **FR-006: Dynamic Catalog** | category, product catalog, search filters | Category |

After filtering out attributes, transient request states, and out-of-scope system nouns, the final structural domain classes are defined below.

---

## 6.2 Domain Model

```mermaid
classDiagram
    class User {
        -int userId
        -string firstName
        -string lastName
        -string email
        -string passwordHash
        -Role role
        -boolean isActive
        -datetime createdAt
        +login()
        +logout()
        +updateProfile()
        +resetPassword()
    }

    class Customer {
        -int customerId
        -string defaultShippingAddress
        -string phone
        +addToCart()
        +checkout()
        +bookShowroomVisit()
    }

    class StoreManager {
        -int managerId
        -string employeeId
        +updateProductCatalog()
        +manageStock()
        +approveAppointment()
    }

    class Product {
        -int productId
        -string sku
        -string name
        -string description
        -decimal price
        -int stockQuantity
        -string material
        -string dimensions
        -boolean isActive
        +updateStock(int quantity)
        +deactivate()
        +isLowStock() boolean
    }

    class Category {
        -int categoryId
        -string name
        -string description
        -boolean isActive
        +getProducts()
    }

    class Cart {
        -int cartId
        -datetime createdAt
        -datetime updatedAt
        +addItem(Product product, int qty)
        +removeItem(int productId)
        +clearCart()
        +calculateTotal() decimal
    }

    class CartItem {
        -int cartItemId
        -int quantity
        +updateQuantity(int qty)
    }

    class Order {
        -int orderId
        -datetime orderDate
        -OrderStatus orderStatus
        -BillingStatus billingStatus
        -decimal totalAmount
        -string shippingAddress
        +updateStatus(OrderStatus status)
        +cancelOrder()
    }

    class OrderItem {
        -int orderItemId
        -decimal priceSnapshot
        -int quantity
        +getSubtotal() decimal
    }

    class ShowroomAppointment {
        -int appointmentId
        -string showroomLocation
        -datetime appointmentDate
        -string timeSlot
        -AppointmentStatus status
        +approve()
        +reschedule(datetime newDate)
        +cancel()
    }

    class ProductReview {
        -int reviewId
        -int rating
        -string comment
        -datetime submittedAt
        -boolean isModerated
        +approveReview()
        +flagReview()
    }

    %% Inheritance
    User <|-- Customer
    User <|-- StoreManager

    %% Associations
    Category "1" --> "*" Product : classifies
    Customer "1" --> "1" Cart : owns
    Cart "1" --> "*" CartItem : contains
    Product "1" --> "*" CartItem : references
    Customer "1" --> "*" Order : places
    Order "1" --> "*" OrderItem : contains
    Product "1" --> "*" OrderItem : snapshotted in
    Customer "1" --> "*" ShowroomAppointment : schedules
    Customer "1" --> "*" ProductReview : writes
    Product "1" --> "*" ProductReview : receives
```
