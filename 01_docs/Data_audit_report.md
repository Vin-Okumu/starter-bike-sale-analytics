## Dataset Overview

    Item                |   Description
    --------------------|---------------------------------
    Dataset Name        |   Quarter One Bike Sales Report
    Source              |           Internal Sales System
    Format              |          Microsoft Excel(.xlsx)
    Reporting Period    |                       Quarter 1
    Number of Rows      |                             245
    Number of Columns   |                              21
    Date Audited        |                       July 2026
    Analyst             |                   Vincent Okumu
    --------------------|----------------------------------

### Dataset Description

The dataset contains transactional sales records for Starter Bike LLC (fictional Bicycle Company) during Quarter One. Each record represents an individual customer order and includes customer information, product details, payment method, sales amount, and order status.

## Dataset Inventory

Every variable documented

    Column              | Description               | Expected Data Type | Business Purpose
    --------------------| :-------------------------| :------------------| :---------------
    Product ID          | Unique product identifier | Number             | Primary key
    Product category    | Product category          | Text               | Operations
    Product Subcategory | Product subcategory       | Text               | OPerations
    Product Name        | Product Name              | Text               | Product descriptor
    Order Date          | Date of product order     | Date               | Trend analysis
    Wholesale Price     | Wholesale price of product| Currency           | Revenue calculation
    Retail Price        | Retail price of product   | Currency           | Revenue calculation
    Order Quantity      | Units sold for the order  | Number             | Sales
    Payment Method      | Type of preferred payment | Text               | Payment preference analysis
    Order Status        | Order completion          | Text               | Operations
    Order ID            | Unique order identifier   | Number             | Foreign Key
    Customer ID         | Unique customer identifier| Number             | Foreign key
    Product Description | Product description       | Text               | Operations
    Product Size        | Product size              | Text               | Operations
    Product Weight      | Product weight            | Number             | Operations

## Data Profiling

Here, we are essentially examining each variable to answer the question as to whether the variables are stored in their correct data types

    Variable                | Current Type  | Expected Type | Action
    ----------------------- | :------------ | :------------ | :-------
    Product ID              | Whole Number  | Whole Number  | None
    Product Category        | Text          | Text          | None
    Product Subcategory     | Text          | Text          | None
    Product Name            | Text          | Text          | None
    Order Date              | Date          | Date          | None
    Wholesale Price         | Decimal Number| Currency      | Convert
    Retail Price            | Decimal Number| Currency      | Convert
    Order Quantity          | Whole Number  | Whole Number  | None
    Payment Method          | Text          | Text          | None
    Order Status            | Text          | Text          | None
    Order ID                | Whole Number  | Whole Number  | None
    Customer ID             | Whole Number  | Whole Number  | None
    Product Description     | Text          | Text          | None
    Product Size            | Text          | Text          | None
    Product Weight          | Whole number  | Whole Number  | None

## Missing Values

The dataset has no missing values from the fundamental fields. The only fields with missingness, are calculated columns, where functions will be used to fill. 

    Variable    |   Missing Count   |   Missing %   |   Action
    ------------|-------------------|---------------|----------

    
## Uniqueness Test

For identifiers, Order ID should be unique while Product ID and Customer ID should repeat.

## Invalid values

Negative quantities, negative prices, negative revenue, blank payment methods etc., should be flagged for cleaning.

## Outlier detection




