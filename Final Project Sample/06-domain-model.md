06. Domain Model & Class Diagram

6.1 Identifying Classes from Use Cases

Domain classes are discovered by extracting nouns from use cases, database structures, and e-commerce requirements. The analysis focuses on actors, business entities, persistent data, and system responsibilities.

Source| Nouns Extracted| Candidate Class
UC-004: Checkout & Place Order| customer, cart, product, order, order item, price snapshot, inventory| Customer, Cart, CartItem, Product, Order, OrderItem
UC-007: Book Showroom Visit| customer, showroom, appointment, booking date, time slot, status| ShowroomAppointment
UC-006: Submit Product Review| customer, product, rating, comment, verified status| ProductReview
FR-001: Authentication & Authorization| user, identity, role, email, password| ApplicationUser, Role
FR-006: Dynamic Catalog| category, products, filtering| Category, Product
US-005: Wishlist Management| customer, saved products, wishlist| Wishlist
FR-006: Security Audit Logging| user action, timestamp, affected table| AuditLog

After removing attributes, temporary states, and non-domain concepts, the final domain classes are defined below.

---

6.2 Domain Model

classDiagram

class ApplicationUser {
    -string userId
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
    -string phone
    -string defaultShippingAddress
    +addToCart()
    +checkout()
    +addToWishlist()
    +bookShowroomVisit()
}

class Administrator {
    -int adminId
    -string department
    -int accessLevel
    +manageUsers()
    +manageProducts()
    +viewAuditLogs()
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
    +addItem(Product product,int qty)
    +removeItem(int productId)
    +clearCart()
    +calculateTotal() decimal
}

class CartItem {
    -int cartItemId
    -int quantity
    +updateQuantity(int qty)
}

class Wishlist {
    -int wishlistId
    -datetime createdAt
    +addProduct(Product product)
    +removeProduct(Product product)
    +clearWishlist()
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

class AuditLog {
    -int logId
    -string action
    -string tableAffected
    -datetime timestamp
    +record()
}


ApplicationUser <|-- Customer
ApplicationUser <|-- Administrator

Category "1" --> "*" Product : classifies

Customer "1" --> "1" Cart : owns
Cart "1" --> "*" CartItem : contains
Product "1" --> "*" CartItem : references

Customer "1" --> "1" Wishlist : owns
Wishlist "1" --> "*" Product : contains

Customer "1" --> "*" Order : places
Order "1" --> "*" OrderItem : contains
Product "1" --> "*" OrderItem : snapshotted in

Customer "1" --> "*" ShowroomAppointment : schedules

Customer "1" --> "*" ProductReview : writes
Product "1" --> "*" ProductReview : receives

ApplicationUser "1" --> "*" AuditLog : generates

---
6.3 Class Relationship Summary

Relationship| Type| Description
ApplicationUser → Customer / Administrator| Inheritance| All users share common authentication information while each role extends specific responsibilities.
Category → Product| Association (1:*)| A category contains multiple products, while each product belongs to one category.
Customer → Cart| Composition (1:1)| Each customer owns one active shopping cart. The cart cannot exist independently.
Cart → CartItem| Composition (1:*)| Cart items depend on their parent cart and are removed when the cart is deleted.
Customer → Wishlist| Composition (1:1)| Each customer owns a personal wishlist for saving preferred products.
Wishlist → Product| Association (1:*)| A wishlist contains multiple saved products.
Order → OrderItem| Composition (1:*)| Order items belong exclusively to one order and represent purchased products.
Product → OrderItem| Association (1:*)| Order items store a price snapshot to preserve historical transaction prices.
Customer → ShowroomAppointment| Association (1:*)| Customers may schedule multiple showroom appointments.
Customer → ProductReview| Association (1:*)| Customers can submit reviews for purchased products.
ApplicationUser → AuditLog| Association (1:*)| User actions are recorded for security auditing and accountability.

---

6.4 Enumeration Types

classDiagram

class Role {
    <<enumeration>>
    CUSTOMER
    ADMINISTRATOR
}

class OrderStatus {
    <<enumeration>>
    PENDING
    PROCESSING
    SHIPPED
    DELIVERED
    CANCELLED
}

class AppointmentStatus {
    <<enumeration>>
    PENDING_APPROVAL
    APPROVED
    RESCHEDULED
    CANCELLED
}

class BillingStatus {
    <<enumeration>>
    PENDING_PAYMENT
    PAID
    REFUNDED
}

---

"← Previous: User Stories" (./05-user-stories.md) | "Back to Index" (./README.md) | "Next: UML Behavioral Models →" (./07-uml-behavioral.md)

