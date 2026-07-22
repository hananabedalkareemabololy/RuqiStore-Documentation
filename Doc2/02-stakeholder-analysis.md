# 02. Stakeholder & User Role Analysis

## 2.1 Stakeholder Register

| # | Stakeholder | Role | Interest | Influence | Key Concern |
| :---: | :--- | :--- | :--- | :---: | :--- |
| **1** | **Store Owner** | Business Owner | Business growth, profitability, and operational improvement | High | Accurate inventory information, efficient operations, and customer satisfaction |
| **2** | **Store Manager** | System Manager | Daily store operations and product management | High | Easy product updates, order management, and inventory control |
| **3** | **Customer** | End User | Purchasing furniture online | High | Easy browsing, secure checkout, wishlist management, and order tracking |
| **4** | **Administrator** | System Administrator | System security and access management | High | User permissions, system reliability, and data protection |

---

# 2.2 Stakeholder Map

This quadrant chart visualizes stakeholders according to their influence and interest in the **Ruqi Store** platform.

```mermaid
quadrantChart
    title Stakeholder Influence vs. Interest

    x-axis Low Interest --> High Interest
    y-axis Low Influence --> High Influence

    quadrant-1 Manage Closely
    quadrant-2 Keep Satisfied
    quadrant-3 Monitor
    quadrant-4 Keep Informed

    "Store Owner": [0.85, 0.90]
    "Store Manager": [0.80, 0.85]
    "Administrator": [0.70, 0.80]
    "Customer": [0.85, 0.55]
