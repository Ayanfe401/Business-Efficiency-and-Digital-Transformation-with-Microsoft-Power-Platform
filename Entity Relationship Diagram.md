---

## Entity Relationship Diagram (ERD) for Omega Manufacturing’s Order Processing System

## Overview


This document outlines the data schema for the Order Processing System at Omega Manufacturing company. The schema defines the core tables (entities), their key fields with data types, and the relationships between them. The design is created to centralize order processing and inventory management. 

---

## 1. Tables (Entities) & Columns 

Below are the tables along with the important fields and their data types. Each table is designed to capture information related to customers, products, orders, inventory, and approval.

### 🔴 1. Customer  
The Customer table stores key information about each client who places an order. Each customer is uniquely identified using CustomerID, ensuring a clear distinction between different clients. Contact details such as FirstName, LastName, Email, Phone, and Address allow easy communication and order tracking. Additional location fields (City, State, Postal Code, and Country) help in organizing deliveries, ensuring accurate shipping details, and supporting regional sales analysis. This structured approach streamlines customer management and improves service efficiency.

| **Columns Name** | **Data Type**         | **Description**                         |
| -------------- | --------------------- | --------------------------------------- |
| CustomerID     | Unique Identifier     | Primary key, unique customer ID         |
| FirstName      | Text                  | Customer's first name                   |
| LastName       | Text                  | Customer's last name                    |
| Email          | Text                  | Customer's email address                |
| Phone          | Text                  | Customer's contact number               |
| Address        | Text                  | Street address                          |
| City           | Text                  | City name                               |
| State          | Text                  | State or region                         |
| Postal Code     | Text                 | Zip or postal code                      |
| Country        | Text                  | Country name                            |

---

### 🟢 2. Product  
The Product table contains details about all available products in the inventory. Each product is uniquely identified by ProductID, ensuring precise tracking. The ProductName and Description fields provide essential details about the product, aiding customers and staff in identifying items. The UnitPrice field determines the cost per unit of the product, which is crucial for pricing calculations in orders. The Category field categorizes products, helping organize items into groups such as Raw Materials or Finished Goods. This structured data allows for effective product management, pricing strategies, and inventory control.

| **Columns Name**   | **Data Type**         | **Description**                                                   |
| ---------------- | --------------------- | ----------------------------------------------------------------- |
| ProductID        | Unique Identifier     | Primary key, unique product ID                                    |
| ProductName      | Text                  | Name of the product                                               |
| Description      | Text                  | A brief description of the product                                |
| UnitPrice        | Currency              | Price per unit                                                    |
| Category         | Choice                | Product category such as Raw Materials or Finished Goods          |

---

### 🔵 3. Customer Order  
The Customer Order table records all orders placed by customers. Each order is uniquely identified using OrderID, which links directly to a specific customer via CustomerID. The OrderDate captures when the order was placed, enabling time-based tracking and analysis. The OrderStatus field helps track order progress, moving through stages like Pending, Shipped, Delivered, or Canceled. The OrderTotal calculates the total price of all items in the order, ensuring accurate billing. This table plays a vital role in tracking sales transactions and managing the order fulfillment process efficiently.

| **Columns Name** | **Data Type**         | **Description**                                       |
| -------------- | --------------------- | ----------------------------------------------------- |
| OrderID        | Unique Identifier     | Primary key, unique order ID                           |
| CustomerID     | Unique Identifier     | Foreign key referencing Customer(CustomerID)         |
| OrderDate      | Date/Time             | Date and time when the order was placed                |
| OrderStatus    | Choice                | Order status (e.g., Pending, Shipped, Delivered, Canceled) |
| OrderTotal     | Currency              | Total amount for the order                             |

---

### 🟡 4. Order Item  
The Order Item table tracks the individual products within each order. Since one order can contain multiple items, each entry is uniquely identified by OrderItemID, with a link to OrderID to associate it with a specific order. The ProductID field identifies the specific product in the order, ensuring accurate inventory deduction and pricing. The Quantity field records how many units of the product were ordered, while the UnitPrice captures the price per unit at the time of order. The ExtendedPrice field calculates the total cost for that product within the order (Quantity × UnitPrice), ensuring precise order billing and financial tracking. This table ensures seamless order processing and inventory updates.

| **Columns Name**   | **Data Type**         | **Description**                                                   |
| ---------------- | --------------------- | ----------------------------------------------------------------- |
| OrderItemID      | Unique Identifier     | Primary key, unique identifier for the order item                 |
| OrderID          | Unique Identifier     | Foreign key referencing Customer Order(OrderID)                   |
| ProductID        | Unique Identifier     | Foreign key referencing Product(ProductID)                        |
| Quantity         | Number                | Quantity of the product ordered                                   |
| UnitPrice        | Currency              | Price per unit at order time                                        |
| ExtendedPrice    | Currency              | Total price (Quantity multiplied by UnitPrice)                      |

---

### 🟣 5. Inventory  
Inventory management ensures that products are available to fulfill customer orders. Each product has an associated StockLevel, which is updated whenever an order is processed. The LastUpdated field keeps track of inventory changes, ensuring real-time stock monitoring. The Location field helps identify where specific products are stored within the warehouse, reducing retrieval time and improving logistics efficiency.

| **Columns Name**      | **Data Type**         | **Description**                                                |
| ------------------- | --------------------- | -------------------------------------------------------------- |
| ProductID           | Unique Identifier     | Primary key and foreign key referencing Product(ProductID)     |
| StockLevel          | Number                | Current available quantity in stock                             |
| LastUpdated         | Date/Time             | Date and time when the stock level was last updated              |
| Location            | Text                  | Physical location of the inventory (if applicable)               |

---

### 🟠 6. Approval Entity
The Approval table ensures that every order undergoes an approval process as part of Omega Manufacturing’s workflow. This entity tracks all necessary information related to approvals, providing oversight and accountability. The table ensures that orders go through an authorization process before fulfillment. When a new order is placed, it is assigned an ApprovalStatus (Pending by default). An authorized individual (ApproverID) reviews the order and either approves or rejects it. The ApprovalDateTime logs the exact time of approval or rejection, while the Comments field allows for additional notes. This structured approach ensures accountability and compliance with business policies.

| **Columns Name**      | **Data Type**         | **Description**                                                     |
| ------------------- | --------------------- | ------------------------------------------------------------------- |
| ApprovalID          | Unique Identifier     | Primary key, unique identifier for the approval process             |
| OrderID             | Unique Identifier     | Foreign key referencing the associated Customer Order (OrderID)     |
| ApproverID          | Unique Identifier     | Unique identifier for the user or manager responsible for approval  |
| ApprovalStatus      | Choice                | Status of the approval (e.g., Pending, Approved, Rejected)          |
| ApprovalDateTime    | Date/Time             | Date and time when the approval decision was made                   |
| Comments            | Text                  | Additional remarks or details provided by the approver              |

---

## 2. Relationships Between Tables

Understanding how these entities are connected is essential for a unified data structure:

| **Relationship**                             | **Description**                                                                              |
| -------------------------------------------- | -------------------------------------------------------------------------------------------- |
| Customer ↔ Customer Order                      | One customer can have many orders. The Customer Order table uses CustomerID to link each order to its customer. |
| Customer Order ↔ Approval                      | Every order requires approval. The Approval table links to the Customer Order table using OrderID to track the approval process. |
| Customer Order ↔ Order Item                    | One order can contain multiple items. Each Order Item is linked to a particular order using OrderID.  |
| Product ↔ Order Item                           | A product can appear in many order items. The Order Item table uses ProductID to reference the corresponding product. |
| Product ↔ Inventory                            | Each product has one corresponding inventory record. The ProductID matches in both tables to ensure accurate stock information. |

---

## 3. Entity Relationship Diagram (ERD)

Entity Relationship Diagram (ERD) for Omega Manufacturing's Order Processing System using straight lines and broken lines to differentiate relationships. Straight lines represent primary relationships (such as one-to-many connections), while broken lines indicate secondary or one-to-one relationships. The diagram below visually represents the relationships among these key entities. Each box represents a core table, and the connecting lines illustrate the relationships between them.

```
                 +---------------------+         +---------------------+         +---------------------+
                 |       🔴 Customer   |         |      🔵 Order       |         |     🟡 OrderItem    |
                 |---------------------|         |---------------------|         |---------------------|
                 | CustomerID (PK)     | 1     N | OrderID (PK)        | 1     N | OrderItemID (PK)    |
                 | FirstName           |────────>| CustomerID (FK)     |────────>| OrderID (FK)        |
                 | LastName            |         | OrderDate           |         | ProductID (FK)      |
                 | Email               |         | OrderStatus         |         | Quantity            |
                 | Phone               |         | OrderTotal          |         | UnitPrice           |
                 | Address             |         +---------------------+         | ExtendedPrice       |
                 +---------------------+                                       +---------------------+
                                                                                
                                                                                
                 +---------------------+                                +---------------------+
                 |      🟢 Product     |                                |     🟣 Inventory    |
                 |---------------------|                                |---------------------|
              N  | ProductID (PK)      |◄───────────── 1               | ProductID (PK,FK)    |
                 | ProductName         |                                | StockLevel          |
                 | Description         |                                | LastUpdated         |
                 | UnitPrice           |                                | Location            |
                 | Category            |                                +---------------------+
                 +---------------------+
                                                        
                                                        
                 +---------------------+                                
                 |      🟠 Approval    |                                
                 |---------------------|                                
                 | ApprovalID (PK)     |                                
                 | OrderID (FK)        |                                
                 | ApproverID          |                                
                 | ApprovalStatus      |                                
                 | ApprovalDateTime    |                                
                 | Comments            |                                
                 +---------------------+                                
```
Below is a detailed explanation of the **arrows** and the **'1'** and **'N'** notations in the Entity Relationship Diagram:

---

### **Explanation of Arrows**
- **Direction of the Arrows:**  
  Arrows indicate the flow of relationships between tables. For instance, the arrow from **Customer** to **Order** shows that one customer can have multiple orders. Similarly, the arrow from **Order** to **OrderItem** shows that each order can include multiple items.

- **Types of Arrows:**  
  - **Straight Lines:** Represent primary, one-to-many relationships where one entity connects to multiple related entries in another table.  
  - **Broken Lines or Secondary Links:** Often indicate one-to-one or optional relationships between entities (e.g., between **Product** and **Inventory**).

---

### **Explanation of '1' and 'N'**
- **'1':** Represents the "one" side of the relationship:
  - In **Customer ↔ Order**, the "1" next to **Customer** means that a single customer can place one or more orders.
  - In **Product ↔ Inventory**, the "1" next to **Product** indicates that a product has exactly one inventory record.

- **'N':** Represents the "many" side of the relationship:
  - In **Customer ↔ Order**, the "N" next to **Order** indicates that a customer can place multiple (many) orders.
  - In **Order ↔ OrderItem**, the "N" next to **OrderItem** shows that an order can contain multiple items.

---

### **Detailed Breakdown of Relationships**
1. **🔴 Customer ↔ 🔵 Order (1:N):**
   - **Meaning:** One customer can place many orders.
   - **Arrow Direction:** From **Customer** to **Order**.
   - **Key Field Connection:** `CustomerID` is the primary key in the **Customer** table and a foreign key in the **Order** table, linking the two.

2. **🔵 Order ↔ 🟡 OrderItem (1:N):**
   - **Meaning:** One order can include multiple order items.
   - **Arrow Direction:** From **Order** to **OrderItem**.
   - **Key Field Connection:** `OrderID` is the primary key in the **Order** table and a foreign key in the **OrderItem** table.

3. **🟢 Product ↔ 🟡 OrderItem (1:N):**
   - **Meaning:** A product can appear in many order items across different orders.
   - **Arrow Direction:** From **Product** to **OrderItem**.
   - **Key Field Connection:** `ProductID` is the primary key in the **Product** table and a foreign key in the **OrderItem** table.

4. **🟢 Product ↔ 🟣 Inventory (1:1):**
   - **Meaning:** Each product has exactly one associated inventory record.
   - **Arrow Direction:** From **Product** to **Inventory** (with a 1:1 relationship represented by "1" on both ends).
   - **Key Field Connection:** `ProductID` acts as both the primary key in the **Product** table and the foreign key in the **Inventory** table.

5. **🔵 Order ↔ 🟠 Approval (1:N):**
   - **Meaning:** Each order requires one or more approvals.
   - **Arrow Direction:** From **Order** to **Approval**.
   - **Key Field Connection:** `OrderID` is the primary key in the **Order** table and a foreign key in the **Approval** table.

---

##  Conclusion

This well-structured Dataverse schema for the Inventory Management System provides a robust foundation for automating Omega Manufacturing’s order processing and inventory tracking. By centralizing all critical data into one secure repository, this design ensures accuracy, eliminates manual errors, and allows for real‑time reporting and analytics. The relationships between the Customer, Product, Customer Order, Order Item, Inventory, and approval tables ensure seamless data flow.

## Dataverse Entity Relationship Diagram for Omega Manufacturing

Click [here](https://1drv.ms/p/c/03cfc99376a3ee7d/EeeqNxCmhqhLtQtdO7Jz3k8BwaDkdh4wa9XJbeOO9zQDPw) to view my Entity Relationship Diagram presentation.  


*Presented by:* Ayanfe Moses Asulewon
