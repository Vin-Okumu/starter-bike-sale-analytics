# ETL Log

Steps that turn [`quarter_one_sales_workbook.xlsx`](../02_data/quarter_one_sales_workbook.xlsx) into the dashboard's data model. Steps 1–5 run in Power Query, steps 6–10 in the model (DAX). All of them rerun automatically on **Refresh**.

| Step | Where | Transformation | Detail |
|---:|---|---|---|
| 1 | Power Query | Set the source folder | `Data Folder` parameter, so the path is changed in one place after cloning |
| 2 | Power Query | Import the workbook | `Excel.Workbook ( File.Contents ( #"Data Folder" & "quarter_one_sales_workbook.xlsx" ) )`, sheet `Sales` |
| 3 | Power Query | Promote headers | First row becomes column names |
| 4 | Power Query | Trim header names | `Table.TransformColumnNames ( …, Text.Trim )` fixes `"Product Category "` |
| 5 | Power Query | Set data types | Date, Currency (prices), Whole number (IDs, quantity, weight), Text |
| 6 | Model | Verify row and column counts | 245 rows × 15 columns |
| 7 | Model | Add calculated columns | Revenue, Profit (both 0 for cancelled lines), Tax Due, Customer Bill, Order Value, Tax Proportion, Order Month, Order Month No; see [Calculations](../03_DAX/Calculations.md) |
| 8 | Model | Build the `Date` table | `CALENDAR` over full years, marked as the date table, joined to `Sales[Order Date]`; Auto date/time turned off |
| 9 | Model | Add `Refresh Info` | `ROW ( "Refreshed At", NOW () )` for the footer's "Last refreshed" date |
| 10 | Model | Add measures | 182 measures in 8 display folders; see [Measures](../03_DAX/Measures.md) |

## Change history

| Version | Change |
|---|---|
| v1 | `.pbix` report with 18 measures and photo backgrounds; hard-coded file path |
| v2 | Converted to a Power BI Project (`.pbip`). Rebuilt all four pages to a dashboard-design guide. Marked the Date table and turned off Auto date/time. Trimmed headers and added the `Data Folder` parameter. Replaced "Net Revenue = Revenue − Tax" with Customer Billings, because tax is added to the bill, not deducted from revenue. Added prior-year comparisons, insight titles and conditional formatting. |
