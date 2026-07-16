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

## 11.3 Wireframes

### A. Product Catalog Page (RTL View Example)

```text
┌─────────────────────────────────────────────────────────────┐
│ [Ruqi Logo]   [البحث عن أثاث...]      [العربية/EN]  [🛒 السلة (2)] │
│ الرئيسية | المنتجات | المجموعات | صالة العرض | من نحن      [الحساب] │
├─────────────────────────────────────────────────────────────┤
│ الرئيسية > المنتجات                                         │
│                                                             │
│ تصنيف المنتجات: [ الكل ] [ مقاعد ] [ طاولات ] [ أرائك ] [ إضاءة ] │
├─────────────┬───────────────────────────────────────────────┤
│ تصفية حسب   │ المنتجات المميزة                              │
│             │ ┌──────────────────────┐ ┌──────────────────┐ │
│ السعر       │ │     [صورة المنتج]     │ │  [صورة المنتج]   │ │
│ [=======]   │ │                      │ │                  │ │
│             │ │ كرسي مكتب مريح        │ │ طاولة خشب طبيعي  │ │
│ المواد      │ │ 625.00 ر.س           │ │ 1,250.00 ر.س     │ │
│ [ ] خشب     │ │ [🛒 أضف للسلة] [♡]   │ │ [🛒 أضف للسلة] [♡]│ │
│ [ ] معدن    │ └──────────────────────┘ └──────────────────┘ │
│             │                                                │
│ الألوان     │ ┌──────────────────────┐ ┌──────────────────┐ │
│ [ ] بيج     │ │     [صورة المنتج]     │ │  [صورة المنتج]   │ │
│ [ ] رمادي   │ │ أريكة مخملية فاخرة     │ │ وحدة إضاءة مودرن │ │
│             │ │ 3,400.00 ر.س         │ │ 450.00 ر.س       │ │
│             │ │ [🛒 أضف للسلة] [♡]   │ │ [🛒 أضف للسلة] [♡]│ │
│             │ └──────────────────────┘ └──────────────────┘ │
├─────────────┴───────────────────────────────────────────────┤
│ © 2026 متجر رقي للأثاث الفاخر. جميع الحقوق محفوظة.          │
└─────────────────────────────────────────────────────────────┘
