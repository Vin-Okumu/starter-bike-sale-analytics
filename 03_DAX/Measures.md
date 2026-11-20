# DAX Measures

The model has **182 measures**, all in the `Measure` table and grouped into eight display folders. This page explains how they fit together and then lists every one. The full code is in [`Measure.tmdl`](../04_reports/starter-bike-sales-analytics-dashboard.SemanticModel/definition/tables/Measure.tmdl). Calculated columns and tables are in [Calculations](Calculations.md).

| Folder | Count | Purpose |
|---|---:|---|
| `1 Core` | 21 | Base figures: revenue, profit, order lines, order value by status, shipped/processing/cancelled rates |
| `2 Comparisons` | 42 | Prior-year values, % changes and the "▲ 36.5% vs Q1 2022 ($247.3K)" text under each KPI |
| `0 KPI Text` | 8 | KPI card values formatted as $337.6K |
| `3 Products` | 10 | Top category, top model, category shares, PayPal vs card |
| `4 Orders` | 8 | Worst category for cancellations, line counts, share of loss by model |
| `5 Tax` | 7 | Taxable lines, effective rate, top tax month and category |
| `6 Titles` | 33 | Chart titles and subtitles that state the insight and update with the slicers |
| `7 Formatting` | 53 | Colours, data labels, table highlights and axis ranges used for conditional formatting |

## Conventions

- **Revenue excludes cancelled lines and tax.** `Sales[Revenue]` is 0 for cancelled lines. Use `[Order Value]` for the value of all lines, including cancelled.
- **Order lines, not orders.** Each row of `Sales` is one order line; `Product ID` is unique per row. Rates such as `[Cancelled Line %]` are shares of order lines.
- **Prior year = same months a year earlier.** All `… PY` measures use `SAMEPERIODLASTYEAR` on the marked `Date` table. With the Year slicer on 2023 they return Q1 2022. With 2022 they return blank, and the labels say "No prior-year data".
- **"Top" measures follow the slicers.** Measures such as `[Top Category]` and `[Top Cancel Category]` are recalculated for whatever is selected, so titles and highlights always point at the current leader.

## How the main patterns work

### 1. Core figures
```DAX
Total Revenue = SUM ( Sales[Revenue] )

Gross Margin % = DIVIDE ( [Total Profit], [Total Revenue] )

Cancelled Value = CALCULATE ( [Order Value], Sales[Order Status] = "Cancelled" )

Cancelled Line % = DIVIDE ( [Total Cancelled Order], [Order Lines] )
```

### 2. Prior-year comparison under each KPI card
Each KPI has a value, a prior-year value, a comparison label and a colour:
```DAX
Revenue PY = CALCULATE ( [Total Revenue], SAMEPERIODLASTYEAR ( 'Date'[Date] ) )

Revenue vs PY Label =
VAR cur = [Total Revenue]
VAR py = [Revenue PY]
VAR chg = DIVIDE ( cur - py, py )
RETURN
    IF (
        ISBLANK ( py ),
        "No prior-year data",
        IF ( chg >= 0, "▲ ", "▼ " ) & FORMAT ( ABS ( chg ), "0.0%" ) & " vs " & [Prior Period Label]
            & " (" & IF ( ABS ( py ) >= 1000, FORMAT ( py / 1000, "$#,0.0" ) & "K", FORMAT ( py, "$#,0" ) ) & ")"
    )

Revenue vs PY Color =
VAR cur = [Total Revenue]
VAR py = [Revenue PY]
VAR chg = DIVIDE ( cur - py, py )
RETURN
    SWITCH ( TRUE (), ISBLANK ( py ), "#4B5563", chg > 0, "#15803D", chg < 0, "#B91C1C", "#4B5563" )
```
- **Percentages** (margin, shipped rate and so on) compare in percentage points. A change under 0.5 points shows as "≈ flat" in grey.
- **"Up is bad" measures** (cancelled value, cancellation rates) swap green and red.
- **Neutral measures** (tax) stay grey.

### 3. Insight titles
Titles are measures, applied through **Format → Title → Text → fx**, so the sentence stays true when the slicers change:
```DAX
Title Category Revenue =
VAR c = CALCULATE ( [Top Category], ALLSELECTED ( Sales[Product Category] ) )
VAR s = CALCULATE ( [Top Category Share], ALLSELECTED ( Sales[Product Category] ) )
RETURN c & " earned " & FORMAT ( s, "0%" ) & " of revenue"
```
With Year = 2023 this returns "Mountain Bikes earned 49% of revenue".

### 4. One accent colour per chart
The highlighted bar, label or table row comes from a colour measure applied through **fx → Field value**:
```DAX
Color Top Category =
VAR topC = CALCULATE ( [Top Category], ALLSELECTED ( Sales[Product Category] ) )
RETURN IF ( SELECTEDVALUE ( Sales[Product Category] ) = topC, "#1D4ED8", "#C3C9D2" )
```
`ALLSELECTED` finds the leader across all bars on the chart, and `SELECTEDVALUE` checks whether the current bar is that leader.

### 5. Bars inside tables
Power BI data bars can't colour one row differently, or put the figure at the bar's end. The Products and Orders tables therefore draw the bar as text, using the ━ character scaled to the largest value, with the figure after it:
```DAX
Revenue Bar Label =
VAR v = [Total Revenue]
VAR mx = MAXX ( ALLSELECTED ( Sales[Product Category] ), [Total Revenue] )
VAR units = 16
VAR n = IF ( v > 0, MAX ( 1, ROUND ( DIVIDE ( v, mx ) * units, 0 ) ), 0 )
VAR txt = IF ( v >= 1000, FORMAT ( v / 1000, "$#,0.0" ) & "K", FORMAT ( v, "$#,0" ) )
RETURN IF ( ISINSCOPE ( Sales[Product Category] ), REPT ( UNICHAR ( 9473 ), n ) & "  " & txt, txt )
```
The font colour (`[Bar Color Top Category]`) turns the top row's bar blue.

## All measures

### 1 Core (21)

| Measure | What it returns |
|---|---|
| `Total Revenue` | Revenue from shipped and processing lines, excluding tax |
| `Total Profit` | Gross profit from shipped and processing lines |
| `Total Tax Due` | 5% tax on fulfilled lines worth more than $2,000 |
| `Average Order Value` | Revenue per order (not used on the pages; kept for ad hoc analysis) |
| `Order Lines` | All order lines, including cancelled |
| `Fulfilled Lines` | Shipped plus processing lines |
| `Gross Margin %` | Gross profit ÷ revenue |
| `Order Value` | Value of all order lines, including cancelled |
| `Shipped Value` | Order value of shipped lines |
| `Processing Value` | Order value still processing |
| `Cancelled Value` | Order value lost to cancellations |
| `Customer Billings` | Revenue plus tax added to the customer's bill |
| `Shipped Line %` | Share of order lines shipped |
| `Processing Line %` | Share of order lines still processing |
| `Cancelled Line %` | Share of order lines cancelled |
| `Total Orders` | Distinct orders |
| `Customers` | Distinct customers (not used on the pages; kept for ad hoc analysis) |
| `Quantity Sold` | Bikes on shipped and processing lines |
| `Action Cancellations` | "What to do next" line 1: the cancellation fix and its value |
| `Action Top Category` | "What to do next" line 2: revenue concentration |
| `Action Payment` | "What to do next" line 3: payment method with the larger order lines |

### 2 Comparisons (42)

| Measure | What it returns |
|---|---|
| `Top Category Share PY` | Top Category Share for the same months a year earlier |
| `Top Model Revenue PY` | Top Model Revenue for the same months a year earlier |
| `Top Cancel Category Rate PY` | Top Cancel Category Rate for the same months a year earlier |
| `Top Tax Month Value PY` | Top Tax Month Value for the same months a year earlier |
| `Prior Period Label` | Text such as "Q1 2022" used in labels |
| `Revenue PY` | Revenue for the same months a year earlier |
| `Revenue YoY %` | % change in revenue vs prior year |
| `Revenue vs PY Label` | KPI comparison text for revenue, e.g. "▲ 36.5% vs Q1 2022 ($247.3K)" |
| `Gross Profit PY` | Gross Profit for the same months a year earlier |
| `Gross Profit vs PY Label` | KPI comparison text for gross profit, e.g. "▲ 36.5% vs Q1 2022 ($247.3K)" |
| `Quantity Sold PY` | Quantity Sold for the same months a year earlier |
| `Quantity Sold vs PY Label` | KPI comparison text for quantity sold, e.g. "▲ 36.5% vs Q1 2022 ($247.3K)" |
| `Cancelled Value PY` | Cancelled Value for the same months a year earlier |
| `Cancelled Value vs PY Label` | KPI comparison text for cancelled value, e.g. "▲ 36.5% vs Q1 2022 ($247.3K)" |
| `Tax Due PY` | Tax Due for the same months a year earlier |
| `Tax Due YoY %` | % change in tax due vs prior year |
| `Tax Due vs PY Label` | KPI comparison text for tax due, e.g. "▲ 36.5% vs Q1 2022 ($247.3K)" |
| `Customer Billings PY` | Customer Billings for the same months a year earlier |
| `Customer Billings vs PY Label` | KPI comparison text for customer billings, e.g. "▲ 36.5% vs Q1 2022 ($247.3K)" |
| `Gross Margin PY` | Gross Margin for the same months a year earlier |
| `Gross Margin vs PY Label` | KPI comparison text for gross margin, e.g. "▲ 36.5% vs Q1 2022 ($247.3K)" |
| `Shipped Lines PY` | Shipped Lines for the same months a year earlier |
| `Shipped Lines vs PY Label` | KPI comparison text for shipped lines, e.g. "▲ 36.5% vs Q1 2022 ($247.3K)" |
| `Processing Lines PY` | Processing Lines for the same months a year earlier |
| `Processing Lines vs PY Label` | KPI comparison text for processing lines, e.g. "▲ 36.5% vs Q1 2022 ($247.3K)" |
| `Cancelled Lines PY` | Cancelled Lines for the same months a year earlier |
| `Cancelled Lines vs PY Label` | KPI comparison text for cancelled lines, e.g. "▲ 36.5% vs Q1 2022 ($247.3K)" |
| `Tax Rate PY` | Tax Rate for the same months a year earlier |
| `Tax Rate vs PY Label` | KPI comparison text for tax rate, e.g. "▲ 36.5% vs Q1 2022 ($247.3K)" |
| `Taxable Lines PY` | Taxable Lines for the same months a year earlier |
| `Taxable Lines vs PY Label` | KPI comparison text for taxable lines, e.g. "▲ 36.5% vs Q1 2022 ($247.3K)" |
| `Top Category Share vs PY Label` | KPI comparison text for top category share, e.g. "▲ 36.5% vs Q1 2022 ($247.3K)" |
| `Top Cancel Category Rate vs PY Label` | KPI comparison text for top cancel category rate, e.g. "▲ 36.5% vs Q1 2022 ($247.3K)" |
| `Top Model Revenue YoY %` | % change in top model revenue vs prior year |
| `Top Tax Month Value YoY %` | % change in top tax month value vs prior year |
| `PayPal vs Card Label` | "▲ 41.1% vs credit card ($2,287)" |
| `Categories Selling PY` | Categories with sales in the prior year |
| `Categories Selling vs PY Label` | "+3 new vs Q1 2022 (4)" |
| `Top Model Subtitle` | Top model name with its change vs prior year |
| `Top Tax Month Subtitle` | Highest tax month with its change vs the same month last year |
| `Tooltip Cancelled Models` | Hover list of models and lost value per category |
| `Current Period Label` | Text such as "Q1 2023" |

### 0 KPI Text (8)

| Measure | What it returns |
|---|---|
| `KPI Revenue` | KPI card value for revenue, formatted as $337.6K |
| `KPI Gross Profit` | KPI card value for gross profit, formatted as $337.6K |
| `KPI Cancelled Value` | KPI card value for cancelled value, formatted as $337.6K |
| `KPI Tax Due` | KPI card value for tax due, formatted as $337.6K |
| `KPI Customer Billings` | KPI card value for customer billings, formatted as $337.6K |
| `KPI Top Model Revenue` | KPI card value for top model revenue, formatted as $337.6K |
| `KPI PayPal Line Value` | KPI card value for paypal line value, formatted as $337.6K |
| `KPI Top Tax Month Value` | KPI card value for top tax month value, formatted as $337.6K |

### 3 Products (10)

| Measure | What it returns |
|---|---|
| `Avg Line Value` | Average revenue per fulfilled order line |
| `Category Revenue Share` | Category revenue ÷ revenue of all selected categories |
| `Categories Selling` | Categories with revenue in the period |
| `Top Category` | Category with the most revenue |
| `Top Category Share` | Top category's share of revenue |
| `Top Model` | Model with the most revenue |
| `Top Model Revenue` | Revenue of the top model |
| `PayPal Line Value` | Average PayPal order line |
| `Card Line Value` | Average credit card order line |
| `PayPal Premium %` | How much larger PayPal lines are than card lines |

### 4 Orders (8)

| Measure | What it returns |
|---|---|
| `Top Cancel Category` | Category losing the most order value to cancellations |
| `Top Cancel Category Rate` | Share of that category's lines cancelled |
| `Top Cancel Category Value Share` | That category's share of all value lost |
| `Total Shipped Order` | Shipped order lines |
| `Total Cancelled Order` | Cancelled order lines |
| `Total Orders Under Process` | Order lines still processing |
| `Cancelled Share of Loss` | A model's share of all value lost (cancelled-models table) |
| `Top Cancel Model` | Model losing the most order value |

### 5 Tax (7)

| Measure | What it returns |
|---|---|
| `Taxable Lines` | Fulfilled lines over $2,000 |
| `Taxable Line %` | Taxable lines ÷ fulfilled lines |
| `Tax % of Revenue` | Effective tax rate |
| `Top Tax Month` | Month with the most tax due |
| `Top Tax Month Value` | Tax due in that month |
| `Top Tax Category` | Category generating the most tax |
| `Top Tax Category Share` | That category's share of tax due |

### 6 Titles (33)

| Measure | What it returns |
|---|---|
| `Title Revenue Trend` | Insight title: revenue trend chart |
| `Title Category Revenue` | Insight title: category revenue chart |
| `Title Cancellations` | Insight title: cancellations chart |
| `Title Order Status` | Insight title: order status chart |
| `Title Top Model` | Insight title: top model chart |
| `Title PayPal` | Insight title: paypal chart |
| `Title Processing Value` | Insight title: processing value chart |
| `Title Tax Trend` | Insight title: tax trend chart |
| `Title Tax Category` | Insight title: tax category chart |
| `Last Refreshed` | "Last refreshed: dd mmm yyyy" from the Refresh Info table |
| `Title Margin Leaders` | Insight title: margin leaders chart |
| `Title Cancel Trend` | Insight title: cancel trend chart |
| `Title Cancelled Models` | Insight title: cancelled models chart |
| `Title Category Table` | Insight title: category table chart |
| `Subtitle Revenue Trend` | Subtitle with the selected year: revenue trend chart |
| `Subtitle Category Revenue` | Subtitle with the selected year: category revenue chart |
| `Subtitle Cancellations` | Subtitle with the selected year: cancellations chart |
| `Subtitle Margin Scatter` | Subtitle with the selected year: margin scatter chart |
| `Subtitle Top Models` | Subtitle with the selected year: top models chart |
| `Subtitle Payment` | Subtitle with the selected year: payment chart |
| `Label Payment` | PayPal chart data labels ("$3,227 per line") |
| `Note Payment Mix` | Note under the PayPal chart (usage vs revenue by payment method) |
| `Subtitle Order Status` | Subtitle with the selected year: order status chart |
| `Subtitle Cancel Trend` | Subtitle with the selected year: cancel trend chart |
| `Subtitle Cancelled Models` | Subtitle with the selected year: cancelled models chart |
| `Subtitle Order Value` | Subtitle with the selected year: order value chart |
| `Subtitle Tax Trend` | Subtitle with the selected year: tax trend chart |
| `Subtitle Tax Category` | Subtitle with the selected year: tax category chart |
| `Title Tax Table` | Insight title: tax table chart |
| `Subtitle Tax Table` | Subtitle with the selected year: tax table chart |
| `Tax Note Taxable` | "How tax is calculated", line 1 |
| `Tax Note Billing` | "How tax is calculated", line 2 |
| `Tax Note Cancelled` | "How tax is calculated", line 3 |

### 7 Formatting (53)

| Measure | What it returns |
|---|---|
| `Revenue vs PY Color` | Green/red/grey for that comparison text |
| `Gross Profit vs PY Color` | Green/red/grey for that comparison text |
| `Quantity Sold vs PY Color` | Green/red/grey for that comparison text |
| `Cancelled Value vs PY Color` | Green/red/grey for that comparison text |
| `Customer Billings vs PY Color` | Green/red/grey for that comparison text |
| `Gross Margin vs PY Color` | Green/red/grey for that comparison text |
| `Shipped Lines vs PY Color` | Green/red/grey for that comparison text |
| `Processing Lines vs PY Color` | Green/red/grey for that comparison text |
| `Cancelled Lines vs PY Color` | Green/red/grey for that comparison text |
| `Top Cancel Category Rate vs PY Color` | Green/red/grey for that comparison text |
| `Top Model Revenue vs PY Color` | Green/red/grey for that comparison text |
| `Color Top Category` | Bar colour: blue (or red) for the top category, grey for the rest |
| `Color Top Cancel Category` | Bar colour: blue (or red) for the top cancel category, grey for the rest |
| `Color Top Model` | Bar colour: blue (or red) for the top model, grey for the rest |
| `Color Top Tax Category` | Bar colour: blue (or red) for the top tax category, grey for the rest |
| `Color Margin Leaders` | Blue dots for those categories on the scatter |
| `Color Top Payment` | Blue bar for the payment method with larger lines |
| `Bg Top Category` | Table highlight for the top category |
| `Text Top Category` | Table highlight for the top category |
| `Bg Margin Leaders` | Light-blue background for those cells |
| `Bg Top Cancel Model` | Table highlight for the top cancel model |
| `Text Top Cancel Model` | Table highlight for the top cancel model |
| `Bg Top Tax Month` | Table highlight for the top tax month |
| `Text Top Tax Month` | Table highlight for the top tax month |
| `Text Margin Leaders` | Blue text for categories 5+ pts above average margin |
| `Label Color Top Category` | Data-label colour matching the highlighted bar (category) |
| `Label Category Revenue` | "$164.2K · 49%" on the top revenue bar |
| `Label Cancelled Value` | "$97.5K · 22%" segment label |
| `Label Color Top Cancel Category` | Data-label colour matching the highlighted bar (cancel category) |
| `Axis Max Cancelled Value` | Axis range that leaves room for labels |
| `Avg Margin Line` | Value for the dashed average-margin line on the scatter |
| `Axis Max Revenue Scatter` | Axis range that leaves room for labels |
| `Axis Min Margin Scatter` | Axis range that leaves room for labels |
| `Axis Max Margin Scatter` | Axis range that leaves room for labels |
| `Label Color Top Model` | Data-label colour matching the highlighted bar (model) |
| `Axis Max Top Models` | Axis range that leaves room for labels |
| `Revenue Bar Label` | Text bar plus figure for the Revenue column (Products table) |
| `Bar Color Top Category` | Colour of that text bar: blue for the top category |
| `Label Color Top Payment` | Data-label colour matching the highlighted bar (payment) |
| `Axis Max Payment` | Axis range that leaves room for labels |
| `Cancelled Bar Label` | Text bar plus figure for "Order value lost" (cancelled-models table) |
| `Bar Color Cancelled Model` | Colour of that bar: red for the worst category |
| `Label Shipped Value` | "$206.2K · 47%" segment label |
| `Label Processing Value` | "$131.4K · 30%" segment label |
| `Label Spacer` | Invisible segment after each order-status bar; carries the line count and the sort order |
| `Label Line Count` | "37 lines" label after each order-status bar |
| `Label Color Line Count` | Red for the worst category's line count |
| `Label Tax Category` | "$7,170 · 52%", or "$0 (no line over $2,000)" |
| `Label Color Top Tax Category` | Data-label colour matching the highlighted bar (tax category) |
| `Axis Max Tax Category` | Axis range that leaves room for labels |
| `Tax Change Label` | ▲/▼ change column in the tax table |
| `Tax Change Color` | Green/red for that column |
| `Label Cancelled Category` | "$96.3K · 99% of value lost" on the Overview cancellations chart |
