# Data Cleaning Report

Actions taken after the [data audit](Data_audit_report.md). The workbook itself is left unchanged; all steps run in Power Query and the model each time the data is refreshed. See the [ETL log](../04_reports/ETL%20log.md) for the full order of steps.

| Check | Result | Action |
|---|---|---|
| Missing values | None | None required |
| Duplicate rows (all 15 fields) | None | None required |
| Lines identical apart from Product ID | 2 pairs (ReclineRider 1000, TrailBlazer 2000) | Kept as recorded and flagged; can't be confirmed as double entries |
| Column headers | `Product Category` had a trailing space | Trimmed every header on import (`Table.TransformColumnNames(…, Text.Trim)`) |
| Data types | Prices stored as plain numbers | Wholesale and Retail Price set to Currency; dates, whole numbers and text set explicitly |
| Retail price below wholesale | 1 line (EnduroMaster 2000, 28 Mar 2023) | Kept as recorded and flagged |
| Invalid quantities or prices | None | None required |
| Null Customer or Order IDs | None | None required |
| Order ID uniqueness | Order IDs repeat across lines, dates and years | Product ID used as the row key; fulfilment measured in order lines |
| Hard-coded file path | The query pointed at one folder on one PC | Replaced with a `Data Folder` parameter |

## Data type validation

- `Order Date` is stored as Date and joined to a marked `Date` table.
- Wholesale and Retail Price are stored as Currency.
- Order Quantity, Product Weight and all IDs are stored as whole numbers.
- Category, subcategory, name, payment method, status, description and size are stored as text.
