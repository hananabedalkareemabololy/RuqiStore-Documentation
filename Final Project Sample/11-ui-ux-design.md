# 11. UI/UX Design & Frontend Specifications

## 📌 Overview
This document defines the user interface (UI) specifications, user experience (UX) guidelines, and design system constraints for the **Ruqi Store** web application. It outlines how the application ensures accessibility, seamless localization (RTL/LTR), and responsive layouts to deliver a premium furniture e-commerce experience.

---

## 🌐 1. Globalization & Localization (RTL / LTR)

To serve a diverse audience, Ruqi Store supports bilingual capabilities (Arabic and English). The layout dynamically adapts based on the selected language's text direction (`dir="rtl"` or `dir="ltr"`).

- **CSS Framework:** When Arabic is selected, the application loads the Bootstrap 5 RTL stylesheet.
- **Alignment & Logical Properties:** Text is right-aligned in Arabic and left-aligned in English. All styles use logical CSS properties (e.g., `margin-inline-start`, `padding-inline-end`) instead of physical properties (`margin-left`, `padding-right`) so layouts automatically adapt to the document direction.
- **Language Toggle:** A persistent language selector is available in the main navigation bar. The selected language is stored using ASP.NET Core localization cookies (e.g., `.AspNetCore.Culture`) to persist user preferences across sessions.

---

## 📱 2. Responsive Design Grid System

The interface automatically adapts to different screen sizes to provide an optimal shopping experience.

### Mobile (320px–767px)
- Single-column layout.
- Navigation collapses into an accessible hamburger menu.
- Product tables and shopping cart lists stack vertically or allow safe horizontal scrolling.

### Tablet (768px–1023px)
- Two-column product grids.
- Optimized spacing for touch interactions.

### Desktop (1024px and above)
- Three- or four-column product grids depending on the available screen width.
- Fixed sidebar navigation for user accounts and admin dashboards.

---

## 💬 3. Feedback, Validation & Error Messaging

### Form Validation

- **Client-Side Validation:** ASP.NET Core MVC with jQuery Unobtrusive Validation validates user input before form submission.
- **Server-Side Validation:** Validation errors returned by the server are mapped back to the `ModelState` and displayed beside the corresponding form fields.
- **Error Handling:** Users never see raw system exceptions or database errors. Unexpected errors are redirected to a custom user-friendly error page.

### Interactive States & Notifications

- **Loading States:** During form submission (e.g., Login or Checkout), the submit button is disabled and displays an inline loading spinner to prevent duplicate requests.
- **Toast Notifications:** AJAX operations such as adding products to the cart or wishlist display a temporary loading indicator followed by a success or error toast notification positioned at the **top-end** of the page.

---

## ♿ 4. Accessibility & Visual Design

### WCAG 2.1 AA Compliance

- **Color Contrast:** All text maintains a minimum contrast ratio of **4.5:1** in accordance with WCAG 2.1 AA.
- **Keyboard Navigation:** All interactive elements are fully accessible using the keyboard (`Tab` navigation). Visible focus indicators (`:focus-visible`) are preserved.
- **ARIA & Semantic HTML:** Icon-only buttons include `aria-label` attributes, furniture images contain descriptive `alt` text, and semantic HTML elements such as `<header>`, `<main>`, `<nav>`, and `<footer>` are used throughout the application.

### Furniture Store Visual Style

- **Typography:** Elegant **Inter** font using weights **400 (Regular)** and **600 (SemiBold)**.
- **Color Palette:** Neutral backgrounds (white, off-white, light gray) combined with a premium deep navy blue primary color (`#1A2B4C`).
- **Spacing:** Consistent whitespace with a minimum vertical spacing of **24px** to create a clean and premium appearance.

---

## 📄 5. Page-Specific UI/UX Specifications

### 🏡 1. Home Page

- **Header:** Displays the minimalist **Ruqi** logo, navigation links (Home, Products, Collections, About Us, Contact Us), Search, Shopping Cart, and User Profile/Login.
- **Hero Section:** A full-width high-resolution furniture showcase image with an elegant headline and a prominent **Explore Collection** call-to-action button.
- **Featured Categories:** Responsive grid displaying featured furniture categories using optimized WebP images and subtle hover animations.

---

### 🪑 2. Products Page

- **Page Header:** Elegant page title with a concise subtitle introducing the product catalog.
- **Filter Bar:** Horizontal filter and sorting controls (All, Chairs, Tables, Sofas, Lighting).
- **Product Cards:** Each card contains:
  - Large product image with subtle hover zoom.
  - Product name.
  - Collection or material description.
  - Product price.
  - Quick Add to Cart and Wishlist buttons.

---

### 🛒 3. Collections & Cart Page

- **Page Title:** *Your Curated Collection*.
- **Cart Layout:** Each cart row includes:
  - Product thumbnail.
  - Product name and material details.
  - Quantity selector.
  - Remove button.
- **Order Summary Sidebar:**
  - Subtotal.
  - Estimated Tax.
  - Total Price.
  - Product warranty information.
  - Continue Shopping link.
  - **Proceed to Checkout** button.

---

### 📜 4. About Us Page

- **Hero Section:** Full-width premium furniture showcase image.
- **Brand Story:** Centered paragraph describing Ruqi's commitment to quality craftsmanship and premium materials.
- **Core Values:** Three-column section highlighting:
  - Sustainable Sourcing
  - Master Craftsmanship
  - Timeless Design
- **Process Gallery:** Masonry-style gallery displaying workshop images, wood textures, and handcrafted furniture production.

---

### 📞 5. Contact Us Page

- **Page Header:** *Contact Us – Let's Design Your Space*.
- **Two-Column Layout:**
  - **Left Column:** Showroom image, business address, opening hours (**Monday–Friday: 10:00–18:00**) and a **Get Directions** link.
  - **Right Column:** Contact form including Name, Email, and Message fields with accessible focus styles.
- **Footer Contact Section:** Dark promotional banner containing links to WhatsApp, Instagram, Facebook, and Email.

---

## 🎨 6. Additional UI/UX Standards

### Empty States
- Friendly illustrations and messages are displayed when no products, orders, or search results are available.

### Loading Experience
- Skeleton loaders are displayed while products and categories are loading.

### Image Optimization
- Images use lazy loading and modern formats (WebP) to improve performance.

### Breadcrumb Navigation
- Breadcrumbs are displayed on all internal pages to improve navigation and usability.

### Animations
- UI animations use subtle transitions between **150ms and 300ms** to provide smooth user interactions without affecting performance.

### Reusable Components
The application follows a reusable component approach for:
- Buttons
- Product Cards
- Forms
- Modal Dialogs
- Toast Notifications
- Navigation Menus

This ensures visual consistency and simplifies future maintenance.
