# 🛒 Supermarket POS System

## Course

**Cap1: System Analysis and Design**

---

# 👥 Team Information

**Team Name:** Prime Logic Inc.

| Name       | Group | Student ID | Role              |
| ---------- | ----- | ---------- | ----------------- |
| Bobur      | I24A  | 202490377  | Leader            |
| Azizjon    | I24A  | 202490017  | Project Manager   |
| Ulug’bek   | I24A  | 202490206  | Designer          |
| Erkin      | I24A  | 202490286  | Software Engineer |
| Kamalatdin | I24A  | 202490284  | Software Engineer |
| Nusrat     | I24B  | 202490199  | Software Engineer |

---

# 📌 Project Description

This project develops a **database-driven Supermarket Point-of-Sale (POS) System** using **SQL**.

The system records and manages payment information when customers purchase products through POS terminals.

The database stores key information including:

* Customer identity
* Purchased products and quantities
* Total transaction amount
* Payment method
* POS terminal location
* Transaction date and time

Each transaction is initially recorded with a **Pending** status and later updated to **Confirmed** or **Failed** depending on the payment result.

### Main Database Tables

* Customers
* Products
* POS Terminals
* Transactions
* Transaction Items

The system demonstrates how businesses:

* Track sales
* Confirm payments
* Maintain reliable transaction records
* Generate reports for business analysis

---

# ⚠️ Problem Statement

Businesses require a reliable system to record and verify customer payments processed through POS terminals.

Without a structured system:

* Transaction data may be lost
* Records may become inconsistent
* Payment verification becomes difficult
* Financial reporting may contain errors

### Our Solution

This project implements a **structured SQL database** that:

* Securely stores transaction information
* Tracks payment status (**Pending / Confirmed / Failed**)
* Maintains accurate records of **who purchased what, when, and where**

---

# 🎯 Project Objectives

* Design a structured **SQL database** for transaction management
* Record **customer, product, POS, and time information**
* Implement **payment status tracking**
* Ensure **data integrity** using relationships and constraints
* Enable easy **data retrieval for monitoring and reporting**

---

# 📦 System Scope

The system manages POS transaction data including:

* Customer information
* Product details
* POS terminal data
* Transaction records
* Purchased items
* Payment method
* Transaction status
* Timestamp

### Limitations

* No graphical user interface
* No real payment gateway integration
* No inventory management system
* No authentication module

⚠️ **This project is designed for educational purposes only.**

---

# 👤 Expected Users

| User                  | Purpose                              |
| --------------------- | ------------------------------------ |
| Store Cashiers        | Record sales transactions            |
| Store Managers        | Review sales and transaction history |
| Business Owners       | Analyze revenue and performance      |
| System Administrators | Maintain the database                |

---

# 📅 Project Timeline

| Week                     | Activities                    |
| ------------------------ | ----------------------------- |
| Week 1 (06 Feb – 13 Feb) | Project Planning              |
| Week 2 (13 Feb – 20 Feb) | BPM & Use Case                |
| Week 3 (20 Feb – 27 Feb) | BPM & Use Case                |
| Week 4 (27 Feb – 06 Mar) | ERD Design                    |
| Week 5 (06 Mar – 13 Mar) | Report Writing & Presentation |

---

# 🏆 Project Outcomes

Through this project we:

* Learned **relational database design**
* Gained practical experience using **SQL**

  * `CREATE`
  * `INSERT`
  * `UPDATE`
  * `SELECT`
* Understood **transaction management**
* Practiced **table relationship design**
* Studied **real-world payment and sales workflows**
* Improved **system design and problem-solving skills**

---

# 📚 Conclusion

This project provided a comprehensive introduction to **relational database management systems**.

The development process included:

* Configuring a **local SQL environment**
* Using a collaborative **AI-assisted development workflow**
* Designing database schemas and relationships
* Creating professional system documentation

Technical documentation was created using **draw.io (app.diagrams.net)** including:

* **Use Case Diagrams**
* **Business Process Models (BPM)**
* **Entity-Relationship Diagrams (ERD)**

These tools helped build a strong understanding of both:

* **Backend database implementation**
* **Structured system architecture**

---

# 📖 References

### draw.io Tutorials

* [https://youtu.be/1T4igDVMVEg](https://youtu.be/1T4igDVMVEg)
* [https://youtu.be/_MzTijsoSe8](https://youtu.be/_MzTijsoSe8)
* [https://youtu.be/JYZPdU5F2iM](https://youtu.be/JYZPdU5F2iM)

### MySQL Installation Tutorial

* [https://youtu.be/hiS_mWZmmI0](https://youtu.be/hiS_mWZmmI0)

---

# 📎 Appendix

## Use Case Diagram

*(Insert diagram image here)*

```
/docs/use-case-diagram.png
```

---

## Business Process Model (BPM)

*(Insert diagram image here)*

```
/docs/business-process-model.png
```

---

## Entity Relationship Diagram (ERD)

*(Insert diagram image here)*

```
/docs/erd-diagram.png
```

---

# 🗄 SQL Database Schema

```sql
-- =========================================
-- CREATE DATABASE
-- =========================================

CREATE DATABASE Prime_Logic;

USE Prime_Logic;

-- =========================================
-- 1. INDEPENDENT TABLES
-- =========================================

CREATE TABLE Employee (
    Employee_ID INT AUTO_INCREMENT PRIMARY KEY,
    First_Name VARCHAR(50) NOT NULL,
    Last_Name VARCHAR(50) NOT NULL,
    Role ENUM('Cashier','Manager','Admin') NOT NULL,
    Passcode_Hash VARCHAR(255) NOT NULL
);

CREATE TABLE Customer (
    Customer_ID INT AUTO_INCREMENT PRIMARY KEY,
    Phone_Number VARCHAR(15) UNIQUE,
    Discount_Tier DECIMAL(3,2) NOT NULL,
    Total_Spent DECIMAL(10,2) DEFAULT 0.00
);

CREATE TABLE Supplier (
    Supplier_ID INT AUTO_INCREMENT PRIMARY KEY,
    Company_Name VARCHAR(100) NOT NULL,
    Contact_Email VARCHAR(100) NOT NULL,
    Lead_Time_Days INT NOT NULL CHECK (Lead_Time_Days >= 0)
);

CREATE TABLE Product (
    Product_ID INT AUTO_INCREMENT PRIMARY KEY,
    Barcode VARCHAR(50) UNIQUE NOT NULL,
    Product_Name VARCHAR(100) NOT NULL,
    Base_Price DECIMAL(10,2) NOT NULL CHECK (Base_Price >= 0),
    Current_Stock INT NOT NULL CHECK (Current_Stock >= 0),
    Min_Threshold INT NOT NULL CHECK (Min_Threshold >= 0)
);

-- =========================================
-- 2. DEPENDENT TABLES
-- =========================================

CREATE TABLE Sale (
    Sale_ID INT AUTO_INCREMENT PRIMARY KEY,
    Timestamp DATETIME DEFAULT CURRENT_TIMESTAMP,
    Employee_ID INT NOT NULL,
    Customer_ID INT,
    Total_Amount DECIMAL(10,2) NOT NULL CHECK (Total_Amount >= 0),
    Discount_Value DECIMAL(10,2) DEFAULT 0.00,

    FOREIGN KEY (Employee_ID)
        REFERENCES Employee(Employee_ID)
        ON DELETE RESTRICT
        ON UPDATE CASCADE,

    FOREIGN KEY (Customer_ID)
        REFERENCES Customer(Customer_ID)
        ON DELETE SET NULL
        ON UPDATE CASCADE
);

CREATE TABLE Purchase_Order (
    PO_ID INT AUTO_INCREMENT PRIMARY KEY,
    Created_At DATETIME DEFAULT CURRENT_TIMESTAMP,
    Employee_ID INT NOT NULL,
    Supplier_ID INT NOT NULL,
    Status ENUM('Pending','Ordered','Received','Cancelled') NOT NULL,

    FOREIGN KEY (Employee_ID)
        REFERENCES Employee(Employee_ID)
        ON DELETE RESTRICT
        ON UPDATE CASCADE,

    FOREIGN KEY (Supplier_ID)
        REFERENCES Supplier(Supplier_ID)
        ON DELETE RESTRICT
        ON UPDATE CASCADE
);

-- =========================================
-- 3. ASSOCIATIVE TABLES
-- =========================================

CREATE TABLE Sale_Item (
    Sale_ID INT,
    Product_ID INT,
    Quantity INT NOT NULL CHECK (Quantity > 0),
    Unit_Price DECIMAL(10,2) NOT NULL CHECK (Unit_Price >= 0),

    PRIMARY KEY (Sale_ID, Product_ID),

    FOREIGN KEY (Sale_ID)
        REFERENCES Sale(Sale_ID)
        ON DELETE CASCADE
        ON UPDATE CASCADE,

    FOREIGN KEY (Product_ID)
        REFERENCES Product(Product_ID)
        ON DELETE RESTRICT
        ON UPDATE CASCADE
);

CREATE TABLE Purchase_Order_Item (
    PO_ID INT,
    Product_ID INT,
    Quantity_Req INT NOT NULL CHECK (Quantity_Req > 0),
    Unit_Cost DECIMAL(10,2) NOT NULL CHECK (Unit_Cost >= 0),

    PRIMARY KEY (PO_ID, Product_ID),

    FOREIGN KEY (PO_ID)
        REFERENCES Purchase_Order(PO_ID)
        ON DELETE CASCADE
        ON UPDATE CASCADE,

    FOREIGN KEY (Product_ID)
        REFERENCES Product(Product_ID)
        ON DELETE RESTRICT
        ON UPDATE CASCADE
);
```

---
