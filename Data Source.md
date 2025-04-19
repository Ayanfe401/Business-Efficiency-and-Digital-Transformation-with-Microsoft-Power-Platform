
# Data Source Creation for Omega Manufacturing’s Order Processing System

## Table of Contents  
1. [Objective](#1️⃣-objective)  
2. [Table/List Design](#2️⃣-tablelist-design)  
   - [2.1 Customers Table](#21-customers-table)  
   - [2.2 Products Table](#2.2-products-table)  
   - [2.3 Orders Table](#2.3-orders-table)  
   - [2.4 Order Items Table](#2.4-order-items-table)  
   - [2.5 Inventory Table](#2.5-inventory-table)  
   - [2.6 Approvals Table](#2.6-approvals-table)  
3. [Relationships Between Tables](#3️⃣-relationships-between-tables)  
4. [Dataverse Platform Justification](#4️⃣-dataverse-platform-justification)  
5. [Conclusion](#5️⃣-conclusion)

---

## 1️⃣ Objective  

Create a structured and relational data source in **Microsoft Dataverse** that:

- Unifies customer, product, inventory, and order data  
- Enables automation of workflows (e.g., order approvals)  
- Ensures traceability, accuracy, and data compliance  
- Provides a foundation for building apps, flows, and reports  

---

## 2️⃣ Table/List Design  

Below is the proposed table design including columns and data types, purpose-built for Microsoft Dataverse.

---

### 2.1 Customers Table  

| Column Name  | Data Type                  | Description                      |
|--------------|----------------------------|----------------------------------|
| CustomerID   | Autonumber (Primary Key)   | Unique identifier                |
| FirstName    | Text                       | Customer's first name            |
| LastName     | Text                       | Customer's last name             |
| Email        | Email                      | Email address                    |
| Phone        | Phone                      | Contact number                   |
| Address      | Text Area                  | Full street address              |
| City         | Text                       | City of residence                |
| State        | Text                       | State/Province                   |
| PostalCode   | Text                       | ZIP/Postal code                  |
| Country      | Text                       | Country                          |

---

### 📦 2.2 Products Table  

| Column Name   | Data Type                  | Description                      |
|---------------|----------------------------|----------------------------------|
| ProductID     | Autonumber (Primary Key)   | Unique product identifier        |
| ProductName   | Text                       | Name of the product              |
| Description   | Text Area                  | Detailed description             |
| UnitPrice     | Currency                   | Price per unit                   |
| Category      | Choice                     | Product type/category            |

---

### 📃 2.3 Orders Table  

| Column Name   | Data Type                  | Description                      |
|---------------|----------------------------|----------------------------------|
| OrderID       | Autonumber (Primary Key)   | Unique order number              |
| Customer      | Lookup (to Customers)      | Customer who placed the order    |
| OrderDate     | Date/Time                  | Date order was created           |
| OrderStatus   | Choice                     | Status (Pending, Approved, etc.) |
| OrderTotal    | Currency (Calculated)      | Total order value                |

---

### 🧾 2.4 Order Items Table  

| Column Name     | Data Type                  | Description                          |
|------------------|----------------------------|--------------------------------------|
| OrderItemID      | Autonumber (Primary Key)   | Unique ID for each line item         |
| Order            | Lookup (to Orders)         | Associated order                     |
| Product          | Lookup (to Products)       | Product in the order                 |
| Quantity         | Whole Number               | Number of units                      |
| UnitPrice        | Currency                   | Price per unit at time of order      |
| ExtendedPrice    | Currency (Calculated)      | Quantity × UnitPrice                 |

---

### 🏪 2.5 Inventory Table  

| Column Name       | Data Type                  | Description                        |
|--------------------|----------------------------|------------------------------------|
| InventoryID        | Autonumber (Primary Key)   | Unique stock record                |
| Product            | Lookup (to Products)       | Product being tracked              |
| StockLevel         | Whole Number               | Current inventory count            |
| LastUpdated        | Date/Time                  | Timestamp of last update           |
| WarehouseLocation  | Text                       | Physical storage location          |

---

### ✅ 2.6 Approvals Table  

| Column Name       | Data Type                  | Description                          |
|--------------------|----------------------------|--------------------------------------|
| ApprovalID         | Autonumber (Primary Key)   | Unique ID per approval action        |
| Order              | Lookup (to Orders)         | Order requiring approval             |
| Approver           | Lookup (System User)       | Approver's system identity           |
| ApprovalStatus     | Choice                     | Status (Approved, Rejected, Pending) |
| ApprovalDateTime   | Date/Time                  | Time of decision                     |
| Comments           | Text Area                  | Notes from approver                  |

---

## 3️⃣ Relationships Between Tables  

| Primary Table | Related Table   | Relationship Type | Description                                      |
|---------------|------------------|-------------------|--------------------------------------------------|
| Customers     | Orders            | One-to-Many       | One customer can place many orders               |
| Orders        | Order Items       | One-to-Many       | One order can contain multiple products          |
| Products      | Order Items       | One-to-Many       | One product can appear in multiple order lines   |
| Products      | Inventory         | One-to-One        | One inventory record per product                 |
| Orders        | Approvals         | One-to-Many       | Orders may require multiple approvals            |

All relationships are implemented using **Lookup columns in Dataverse**, maintaining data consistency and supporting rich filtering, rollups, and automation.

---

## 4️⃣ Dataverse Platform Justification  

Microsoft Dataverse was selected due to:

| Need                          | Dataverse Capability                                |
|-------------------------------|-----------------------------------------------------|
| Secure and centralized data   | Role-based access, encryption at rest and transit   |
| Rapid app development         | Native support in Power Apps and Power Automate     |
| Strong relationships          | Built-in referential integrity and lookups          |
| Audit trail & compliance      | Native activity logging, field security             |
| Integration potential         | Works with Power BI, AI Builder, external systems   |

---

## 5️⃣ Conclusion  

By designing this data source using **Microsoft Dataverse**, Omega Manufacturing achieves:

- A **single source of truth** across orders, inventory, products, and customers  
- **Scalable, secure** data handling with audit trails and approval visibility  
- A foundation for **automated workflows**, reports, and dashboards  
- Preparedness for next-phase digital transformation using Power Apps, Power BI, Power Pages and Power Automation.
