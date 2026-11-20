# Data Audit Report

## Dataset overview

| Item | Description |
|---|---|
| Dataset name | Quarter One Bike Sales |
| File | [`02_data/quarter_one_sales_workbook.xlsx`](../02_data/quarter_one_sales_workbook.xlsx), sheet `Sales` |
| Source | Internal sales system export |
| Format | Microsoft Excel (.xlsx) |
| Reporting period | Q1 2022 (1 Jan–31 Mar) and Q1 2023 (1 Jan–28 Mar) |
| Rows | 245 order lines (102 in 2022, 143 in 2023) |
| Columns | 15 source fields, plus 8 calculated columns added in the model |
| Analyst | Vincent Okumu |

### Description

Transactional sales records for Starter Bike LLC, a fictional bicycle retailer. **Each row is one order line:** one product on one date, with its price, quantity, payment method and status.

## Dataset inventory

| Column | Description | Expected type | Business purpose |
|---|---|---|---|
| Product ID | Unique line identifier | Whole number | Primary key (one per row) |
| Product Category | Bike category (7 values) | Text | Product analysis |
| Product Subcategory | Bike subcategory (22 values) | Text | Product analysis |
| Product Name | Model name | Text | Product analysis |
| Order Date | Date of the order line | Date | Trend analysis, prior-year comparison |
| Wholesale Price | Unit cost to the retailer | Currency | Profit calculation |
| Retail Price | Unit selling price | Currency | Revenue calculation |
| Order Quantity | Units on the line (1–3) | Whole number | Volume |
| Payment Method | Credit Card or PayPal | Text | Payment analysis |
| Order Status | Shipped, Processing or Cancelled | Text | Fulfilment and cancellation analysis |
| Order ID | Order identifier | Whole number | Grouping of lines (see Identifiers) |
| Customer ID | Customer identifier | Whole number | Customer counts |
| Product Description | Free-text description | Text | Reference only |
| Product Size | M or L | Text | Reference only |
| Product Weight | Weight in kg (9–42) | Whole number | Reference only |

## Data type profiling

| Variable | Type in Excel | Type in model | Action |
|---|---|---|---|
| Product ID | Whole number | Whole number | None |
| Product Category | Text | Text | Header trimmed (had a trailing space) |
| Product Subcategory, Product Name | Text | Text | None |
| Order Date | Date | Date | None |
| Wholesale Price | Decimal | Currency | Converted |
| Retail Price | Whole number | Currency | Converted |
| Order Quantity | Whole number | Whole number | None |
| Payment Method, Order Status | Text | Text | None |
| Order ID, Customer ID | Whole number | Whole number | None |
| Product Description, Product Size | Text | Text | None |
| Product Weight | Whole number | Whole number | None |

## Missing values

| Variable | Missing count | Missing % | Action |
|---|---:|---:|---|
| All 15 source fields | 0 | 0% | None required |

## Uniqueness

| Identifier | Distinct values | Finding |
|---|---:|---|
| Product ID | 245 | Unique on every row, so it identifies the **order line**, not the product. It's the table's key. |
| Order ID | 48 | Each covers 1–10 lines, and the same ID appears in both years and on up to 6 different dates. It behaves like a customer account, not a single checkout. |
| Customer ID | 48 | Maps one to one to Order ID. |
| Product Name | 48 | Each model belongs to exactly one category. |

Because of this, the dashboard measures fulfilment and cancellation in **order lines**, and doesn't analyse orders per customer or repeat purchases.

## Invalid values

| Check | Result |
|---|---|
| Negative or zero prices | None. Retail price $150–$3,700; wholesale price $90–$2,625. |
| Negative or zero quantities | None. Quantity is 1–3. |
| Blank payment method or status | None. Two payment methods and three statuses only. |
| Retail price below wholesale | **1 line.** Product ID 1016, EnduroMaster 2000, 28 Mar 2023: retail $1,600 against wholesale $1,872. The same model sells at $2,600 elsewhere, so this is probably a pricing or entry error. It's kept as recorded and makes a −$272 profit on a processing line. |
| Lines identical apart from Product ID | **2 pairs.** ReclineRider 1000 on 23 Feb 2023 (Order 2085, 2 units at $2,000) and TrailBlazer 2000 on 2 Mar 2023 (Order 2002, 1 unit at $1,500), both processing. These may be double entries, but nothing in the data confirms it, so they're kept. Together they account for $5,500 of revenue. |

## Outliers

| Variable | Range | Finding |
|---|---|---|
| Retail Price | $150–$3,700 (mean $1,948) | Low values are BMX and Kids bikes, which is genuine. The only suspect value is the $1,600 EnduroMaster 2000 line above. |
| Order Quantity | 1–3 (mean 1.6) | No outliers. |
| Order lines per month | 32–52 | Volume rises steadily from Jan 2022 to Mar 2023, with no spikes. |

## Conclusion

The data is complete and correctly typed and fit for the analysis. Three findings are documented rather than "fixed", because the source can't confirm the correct values:
- One line priced below wholesale.
- Two possible duplicate lines.
- Order and Customer IDs that don't represent single orders.

Their combined effect on the headline figures is under 2% of revenue.
