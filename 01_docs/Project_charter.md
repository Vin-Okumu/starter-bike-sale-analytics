<h1 align = "center"> 
Starter Bike LLC Sales Analytics Dashboard<br>
Project Charter
</h1>

<p align = "center">
<img src = "../08_Images/Cover/Project_Cover.png" width = "1000" height = "400">
</p>

## Background

Starter Bike LLC (a fictional company) is a bicycle retailer that sells various types of bicycles to its emerging consumer niche. Having just completed its annual operations for the second year running, company executives demand that consolidated transactional data, including recorded transactional sales data, product information, pricing details, quantities sold, order status, and sales dates, be utilized  to design a data-driven sales drive. However, although operational data is readily available, the executives still lacks a centralized reporting solution that can transform available raw information into meaningful business insights.

The company executives believe that an analytical dashboard can help decision-makers spend considerablly less time compiling reports manually, making it a little simpler to identify revenue drivers, evaluate product performance, monitor operational efficiency, and estimate tax obligations. 

Therefore, this project is intended to transform the company's transactional sales data into an interactive Business Intelligence solution using Microsoft Power BI.

## Business Problem

While Starter Bike LLC generates sales every day, company executives still lack visibility into overall business performance.

Management cannot easily answer critical operational questions such as:

- Which products contribute the most to revenue?
- Which product categories are the most profitable?
- How much tax liability has the business incurred?
- Which products trigger the highest tax expense?
- How are sales changing over time?
- Which orders are successfully completed versus cancelled?
- Where are operational inefficiencies occurring?

Without these insights, management believe the business risks relying on decisions based on assumptions rather than evidence.

## Business Context

As a company governed by financial tax laws, Starter Bike LLC operates under the following tax rule:

Any individual sale order whose value exceeds $2,000 attracts a 5% tax, payable by the company to the Internal Revenue Service (IRS). This tax is treated as a business expense and added to the sales. However, sales valued at $2,000 or below are exempt from this tax.

## Project Goal

To design and develop an interactive Power BI dashboard that will enable Starter Bike LLC executives to monitor sales performance, product performance, operational efficiency, and tax obligations while supporting evidence-based business decision-making.

## Project Objectives

The dashboard will enable management to monitor:

### Sales Performance
- Total revenue generated.
- Sales trends over time.
- Order value and order volumes.

### Product Performance
- Top-performing products.
- Comparison between product categories.
- Product profitability.
- Product contribution to total revenue.

### Operational Performance
- Order status distribution.
- Cancellation of orders.
- Operational efficiency.
- Products with high risk of cancellation.

### Tax Analysis
- Tax liability.
- Net revenue after tax.
- Tax contribution by product and category.

### Executive Reporting

Provide an interactive dashboard allowing executives to filter results by:

- Date, Month and Year
- Product Category
- Order Status

## Scope

This project include documentation of:

- Data cleaning and transformation using Power Query
- Data modeling
- DAX calculations
- Interactive dashboard development
- KPI development
- Executive reporting
- Tax calculations

## Success Criteria

The project is considered successful if it delivers:

- An interactive Power BI dashboard
- Accurate tax calculations
- Executive KPI page
- Customer analytics page
- Operational dashboard
- Product performance dashboard
- Fully documented GitHub repository
- Reproducible workflow

## Deliverables

- A Clean dataset
- Data dictionary
- Power Query documentation
- Data model documentation
- DAX measures documentation
- Power BI dashboard
- Executive summary
- GitHub repository

## Risks

- Missing values: Mitigated by data ceaning procedures
- Duplicate orders: Mitigated by duplicate detection
- Incorrect data types: Mitigated by data validation
- Inconsistent customer information: Mitigated by standardization
- Tax calculation errors: Mitigated by validation against business rules

## Assumption

1. Dataset represents completed business transactions.
2. Customer IDs uniquely identify customers.
3. Order IDs uniquely identify orders.
4. Product prices are accurate.
5. Tax is calculated using pre-tax order value.

## Business Rule

Sales Tax Rule

If an order value surpasses $2000, then the product sales attracts a tax of 5%. Otherwise the sales tax is 0%

The tax is imposed on the customer but does not constitute company revenue as it is remitted to the IRS by the company.
