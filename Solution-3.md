---

### Addressing Omega Manufacturing’s Operational Challenges with Microsoft Dataverse

Omega Manufacturing has faced persistent operational issues including:

- Disjointed data spread across spreadsheets and emails, leading to inefficiencies in order processing and inventory control.  
- Manual approval workflows that are slow, untraceable, and prone to human error.  
- Lack of real-time visibility across customer orders, inventory levels, and internal approvals.  
- Growing concern about *data security*, prompting leadership to insist on a secure, compliant platform.

To address these challenges, a solution built on *Power Platform* using *Microsoft Dataverse* was proposed to centralize and secure business data, automate approvals, and enhance process visibility across the organization.

---

### *Creating the Data Source with Microsoft Dataverse*

To operationalize this solution, the following data model is proposed. It is tailored to support the company’s customer management, order tracking, inventory monitoring, and approval workflows—all in one secure platform.

---

#### *1. Customers Table*  
*Purpose*: Central repository for all customer details to eliminate scattered information and support better service and tracking.

| Column Name     | Data Type     |
|-----------------|---------------|
| CustomerID      | Autonumber (Primary Key) |
| FirstName       | Text          |
| LastName        | Text          |
| Email           | Email         |
| Phone           | Phone         |
| Address         | Text Area     |
| City            | Text          |
| State           | Text          |
| PostalCode      | Text          |
| Country         | Text          |

---

#### *2. Products Table*  
*Purpose*: Store and manage consistent product details, eliminating pricing discrepancies and improving order accuracy.

| Column Name     | Data Type     |
|-----------------|---------------|
| ProductID       | Autonumber (Primary Key) |
| ProductName     | Text          |
| Description     | Text Area     |
| UnitPrice       | Currency      |
| Category        | Choice        |

---

#### *3. Orders Table*  
*Purpose*: Capture all customer orders with current statuses for easy monitoring and fulfillment.

| Column Name     | Data Type     |
|-----------------|---------------|
| OrderID         | Autonumber (Primary Key) |
| Customer        | Lookup (to Customers) |
| OrderDate       | Date/Time     |
| OrderStatus     | Choice (Pending, Approved, Shipped, etc.) |
| OrderTotal      | Currency (calculated) |

---

#### 4. Order Items Table  
*Purpose*: Break down orders into individual product lines to support multiple-item purchases and detailed reporting.

| Column Name     | Data Type     |
|-----------------|---------------|
| OrderItemID     | Autonumber (Primary Key) |
| Order           | Lookup (to Orders) |
| Product         | Lookup (to Products) |
| Quantity        | Whole Number  |
| UnitPrice       | Currency      |
| ExtendedPrice   | Currency (calculated: Quantity × UnitPrice) |

---

#### *5. Inventory Table*  
*Purpose*: Provide up-to-date visibility on stock levels to reduce overselling and support procurement planning.

| Column Name     | Data Type     |
|-----------------|---------------|
| InventoryID     | Autonumber (Primary Key) |
| Product         | Lookup (to Products) |
| StockLevel      | Whole Number  |
| LastUpdated     | Date/Time     |
| WarehouseLocation | Text        |

---

#### *6. Approvals Table*  
*Purpose*: Manage and track internal approvals for customer orders, ensuring accountability and compliance.

| Column Name     | Data Type     |
|-----------------|---------------|
| ApprovalID      | Autonumber (Primary Key) |
| Order           | Lookup (to Orders) |
| Approver        | Lookup (System User) |
| ApprovalStatus  | Choice (Pending, Approved, Rejected) |
| ApprovalDateTime| Date/Time     |
| Comments        | Text Area     |

---

### *Relationships Between Tables*

All data is tightly linked through *lookup relationships*, ensuring referential integrity and traceability.

| Primary Table   | Related Table   | Relationship Type | Business Relevance |
|-----------------|------------------|-------------------|---------------------|
| Customers       | Orders            | One-to-Many       | One customer can place multiple orders |
| Orders          | Order Items       | One-to-Many       | Each order can contain multiple products |
| Products        | Order Items       | One-to-Many       | Track which products appear in which orders |
| Products        | Inventory         | One-to-One        | Track stock for each unique product |
| Orders          | Approvals         | One-to-Many       | Enable multiple approvals per order |

---

### *Overview & Final Positioning*

This data model, built securely on Microsoft Dataverse, forms the backbone of Omega Manufacturing’s digital transformation strategy. It:

- Consolidates data into a *single source of truth*, ending the use of spreadsheets and emails for critical workflows.  
- Enables *automated order approvals* and real-time tracking via Power Automate and Power Apps.  
- Enhances decision-making with accurate, centralized inventory and customer data.  
- Meets leadership’s requirement for *enterprise-level security* and *compliance*.  
- Prepares the company for future enhancements such as analytics dashboards in Power BI and chatbot support.

---
