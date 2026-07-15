# 13. Appendices & Supplementary Materials

## 📌 Overview
This appendix contains supplementary reference materials, a comprehensive glossary of technical terms, and an overview of the technology stack utilized across the **Ruqi Store** development lifecycle. These resources provide extra clarity for developers, designers, and project stakeholders.

---

## 📚 1. Glossary of Terms

| Term | Definition |
| :--- | :--- |
| **Domain Model** | A visual and conceptual representation of real-world entities, their attributes, and relationships operating within the system. |
| **SKU (Stock Keeping Unit)** | A unique alphanumeric identifier assigned to each distinct furniture item to track catalog inventory. |
| **DTO (Data Transfer Object)** | A lightweight, flat class designed specifically to pass data securely between architectural layers without exposing database entity configurations. |
| **Price Snapshot** | A persistent copy of a product's price recorded inside the `OrderItem` table at the precise millisecond of checkout, isolating historical invoices from future catalog price updates. |
| **Cascade Delete** | A database constraint configuration where deleting a parent row (e.g., an `Order`) automatically triggers the deletion of all its child rows (e.g., associated `OrderItems`). |
| **Mermaid.js** | A JavaScript-based diagramming tool that parses markdown-like text definitions to dynamically render diagrams and charts directly in web browsers (and native GitHub markdown files). |
| **WCAG 2.1 AA** | Web Content Accessibility Guidelines version 2.1 at level AA, defining standard benchmarks to ensure websites are fully accessible to individuals with diverse visual and physical abilities. |
| **RTL (Right-to-Left)** | Page layout direction tailored for languages like Arabic, requiring text alignment and structural elements to flip horizontally. |

---

## 💻 2. Technology Stack & Developer Tools

The following technologies are specified for the development, layout, and operation of the **Ruqi Store** ecosystem:

### Backend Development
* **Platform:** .NET Core (C#)
* **Framework:** ASP.NET Core MVC (Model-View-Controller)
* **Database Access:** Entity Framework Core (EF Core) using the Repository & Unit of Work patterns
* **Database Engine:** Microsoft SQL Server

### Frontend & UI/UX Design
* **Styling Framework:** Bootstrap 5 (utilizing `bootstrap.rtl.min.css` for Arabic localized browsing)
* **Client-side Scripting:** jQuery & jQuery Validation Unobtrusive
* **Icons:** FontAwesome or Bootstrap Icons
* **Primary Fonts:** `Inter` (sans-serif)

### Documentation & Diagramming
* **Formatting:** GitHub Flavored Markdown (GFM)
* **Diagrams:** Mermaid.js (Sequence, ERD, and Dependency Flow Charts)

---

## 🔗 3. References & Guidelines

* **WCAG 2.1 Accessibility Checklist:** [W3C Web Accessibility Guidelines](https://www.w3.org/WAI/standards-guidelines/wcag/)
* **Bootstrap 5 RTL Documentation:** [Bootstrap RTL Integration Guide](https://getbootstrap.com/docs/5.0/getting-started/rtl/)
* **Mermaid Diagram Syntax Reference:** [Mermaid.js official documentation](https://mermaid.js.org/)
