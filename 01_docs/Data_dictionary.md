## New Calculated Columns

These are business-friendly columns created in Power Query or Dax

1. Revenue - calculated in DAX
    - Calculates the revenue by multiplying the order quantity by retail price fororders that were not cancelled.

2. Profit - calculated in DAX 
    - Calculates the profit by multiplying the difference between retail and wholesale price by the order quantity.

3. Tax Due - calculated in DAX
    - Imposes a 5% tax for orders valued above $2000 and that are not cancelled. Only completed orders are taxable

4. Order Value - calculated in DAX 
    - Multiplies the retail price by order quantity regardless of whether order is cancelled. Gives perspective of the value of orders, whether processed or not.

5. Customer Bill - calculated in DAX
    - Adds the tax imposed on sale to the order value for the amount payable by customer.



