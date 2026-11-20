# Calculated Columns, Tables and Parameters

Everything the model adds on top of the source workbook. Measures are documented separately in [Measures](Measures.md).

## Data model

```
Date (1) ──── (*) Sales        Date[Date] → Sales[Order Date], single direction
Refresh Info                   standalone, hidden
Measure                        holds all measures, no columns
```

- **Auto date/time is off.** The model uses its own `Date` table, which is marked as the date table, so `SAMEPERIODLASTYEAR` works.
- **`Data Folder` parameter.** The Power Query source reads `#"Data Folder" & "quarter_one_sales_workbook.xlsx"`. To point the model at your copy of the repo, change this one value (**Transform data → Edit parameters**).

## Calculated columns in `Sales`

**1. Revenue.** Retail price × quantity for shipped and processing lines; 0 for cancelled lines.
```DAX
Revenue =
IF (
    NOT ( 'Sales'[Order Status] = "Cancelled" ),
    'Sales'[Retail Price] * 'Sales'[Order Quantity],
    0
)
```

**2. Profit.** (Retail − wholesale) × quantity for shipped and processing lines; 0 for cancelled lines.
```DAX
Profit =
IF (
    NOT ( 'Sales'[Order Status] = "Cancelled" ),
    ( 'Sales'[Retail Price] - 'Sales'[Wholesale Price] ) * 'Sales'[Order Quantity],
    0
)
```

**3. Tax Due.** 5% on fulfilled lines worth more than $2,000. Cancelled lines have Revenue = 0, so they're never taxed.
```DAX
Tax Due =
IF ( 'Sales'[Revenue] > 2000, 'Sales'[Revenue] * 0.05, 0 )
```

**4. Customer Bill.** What the customer pays: revenue plus the tax added on top. Tax is collected from the customer and remitted, so it is not deducted from revenue.
```DAX
Customer Bill = 'Sales'[Revenue] + 'Sales'[Tax Due]
```

**5. Order Value.** Retail price × quantity whatever the status. Used to value cancelled and processing lines.
```DAX
Order Value = 'Sales'[Retail Price] * 'Sales'[Order Quantity]
```

**6. Tax Proportion.** Tax as a share of the line's revenue (0% or 5%).
```DAX
Tax Proportion = DIVIDE ( [Tax Due], [Revenue] )
```

**7. Order Month** and **8. Order Month No.** Month name taken from the sales data, sorted by month number. The Products page's Month slicer uses it, so it only lists months that have orders (Jan–Mar).
```DAX
Order Month = FORMAT ( Sales[Order Date], "MMM" )      -- sorted by Order Month No

Order Month No = MONTH ( Sales[Order Date] )           -- hidden
```

## Calculated tables

**Date.** A calendar covering full years from the first to the last order. It's marked as the date table and joined to `Sales[Order Date]`.
```DAX
Date =
CALENDAR (
    DATE ( YEAR ( MIN ( Sales[Order Date] ) ), 1, 1 ),
    DATE ( YEAR ( MAX ( Sales[Order Date] ) ), 12, 31 )
)
```

| Column | Expression | Notes |
|---|---|---|
| `Date` | from `CALENDAR` | Key column of the marked date table |
| `Month` | `FORMAT ( 'Date'[Date], "MMM" )` | Sorted by `Month No`; used on line chart axes |
| `Month No` | `MONTH ( 'Date'[Date] )` | Sort key |
| `Year` | `YEAR ( 'Date'[Date] )` | Year slicer on every page (single select, default 2023) |
| `Quarter` | `"Q" & QUARTER ( 'Date'[Date] )` | |

**Refresh Info.** Records when the data was last refreshed. It's recalculated on every refresh, and `[Last Refreshed]` shows it in each page footer.
```DAX
Refresh Info = ROW ( "Refreshed At", NOW () )
```
