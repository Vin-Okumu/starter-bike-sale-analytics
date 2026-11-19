## ETL Documentation

    Step | Transformation                        | Description
    -----|---------------------------------------|------------------
    1    | Imported Excel Workbook               |
    2    | Verified rows and column counts       |
    3    | Standardized column names             |
    4    | Added calculated Revenue Column       | Revenue = Retail Price * Order Quantity 
    5    | Added calculated Tax Due Column       | If Revenue > 2000, Tax Due = Revenue * 5%
    6    | Added calculated Customer Bill column | Customer Bill = Revenue + Tax Due
    7    | Added calculated Profit column        | Profit = (Retail Price -Wholesale Price) * Order Quantity
    8    | Added calculated Order Value column   | Order Value = Retail Price * Order Quantity
    9    | Added calculated Tax Proportion column| Tax Proportion = Tax Due/Revenue
    


   
   