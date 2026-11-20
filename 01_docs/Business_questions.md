# Business Questions

Scoping started with 40+ questions from the [project charter](Project_charter.md) and [requirements](business_requirement_document.md). For the dashboard they were narrowed to **four decision questions**: one per page, each for a named user. Every visual on a page answers its page's question. The supporting questions below show where each answer sits; the rest are listed at the end as out of scope.

## The four decision questions

| # | Page | User | Decision question | Answer (Q1 2023) |
|---:|---|---|---|---|
| 1 | Executive Overview | Owner / GM | **Are we growing, and what's driving it?** | Yes: revenue up 36.5% to $337.6K, beating Q1 2022 every month. Mountain Bikes earned 49% of it. |
| 2 | Products & Margins | Owner / buyer | **Which product lines should we push, protect or cut?** | Protect Mountain Bikes. Hybrid, BMX and Kids earn 40% margins on 2.6% of revenue. PayPal lines are 41% larger. |
| 3 | Orders & Cancellations | Operations manager | **Where are orders failing, and why?** | Road Bikes: 46% of lines cancelled, $96.3K of $97.5K lost, from five models. Recurring in both years. |
| 4 | Tax | Bookkeeper | **How much tax do we owe, and where does it come from?** | $13,915 (4.1% of revenue), 52% from Mountain Bikes. |

## Supporting questions and where they're answered

### 1. Executive Overview
| Question | Answered by |
|---|---|
| What was total revenue, and how does it compare with last year? | Revenue KPI (▲ vs Q1 2022) |
| How much gross profit, and at what margin? | Gross profit and Gross margin KPIs |
| How many bikes were sold? | Bikes sold KPI |
| How did revenue change month by month? | Revenue trend line (this year vs last year) |
| Which category generates the most revenue? | Revenue by category bar |
| How much order value was lost to cancellations, and where? | Lost to cancellations KPI; cancelled value by category |
| What should we do next? | "What to do next" box (dynamic figures) |

### 2. Products & Margins
| Question | Answered by |
|---|---|
| Which categories earn the most, and at what margin? | Revenue vs margin scatter; category detail table |
| How concentrated is revenue in the top category? | Top category's share KPI |
| Which models generate the most revenue? | Top 10 models bar; top model KPI |
| Which payment method brings larger orders? | PayPal vs card bar and note; PayPal KPI |
| Is the product range growing? | Categories selling KPI (7 vs 4) |

### 3. Orders & Cancellations
| Question | Answered by |
|---|---|
| What share of order lines shipped, are processing, or were cancelled? | Three status KPIs (vs Q1 2022) |
| Which categories have the highest cancellations? | Order status by category bar (with line counts) |
| Is the cancellation rate rising or stable? | Cancellation-rate trend (this year vs last year) |
| Which models drive the cancellations? | Cancelled models table |
| How much order value is still processing? | Order value by status bar |

### 4. Tax
| Question | Answered by |
|---|---|
| How much tax is owed, and how has it changed? | Tax due KPI; tax trend line |
| What is the effective tax rate? | Tax as % of revenue KPI; monthly tax schedule |
| What share of lines is taxable? | Lines taxed KPI |
| Which categories and months generate the most tax? | Tax by category bar; highest tax month KPI; monthly schedule |
| How is tax calculated and billed? | "How tax is calculated" box; Customer billings KPI |

## Out of scope

These were on the original list but aren't answered on the dashboard:

| Question | Why not |
|---|---|
| How much revenue remains after tax? | Tax is added to the customer's bill and remitted, not deducted from revenue, so "revenue after tax" would be misleading. Customer billings incl. tax is shown instead. |
| How many customers made 80% of sales? Average orders per customer? | Order and Customer IDs map one to one and span several dates, so they don't support customer-level analysis. |
| Which demographic groups spend the most? | The data has no customer demographics. |
| Which products have the highest markup? Are higher-priced bikes more profitable? | Partly covered by margin by category. Model-level pricing analysis is a possible follow-on project. |
