# Data Dictionary

## `Sales` table: source fields

Example values are from one real line: an EnduroMaster 1000 sold on 27 Jan 2022.

| Field | Type | Description | Example |
|---|---|---|---|
| Product ID | Whole number | Unique order-line identifier (table key) | 1079 |
| Product Category | Text | Bike category: Mountain, Touring, Road, E-Bikes, Hybrid, Bmx or Kids Bikes | Mountain Bikes |
| Product Subcategory | Text | One of 22 subcategories | Enduro |
| Product Name | Text | Model name (48 models) | EnduroMaster 1000 |
| Order Date | Date | Date of the order line | 2022-01-27 |
| Wholesale Price | Currency | Unit cost to the retailer | $1,656 |
| Retail Price | Currency | Unit selling price | $2,300 |
| Order Quantity | Whole number | Units on the line (1–3) | 2 |
| Payment Method | Text | Credit Card or PayPal | PayPal |
| Order Status | Text | Shipped, Processing or Cancelled at report date | Shipped |
| Order ID | Whole number | Order or account identifier; groups several lines | 2027 |
| Customer ID | Whole number | Customer identifier (one to one with Order ID) | 3027 |
| Product Description | Text | Free-text description | Endurance-focused mountain bike |
| Product Size | Text | M or L | M |
| Product Weight | Whole number | Weight in kg | 42 |

## `Sales` table: calculated columns

| Column | Definition | Notes |
|---|---|---|
| Revenue | Retail Price × Order Quantity; **0 if cancelled** | Excludes tax |
| Profit | (Retail Price − Wholesale Price) × Order Quantity; **0 if cancelled** | Gross profit |
| Tax Due | Revenue × 5% when Revenue > $2,000, otherwise 0 | Shipped and processing lines only, because cancelled lines have Revenue = 0 |
| Customer Bill | Revenue + Tax Due | What the customer pays; tax is added on top, not deducted from revenue |
| Order Value | Retail Price × Order Quantity, whatever the status | Values cancelled and processing lines |
| Tax Proportion | Tax Due ÷ Revenue | 0% or 5% per line |
| Order Month | Month name of Order Date (Jan, Feb…) | Sorted by Order Month No; feeds the Products page Month slicer |
| Order Month No | Month number of Order Date | Hidden sort key |

## `Date` table

| Column | Definition |
|---|---|
| Date | One row per day, 1 Jan of the first order year to 31 Dec of the last (marked date table) |
| Month | Month name (Jan, Feb…), sorted by Month No |
| Month No | 1–12 |
| Year | Calendar year; drives the Year slicer |
| Quarter | Q1–Q4 |

## Other objects

| Object | Purpose |
|---|---|
| `Refresh Info` (hidden table) | One row holding the date and time of the last refresh, shown in each page footer |
| `Measure` table | Holds all 182 measures; see [Measures](../03_DAX/Measures.md) |
| `Data Folder` parameter | Folder containing the workbook; change it after cloning the repo |
