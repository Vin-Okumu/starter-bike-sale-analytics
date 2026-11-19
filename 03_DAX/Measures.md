## Measures created

1. Quantity Sold - Aggregates all the bikes sold, both shipped and pending. Calculated in DAX from the syntax:
    
    Quantity Sold = CALCULATE(
            SUM('Sales'[Order Quantity]),
            'Sales'[Order Status] <> "Cancelled"
    )

2. All Orders - Aggregates all orders recorded. Calculated in DAX using the syntax:
    
    All Orders = DISTINCTCOUNT('Sales'[Product ID])

3. Total Revenue - Calculates the total revenue, only from processed and pending orders. Calculated in DAX using the syntax:
    
    Total Revenue = SUM('Sales'[Revenue])

4. Net Revenue - Calculates the revenue remaining after tax. Calculated in DAX using the syntax: 
    
    Net Revenue = [Total Revenue] - [Total Tax Due]

5. Total Profit - Calculates the total profit from sales, meaning only processed and pending orders. Calculated in DAX using the syntax:
    
    Total Profit = SUM('Sales'[Profit])

6. Total Tax Due - Calculates the total tax to be remitted by company from fulfilled and pending orders. Calculated in DAX using the syntax:
    
    Total Tax Due = SUM('Sales'[Tax Due])

7. Average Order Value - Calculates the average value of orders made through the select period. Calculated in DAX using the syntax:
    
    Average Order Value = DIVIDE(
                [Total Revenue],[Total Orders]
    )

8. Total Shipped Orders - Aggregates all orders completed through selected period. Calculated in DAX using the syntax: 
    
    Total Shipped Order = (
                CALCULATE(
                 COUNTROWS('Sales'),
                'Sales'[Order Status] = "Shipped")
    )

9. Total Orders Under Process - Aggregates all pending orders through selected period. Calculated in DAX using the syntax: 
    
    Total Orders Under Process = 
                CALCULATE(
                COUNTROWS('Sales'),
                'Sales'[Order Status] = "Processing")

10. Total Cancelled Orders - Aggregates all cancelled orders through filter period. Calculated in DAX using the syntax: 
    
    Total Cancelled Order = 
                CALCULATE(
                COUNTROWS('Sales'),
                'Sales'[Order Status] = "Cancelled")

11. Shipped Orders - Calculates the proportion of all orders that were shipped. Calculated in DAX using the syntax: 
    
    Shipped Orders = DIVIDE (
                [Total Shipped Order],[All Orders])

12. Under Process - Calculates the proportion of all orders that were still pending at timeof reporting. Calculated in DAX using the syntax:
    
    Under Processing = DIVIDE (
                [Total Orders Under Process],[All Orders])

13. Cancelled Orders - Calculates the proportion of orders cancelled at time of reporting. Calculated in DAX using the syntax:
    
    Cancelled Orders = DIVIDE (
                [Total Cancelled Order],[All Orders])

14. Taxable Sale - Aggregates all orders for which the 5% tax is imposed. Calculated in DAX using the syntax: 
    
    Taxable Sale = CALCULATE(
                COUNTROWS('Sales'),
                'Sales'[Tax Due] > 0)

15. Taxable Sale Percent - Calculates the proportion of taxable orders as a percentage of all orders. Calculated in DAX using the syntax:
    
    Taxable Sale Percent = DIVIDE(
                CALCULATE(
                COUNTROWS('Sales'),'Sales'[Tax Due] > 0),
                CALCULATE(
                    COUNTROWS('Sales'),'Sales'[Product ID]
                )
    )

16. Tax Proportion - Calculates tax due as a percentage of total revenue. Calculated in DAX using the syntax:
    
    Tax Proportion = DIVIDE(
                [Total Tax Due],[Total Revenue]
    )

17. Total Orders - Aggregates all orders made regardless of whether the order was cancelled. Calculated in DAX using the syntax:
    
    Total Orders = DISTINCTCOUNT('Sales'[Order ID])

18. Customers - Aggregates all customers served by the business, regardless of whether they cancelled their order. Calculated in DAX using the syntax:
    
    Customers = DISTINCTCOUNT('Sales'[Customer ID])
