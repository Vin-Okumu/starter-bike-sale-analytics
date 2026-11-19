## New Calculated Columns

These are business-friendly columns created in Power Query or Dax

1. Revenue - calculated in DAX
    
    syntax: Revenue = IF(
                NOT('Sales'[Order Status] = "Cancelled"),
                'Sales'[Retail Price] * 'Sales'[Order Quantity],0)
- Calculates the revenue by multiplying the order quantity by retail price fororders that were not cancelled.

2. Profit - calculated in DAX 
    
    syntax: Profit = (
                'Sales'[Retail Price] -
                'Sales'[Wholesale Price]
            ) * 'Sales'[Order Quantity]

3. Tax Due - calculated in DAX
    
    syntax: Tax Due = IF(
                'Sales'[Revenue] > 2000, 
                'Sales'[Revenue] * 0.05, 0 )

- Imposes a 5% tax for orders valued above $2000

4. Order Value - calculated in DAX 
    
    syntax: Order Value = 
                'Sales'[Retail Price] * 'Sales'[Order Quantity]

- Multiplies the retail price by order quantity regardless of whether order is cancelled

5. Customer Bill - calculated in DAX
    
    syntax: Customer Bill = 'Sales'[Revenue] + 
                            'Sales'[Tax Due]

- Adds the tax imposed on sale to the order value for the amount payable by customer.



