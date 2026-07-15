# 11. UI/UX Design & Frontend Specifications

<p align="center">

<strong>Ruqi Store Premium Furniture E-Commerce</strong>

UI/UX Guidelines, Frontend Architecture & Design System

</p>

---

![RTL Support](https://img.shields.io/badge/RTL-Supported-success)
![WCAG](https://img.shields.io/badge/WCAG-2.1%20AA-blue)
![Responsive](https://img.shields.io/badge/Responsive-Mobile%20First-orange)
![Localization](https://img.shields.io/badge/Localization-AR%20%7C%20EN-purple)

---

# 📑 Table of Contents

- [Overview](#-overview)
- [Frontend Architecture Diagram](#-frontend-architecture-diagram)
- [Globalization & Localization](#-1-globalization--localization-rtl--ltr)
- [Responsive Design Grid System](#-2-responsive-design-grid-system)
- [Feedback Validation & Error Messaging](#-3-feedback-validation--error-messaging)
- [Accessibility & Visual Design](#-4-accessibility--visual-design)
- [Page-Specific UI/UX Specifications](#-5-page-specific-uiux-specifications)
- [Additional UI/UX Standards](#-6-additional-uiux-standards)
- [Design Tokens](#-design-tokens)
- [Reusable Components](#-reusable-components)
- [Performance Optimization](#-performance-optimization)
- [Browser Compatibility](#-browser-compatibility)


---

# 📌 Overview

This document defines the user interface (UI) specifications, user experience (UX) guidelines, and design system constraints for the **Ruqi Store** web application.

It outlines how the application ensures:

- High accessibility standards.
- Seamless localization support.
- RTL/LTR layout adaptation.
- Responsive layouts.
- Premium furniture e-commerce experience.

The goal is to provide a modern, elegant, and consistent shopping experience across all devices and languages.


---

# 🏗 Frontend Architecture Diagram

```mermaid
graph TD

A[User Interface] --> B[Reusable Components]

B --> C[Navigation Menu]
B --> D[Product Cards]
B --> E[Forms]
B --> F[Toast Notifications]
B --> G[Modal Dialogs]

A --> H[Localization Layer]

H --> I[Arabic RTL]
H --> J[English LTR]

A --> K[Responsive Layout System]

K --> L[Mobile 320-767px]
K --> M[Tablet 768-1023px]
K --> N[Desktop 1024px+]

A --> O[Accessibility Layer]

O --> P[ARIA Support]
O --> Q[Keyboard Navigation]
O --> R[WCAG 2.1 AA]

A --> S[Performance Layer]

S --> T[Lazy Loading]
S --> U[WebP Images]
S --> V[Skeleton Loaders]
# 🌐 1. Globalization & Localization (RTL / LTR)

<details>
<summary>Click to expand localization specifications</summary>

## Overview

Ruqi Store supports bilingual capabilities:

| Language | Direction |
|---|---|
| Arabic | RTL |
| English | LTR |

The layout dynamically adapts according to the selected language direction using:

```html
dir="rtl"
```

or

```html
dir="ltr"
```

---

## Localization Rules

| Feature | Implementation |
|---|---|
| CSS Framework | Bootstrap 5 RTL stylesheet loads when Arabic is selected |
| Text Alignment | Arabic uses right alignment, English uses left alignment |
| CSS Properties | Uses logical properties instead of physical properties |
| Language Storage | ASP.NET Core localization cookies |
| Cookie Name | `.AspNetCore.Culture` |

---

## CSS Logical Properties

All styles should use:

```css
margin-inline-start
margin-inline-end
padding-inline-start
padding-inline-end
```

instead of:

```css
margin-left
margin-right
padding-left
padding-right
```

This allows automatic adaptation between RTL and LTR layouts.

---

## Language Toggle

A persistent language selector is available inside the main navigation bar.

Selected language preference is stored using ASP.NET Core localization cookies to persist user preferences across sessions.

</details>


---

# 📱 2. Responsive Design Grid System

<details>
<summary>Click to expand responsive specifications</summary>

The interface automatically adapts to different screen sizes to provide an optimal shopping experience.

| Device | Screen Size | Layout | Features |
|---|---|---|---|
| Mobile | 320px - 767px | Single Column | Collapsed navigation, stacked content |
| Tablet | 768px - 1023px | Two Columns | Optimized touch spacing |
| Desktop | 1024px+ | Three/Four Columns | Sidebar navigation and advanced layouts |

---

## Mobile Experience

| Feature | Implementation |
|---|---|
| Layout | Single-column layout |
| Navigation | Accessible hamburger menu |
| Product Lists | Vertical stacking |
| Tables | Horizontal scrolling when required |
| Cart | Mobile optimized structure |

---

## Tablet Experience

| Feature | Implementation |
|---|---|
| Product Grid | Two-column layout |
| Interaction | Touch-friendly spacing |
| Navigation | Optimized tablet menu |

---

## Desktop Experience

| Feature | Implementation |
|---|---|
| Product Grid | Three or four columns depending on width |
| Dashboard | Fixed sidebar navigation |
| Layout | Full premium desktop experience |

</details>


---

# 💬 3. Feedback, Validation & Error Messaging

<details>
<summary>Click to expand validation specifications</summary>

## Form Validation

| Validation Type | Technology | Purpose |
|---|---|---|
| Client Side | ASP.NET Core MVC + jQuery Unobtrusive Validation | Validate before submission |
| Server Side | ModelState | Map server errors to fields |

---

## Error Handling Rules

| Situation | User Experience |
|---|---|
| Validation Error | Display message beside related field |
| Database Error | Hidden from user |
| Unexpected Exception | Redirect to custom error page |

Users should never see raw system exceptions or database errors.

---

## Loading States

During operations such as:

- Login
- Checkout
- Form submission

The system should:

| Action | Behavior |
|---|---|
| Submit Button | Disabled during processing |
| Feedback | Inline loading spinner |
| Protection | Prevent duplicate submissions |

---

## Toast Notifications

AJAX operations such as:

- Adding products to cart
- Adding products to wishlist

Display:

| Type | Position |
|---|---|
| Success Toast | Top-end |
| Error Toast | Top-end |
| Loading Indicator | During request |

</details>

# ♿ 4. Accessibility & Visual Design

<details>
<summary>Click to expand accessibility and visual design specifications</summary>

## WCAG 2.1 AA Compliance

Ruqi Store follows accessibility standards to ensure an inclusive experience for all users.

| Accessibility Feature | Implementation |
|---|---|
| Color Contrast | Minimum contrast ratio of 4.5:1 according to WCAG 2.1 AA |
| Keyboard Navigation | All interactive elements accessible using Tab navigation |
| Focus Indicators | Visible `:focus-visible` states are preserved |
| ARIA Support | Icon-only buttons include `aria-label` attributes |
| Image Accessibility | Furniture images include descriptive `alt` text |
| Semantic HTML | Uses `<header>`, `<main>`, `<nav>`, and `<footer>` elements |

---

## Keyboard Navigation

All interactive components must support:

- Keyboard-only navigation.
- Visible focus states.
- Logical tab order.
- Accessible form controls.

---

## Furniture Store Visual Style

| Design Element | Specification |
|---|---|
| Typography | Elegant Inter font |
| Font Weights | 400 Regular and 600 SemiBold |
| Background Colors | White, off-white, light gray |
| Primary Color | Deep Navy Blue `#1A2B4C` |
| Spacing System | Minimum vertical spacing of 24px |

---

## Visual Design Principles

| Principle | Description |
|---|---|
| Premium Feel | Clean layouts with generous whitespace |
| Consistency | Reusable UI components |
| Clarity | Clear hierarchy and readable content |
| Accessibility | Design usable for all users |

</details>


---

# 📄 5. Page-Specific UI/UX Specifications

<details>
<summary>🏡 Home Page Specifications</summary>

# 🏡 Home Page

## Header

| Element | Description |
|---|---|
| Logo | Minimalist Ruqi logo |
| Navigation | Home, Products, Collections, About Us, Contact Us |
| Search | Product search functionality |
| Cart | Shopping cart access |
| User Profile | Login and account access |

---

## Hero Section

| Element | Description |
|---|---|
| Image | Full-width high-resolution furniture showcase |
| Headline | Elegant brand message |
| CTA Button | Explore Collection |

---

## Featured Categories

| Feature | Description |
|---|---|
| Layout | Responsive category grid |
| Images | Optimized WebP images |
| Interaction | Subtle hover animations |

</details>


---

<details>
<summary>🪑 Products Page Specifications</summary>

# 🪑 Products Page

## Page Header

| Element | Description |
|---|---|
| Title | Elegant page title |
| Subtitle | Short product catalog introduction |

---

## Filter Bar

| Filter | Purpose |
|---|---|
| All | Display all products |
| Chairs | Filter chairs |
| Tables | Filter tables |
| Sofas | Filter sofas |
| Lighting | Filter lighting products |

---

## Product Cards

Each product card contains:

| Component | Description |
|---|---|
| Product Image | Large image with subtle hover zoom |
| Product Name | Furniture product title |
| Collection | Material or collection description |
| Price | Product price |
| Add To Cart | Quick shopping action |
| Wishlist | Save product option |

</details>


---

<details>
<summary>🛒 Collections & Cart Page Specifications</summary>

# 🛒 Collections & Cart Page

## Page Title

**Your Curated Collection**

---

## Cart Layout

| Element | Description |
|---|---|
| Thumbnail | Product preview image |
| Product Name | Furniture name |
| Material Details | Product material information |
| Quantity Selector | Adjust product quantity |
| Remove Button | Remove item from cart |

---

## Order Summary Sidebar

| Section | Description |
|---|---|
| Subtotal | Product subtotal |
| Estimated Tax | Tax calculation |
| Total Price | Final order total |
| Warranty | Product warranty information |
| Continue Shopping | Return to products |
| Checkout Button | Proceed to Checkout |

</details>


---

<details>
<summary>📜 About Us Page Specifications</summary>

# 📜 About Us Page

## Hero Section

| Element | Description |
|---|---|
| Image | Full-width premium furniture showcase |

---

## Brand Story

Centered paragraph describing:

- Ruqi commitment to quality craftsmanship.
- Premium materials.
- Timeless furniture design.

---

## Core Values

| Value | Description |
|---|---|
| Sustainable Sourcing | Responsible material selection |
| Master Craftsmanship | Professional furniture production |
| Timeless Design | Long-lasting aesthetic quality |

---

## Process Gallery

| Feature | Description |
|---|---|
| Style | Masonry gallery layout |
| Content | Workshop images, wood textures, handcrafted production |

</details>


---

<details>
<summary>📞 Contact Us Page Specifications</summary>

# 📞 Contact Us Page

## Page Header

**Contact Us – Let's Design Your Space**

---

## Two Column Layout

| Column | Content |
|---|---|
| Left Column | Showroom image, address, opening hours, directions |
| Right Column | Contact form |

---

## Business Information

| Information | Details |
|---|---|
| Opening Hours | Monday-Friday: 10:00-18:00 |
| Location | Business address |
| Navigation | Get Directions link |

---

## Contact Form

Fields:

| Field | Requirement |
|---|---|
| Name | Required |
| Email | Required |
| Message | Required |

Includes accessible focus styles.

---

## Footer Contact Section

Dark promotional banner containing:

- WhatsApp
- Instagram
- Facebook
- Email

</details>
# 🎨 Design Tokens

<details>
<summary>Click to expand design system specifications</summary>

Design tokens define the main visual rules used throughout the Ruqi Store application to maintain consistency across all pages and components.

| Token | Value |
|---|---|
| Primary Color | `#1A2B4C` |
| Font Family | Inter |
| Font Weight Regular | 400 |
| Font Weight SemiBold | 600 |
| Background Colors | White, Off-white, Light Gray |
| Minimum Spacing | 24px |
| Animation Duration | 150ms - 300ms |
| Image Format | WebP |
| Border Style | Clean and minimal |

---

## Color System

| Category | Usage |
|---|---|
| Primary | Main actions, buttons, branding |
| Neutral | Backgrounds and sections |
| Text | Headings and body content |
| Feedback | Success, warning, and error messages |

---

## Typography System

| Element | Font Weight | Usage |
|---|---|---|
| Headings | 600 | Titles and important sections |
| Body Text | 400 | General content |
| Buttons | 600 | Primary actions |
| Navigation | 400/600 | Menu items |

</details>


---

# 🧩 Reusable Components

<details>
<summary>Click to expand component architecture</summary>

Ruqi Store follows a reusable component approach to ensure visual consistency and simplify future maintenance.

| Component | Usage |
|---|---|
| Button Component | Primary and secondary actions |
| Product Card | Display furniture products |
| Form Components | User input and validation |
| Modal Dialogs | Confirmation and additional information |
| Toast Notifications | Success and error messages |
| Navigation Menu | Global website navigation |
| Search Component | Product searching |
| Breadcrumb Component | Page navigation |

---

## Component Design Principles

| Principle | Description |
|---|---|
| Reusability | Components can be used across multiple pages |
| Consistency | Same design patterns throughout the application |
| Maintainability | Easier updates and future expansion |
| Accessibility | All components follow WCAG guidelines |

</details>


---

# ⚡ Performance Optimization

<details>
<summary>Click to expand performance guidelines</summary>

Ruqi Store applies performance optimization techniques to provide a fast and smooth shopping experience.

| Feature | Implementation |
|---|---|
| Image Optimization | WebP modern image formats |
| Lazy Loading | Images loaded only when needed |
| Loading Experience | Skeleton loaders during loading states |
| Animations | Lightweight transitions |
| Requests | Optimized AJAX operations |
| UI Rendering | Reusable components to reduce duplication |

---

## Image Optimization Rules

| Rule | Description |
|---|---|
| Format | Use WebP whenever possible |
| Loading | Enable lazy loading |
| Accessibility | Provide meaningful `alt` descriptions |
| Quality | Maintain high quality with optimized size |

---

## Loading Experience

When content is loading:

- Display skeleton loaders.
- Prevent layout shifting.
- Provide visual feedback.
- Maintain user interaction clarity.

</details>


---

# 🌍 Browser Compatibility

<details>
<summary>Click to expand browser support</summary>

Ruqi Store supports modern browsers:

| Browser | Support |
|---|---|
| Google Chrome | Latest 2 versions |
| Microsoft Edge | Latest 2 versions |
| Mozilla Firefox | Latest 2 versions |
| Safari | Latest 2 versions |

---

## Responsive Compatibility

The application must provide a consistent experience across:

| Platform | Support |
|---|---|
| Mobile Devices | Fully responsive |
| Tablets | Optimized touch interaction |
| Desktop | Full feature experience |

</details>


---

# 🧭 Navigation & Usability Standards

<details>
<summary>Click to expand navigation standards</summary>

## Breadcrumb Navigation

Breadcrumbs are displayed on all internal pages to improve:

- User orientation.
- Page hierarchy understanding.
- Navigation efficiency.

---

## Empty States

When no content exists:

| Situation | Experience |
|---|---|
| No Products | Friendly illustration and message |
| No Orders | Helpful empty order state |
| No Search Results | Clear explanation and suggestions |

---

## Animation Standards

All UI animations should follow:

| Rule | Specification |
|---|---|
| Duration | 150ms - 300ms |
| Style | Smooth transitions |
| Purpose | Improve interaction feedback |
| Performance | Must not affect application speed |

</details>


---

# ✅ Final Frontend Standards Checklist

| Category | Requirement |
|---|---|
| Localization | Arabic RTL and English LTR support |
| Responsive Design | Mobile, Tablet, Desktop layouts |
| Accessibility | WCAG 2.1 AA compliance |
| Validation | Client-side and server-side validation |
| Error Handling | Friendly user messages |
| Components | Reusable architecture |
| Images | WebP + Lazy Loading |
| Loading | Skeleton loaders |
| Navigation | Breadcrumb support |
| Animations | 150ms - 300ms transitions |
| Design System | Consistent tokens and components |
| Browser Support | Modern browser compatibility |

---

# 🚀 Ruqi Store UI/UX Documentation Complete

This document defines the complete frontend design system, usability standards, accessibility requirements, localization strategy, and responsive behavior for the Ruqi Store premium furniture e-commerce application.

The goal is to deliver a consistent, accessible, and elegant shopping experience across all devices and languages.
