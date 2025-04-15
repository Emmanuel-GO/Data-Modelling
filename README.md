# Data-Modelling

# Invoice-Based Dimensional Modelling 

## Overview

In many real-world scenarios, business requirements may be inferred directly from the available data. In this assignment, you will work with a sample invoice dataset and reverse-engineer the essential business dimensions. Your task is to design an end-to-end dimensional model that captures the critical information required for business analysis and reporting.

Your solution should support key analytical capabilities such as drill-down reporting and historical analysis, while ensuring data quality and scalability. You will need to infer dimensions such as Customer, Product, Time, and Store from the invoice data provided.

---

## Business Scenario

Imagine you are working for a mid-sized retail company that issues invoices for every sale. Although explicit business requirements have not been documented, you have access to the following invoice data. Your goal is to design a dimensional model that can power a robust analytics platform, enabling insights into customer behavior, product performance, and store-level sales.

### Business Objectives (Inferred from Data)

- **Analytical Reporting:** Provide detailed analysis on sales trends, customer purchasing patterns, and product performance.
- **Drill-Down Capability:** Allow multi-level analysis (e.g., overall sales → sales by store, by product category, by customer segments).
- **Historical Tracking:** Maintain a record of past transactions to support trend analysis and forecast planning.
- **Scalability & Performance:** Ensure the model can scale with data growth and support efficient querying.

## Sample Invoice Data
## 🧾 Sales Invoice Table

| Invoice_ID | Invoice_Date         | Customer_ID | Customer_Name | Product_ID | Product_Name        | Quantity | Unit_Price | Line_Total | Store_ID |
|------------|----------------------|-------------|----------------|------------|----------------------|----------|------------|------------|----------|
| INV001     | 2025-03-15 09:15:00  | C001        | John Smith     | P1001      | Wireless Mouse       | 2        | 25.00      | 50.00      | S01      |
| INV001     | 2025-03-15 09:15:00  | C001        | John Smith     | P1002      | Mechanical Keyboard  | 1        | 85.00      | 85.00      | S01      |
| INV002     | 2025-03-15 10:05:00  | C002        | Jane Doe       | P1003      | HD Monitor           | 1        | 200.00     | 200.00     | S02      |
| INV003     | 2025-03-15 11:30:00  | C003        | Bob Johnson    | P1001      | Wireless Mouse       | 1        | 25.00      | 25.00      | S01      |
| INV003     | 2025-03-15 11:30:00  | C003        | Bob Johnson    | P1004      | USB-C Hub            | 2        | 30.00      | 60.00      | S01      |

## Requirements

### 1. Data Profiling & Inference

- **Identify Dimensions:** Analyze the invoice dataset and determine which attributes should be modelled as dimensions.
- **Determine Fact Table Granularity:** Establish the grain of your fact table based on the invoice line items. Justify your decisions in terms of the types of analysis the business might require.

### 2. Dimensional Modeling Strategy

- **Star Schema Design:** Develop a star schema that integrates your fact table with conformed dimensions.
- **Handling Degenerate Dimensions:** Identify any degenerate dimensions and describe how they will be managed within your model.

### 3. Slowly Changing Dimensions (SCD)

- **SCD Implementation:** Describe how you would handle slowly changing dimensions for attributes that may change over time. Discuss the approach for implementing SCD types (Type 1, Type 2, or Type 3) and the trade-offs involved.

### 4. ETL/ELT Strategy

- **Data Integration:** Outline your strategy for ingesting the invoice data into your dimensional model. Consider both batch processing and the possibility of integrating incremental data updates.
- **Data Quality & Consistency:** Explain how you will ensure data quality, consistency, and timely updates across your fact and dimension tables.

### 5. Advanced Analysis & Scalability

- **Hierarchical Relationships:** Discuss how you might design and incorporate multi-level hierarchies to support detailed drill-down analysis.
- **Performance Optimization:** Recommend strategies such as partitioning, indexing, and surrogate key generation to optimize query performance as data volume grows.

## 📊 Data Profiling & Inference

### 🔎 Identified Dimensions and Fact Table Attributes

---

### 1. 🧍 Customer Dimension (`Dim_Customer`)
- **Attributes**: 
  - Customer ID
  - Name
  - Email
  - Segment (VIP, Regular)
  - Address
  - Effective & Expiry Dates *(SCD Type 2)*
- **Relationship**:  
  `Fact_Sales_Inv.Customer_SK` → `Dim_Customer.Customer_SK`

---

### 2. 📦 Product Dimension (`Dim_Product`)
- **Attributes**:
  - Product ID
  - Name
  - Category
  - Unit Price
  - Discounted Price
  - Effective & Expiry Dates
- **Relationship**:  
  `Fact_Sales_Inv.Product_SK` → `Dim_Product.Product_SK`

---

### 3. 🏬 Store Dimension (`Dim_Store`)
- **Attributes**:
  - Store ID
  - Name
  - City
  - State
  - Region
- **Relationship**:  
  `Fact_Sales_Inv.Store_SK` → `Dim_Store.Store_SK`

---

### 4. 🗓️ Time Dimension (`Dim_Time`)
- **Attributes**:
  - Date
  - Day of Week
  - Month
  - Year
  - Quarter
- **Relationship**:  
  `Fact_Sales_Inv.Invoice_Date` → `Dim_Time.Date`

---

### 5. 📈 Fact Table (`Fact_Sales_Inv`)
- **Attributes**:
  - Invoice ID
  - Customer_SK
  - Product_SK
  - Store_SK
  - Invoice_Date
  - Quantity
  - Unit Price
  - Discounted Price
  - Line Total
**Granularity**: One row per invoice line item.
**Relationships**: Links to Customer, Product, Store, and Time dimensions.

## 🧮 Determining Fact Table Granularity

---

### 📌 Granularity Definition

Each row in the `Fact_Sales` table represents a **single line item of an invoice**, identified by the combination of:
- `Invoice_ID`
- `Product_SK`

---

###  Justification for This Granularity

#### 1. Detailed Transactional Reporting
- Captures **individual products purchased** within each invoice.
- Allows tracking of:
  - Discounts
  - Unit Prices
  - Quantities  
  ...at the **most atomic level**.

#### 2.  Supports Aggregated Reports
- Can be **aggregated** to show total sales:
  - Per Customer
  - Per Store
  - Over Time
- Enables reporting at:
  - Daily
  - Monthly
  - Quarterly
  - Yearly levels

#### 3.  Enables Drill-Down Analysis
Allows users to explore data from **high-level summaries** to detailed views:
- **Total Sales** → By Invoice
- **Invoice Details** → By Product
- **Product Details** → By Store

>  *Facilitates trend detection, top-selling products, and understanding customer behavior.*

#### 4. Tracks Price Variations at the Invoice Line Level
- Captures both **Unit Price** and **Discounted Price** at time of sale.
- Supports:
  - **Profitability analysis** (e.g., effect of discounts on revenue)
  - **Price optimization strategies**

---















