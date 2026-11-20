# Data Quality Checklist

Run this checklist whenever a new quarter is added to the workbook. ✅ = passed for the current data (Q1 2022 and Q1 2023); ⚠️ = passed with a documented exception.

## Structure

- [x] ✅ Sheet is named `Sales` and has the 15 expected columns
- [x] ✅ Headers load without leading or trailing spaces (trimmed automatically on import)
- [x] ✅ Each row is one order line, with a unique `Product ID`
- [x] ✅ Row count matches the source system export (245 lines)

## Completeness

- [x] ✅ No blank values in any of the 15 fields
- [x] ✅ Every line has a payment method (Credit Card or PayPal)
- [x] ✅ Every line has a status (Shipped, Processing or Cancelled)

## Validity

- [x] ✅ Order dates fall inside the reporting quarters
- [x] ✅ Prices are positive (retail $150–$3,700, wholesale $90–$2,625)
- [x] ✅ Quantities are positive whole numbers (1–3)
- [x] ⚠️ Retail price is at or above wholesale price. **Exception:** 1 line (EnduroMaster 2000, 28 Mar 2023, retail $1,600 vs wholesale $1,872), kept as recorded.
- [x] ✅ Each product name maps to exactly one category

## Uniqueness

- [x] ✅ No fully duplicated rows
- [x] ⚠️ No lines identical apart from Product ID. **Exception:** 2 pairs (ReclineRider 1000 on 23 Feb 2023, TrailBlazer 2000 on 2 Mar 2023), kept as recorded.
- [x] ⚠️ Order ID identifies a single order. **Exception:** Order IDs group lines across dates and years, so fulfilment is measured in order lines.

## Model checks after refresh

- [x] ✅ `Date` table covers every order date and is marked as the date table
- [x] ✅ Revenue, Profit and Tax Due are 0 for cancelled lines
- [x] ✅ Tax Due is 5% of Revenue for lines over $2,000 and 0 otherwise
- [x] ✅ Customer Bill = Revenue + Tax Due
- [x] ✅ Q1 2023 totals reconcile: Revenue $337,600 · Profit $103,200 · Tax Due $13,915 · Order Value $435,100
- [x] ✅ Footer shows the new "Last refreshed" date
