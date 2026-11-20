# Data Profile Summary

A snapshot of what the data contains, profiled from [`quarter_one_sales_workbook.xlsx`](../02_data/quarter_one_sales_workbook.xlsx).

## Volume

| | Q1 2022 | Q1 2023 | Total |
|---|---:|---:|---:|
| Order lines | 102 | 143 | 245 |
| Order IDs | 32 | 48 | 48 distinct |
| Customer IDs | 32 | 48 | 48 distinct |
| Categories sold | 4 | 7 | 7 |

Order lines per month: Jan 2022 32 · Feb 34 · Mar 36 · Jan 2023 45 · Feb 46 · Mar 52.

## Categorical fields

| Field | Distinct values | Values |
|---|---:|---|
| Product Category | 7 | Mountain, Touring, Road, E-Bikes (both years); Hybrid, Bmx, Kids Bikes (2023 only) |
| Product Subcategory | 22 | e.g. Cross Country, Downhill, Racing, Gravel, Touring, Electric, Freestyle |
| Product Name | 48 | Each model belongs to one category |
| Payment Method | 2 | Credit Card, PayPal |
| Order Status | 3 | Shipped (123 lines), Processing (91), Cancelled (31) |
| Product Size | 2 | M, L |

## Numeric fields

| Field | Min | Max | Mean |
|---|---:|---:|---:|
| Retail Price | $150 | $3,700 | $1,948 |
| Wholesale Price | $90 | $2,625 | $1,349 |
| Order Quantity | 1 | 3 | 1.6 |
| Product Weight (kg) | 9 | 42 | 26.2 |

## Quality summary

| Check | Result |
|---|---|
| Missing values | None |
| Data types | Correct after setting prices to Currency |
| Header issues | One trailing space (`Product Category`), trimmed on import |
| Duplicate rows | None; 2 pairs identical apart from Product ID, flagged |
| Invalid values | None; 1 line priced below wholesale, flagged |
| Identifiers | Product ID unique per line; Order and Customer IDs group lines across dates |

Details are in the [data audit report](Data_audit_report.md) and the [cleaning report](Data_cleaning_report.md).
