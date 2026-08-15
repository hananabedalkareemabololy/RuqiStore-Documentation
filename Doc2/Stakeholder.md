# 02. Stakeholder & User Role Analysis

## 2.1 Stakeholder

The **Ruqi Store** system has four official user roles. Each role has clearly defined responsibilities, permissions, interests, and concerns within the system.

| # | Stakeholder | Role | Interest | Influence | Key Concern |
| :---: | :--- | :--- | :--- | :---: | :--- |
| **1** | **Customer** | End User | Purchasing furniture online | High | Easy product discovery, reliable checkout, accurate stock, and clear order tracking |
| **2** | **Store Manager** | Store Operations | Product, inventory, and order management | High | Accurate inventory, efficient product management, and smooth order fulfillment |
| **3** | **Payment Officer** | Payment Management | Payment verification and order payment status | High | Accurate payment confirmation, rejection handling, and reliable payment records |
| **4** | **Administrator** | System Administration | Complete system oversight | High | User security, role management, review moderation, auditability, and system reports |

---

# 2.2 Stakeholder Map

This quadrant chart visualizes the four official Ruqi Store roles based on their level of interest and influence over the platform. It helps prioritize their requirements during system analysis and development.

```mermaid
quadrantChart
    title Stakeholder Influence vs. Interest

    x-axis Low Interest --> High Interest
    y-axis Low Influence --> High Influence

    quadrant-1 Manage Closely
    quadrant-2 Keep Satisfied
    quadrant-3 Monitor
    quadrant-4 Keep Informed

    "Administrator": [0.90, 0.95]
    "Store Manager": [0.90, 0.85]
    "Payment Officer": [0.75, 0.80]
    "Customer": [0.95, 0.55]
