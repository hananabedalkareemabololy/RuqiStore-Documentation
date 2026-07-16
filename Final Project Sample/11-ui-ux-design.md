# 11. UI/UX Design & Frontend Specifications

## 11.1 Design Principles

The Ruqi Store interface follows core usability heuristics and premium e-commerce design patterns to ensure a luxurious, accessible, and seamless shopping experience.

| Principle | Application in Ruqi Store |
|-----------|---------------------------|
| **Consistency** | Standardized layout across all views: persistent top navigation with RTL/LTR language toggles, unified product grids, and consistent padding systems. |
| **Visibility of System Status** | Live cart item count badges, active page indicators, explicit tracking of order status (e.g., "Shipped", "Pending"), and highlighted selected time-slots for showroom bookings. |
| **Feedback** | Action validation via non-intrusive top-end toast notifications (e.g., "Added to Cart"), button loading states during payment processing, and dynamic total recalculations. |
| **Error Prevention & Recovery** | Inline form validation for checkout fields, absolute block on choosing past dates for showroom reservations, and clear, friendly guidance if a voucher code is invalid. |
| **Accessibility (WCAG 2.1 AA)** | Text-to-background contrast ratio of at least 4.5:1. Strict keyboard focus indicator visibility, semantic HTML tag distribution, and descriptive `alt` tags for all furniture items. |

---

## 11.2 Navigation Structure

```mermaid
graph TD

    Login[Login / Landing Page] --> RoleCheck{User Role}

    RoleCheck -->|Guest / Customer| CD[Customer Dashboard]
    RoleCheck -->|Store Admin| AD[Admin Dashboard]

    CD --> HP[Home Page]
    CD --> PL[Product Catalog]
    CD --> SC[Shopping Cart]
    CD --> SB[Showroom Booking]
    CD --> OP[My Orders & Profile]

    PL --> PD[Product Details]
    PD --> SC

    SC --> CO[Checkout Process]
    CO --> OP

    AD --> PM[Manage Catalog]
    AD --> OM[Manage Orders]
    AD --> BM[Manage Showroom Bookings]
    AD --> AR[Sales Reports]
