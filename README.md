# MintClassics-Vehicle-Car-Report-Analysis
## Problem Statement:
Mint Classics Company, a retailer of classic model cars and other vehicles, is looking at closing one of their storage facilities. To support a data-based business decision, they are looking for suggestions and recommendations for reorganizing or reducing inventory, while still maintaining timely service to their customers. For example, they would like to be able to ship a product to a customer within 24 hours of the order being placed.

1) Where are items stored and if they were rearranged, could a warehouse be eliminated?
2) How are inventory numbers related to sales figures? Do the inventory counts seem appropriate for each item?
3) Are we storing items that are not moving? Are any items candidates for being dropped from the product line?
4) Identify patterns. Are some items sold more often than others? Does the price seem to affect sales? 
5) Conduct what-if analysis. For example, what impact would it have on the company and customer service if we reduced the quantity on hand for every item by 5%?
6) 
## Datasource and Preparation
Data set for this task was provided by Coursersa Project Netwok. mintclassicsDB.sql a SQL Dump file was provided that contains the script to create and populate data tabled named as:-

- customers
- employees
- offices
- Order
- Orderdetails
- payements
- productlines
- products
- warehouses
  
Import provided SQL File into MySQL Workbench CE 8.0. Run SQL queries to record no of rows in each table. This helped me to see larger picture before going to granule level of data.

1.	Products  records count=110
2.	Warehouses records count=4
3.	Productlines record count=7
4.	Orderdetails record count=2996
5.	Orders record count=326
6.	customers record count=122
7.	payments record count=273
8.	employees record count=23
9.	offices record count=7

Steps performed for Data transformation in Power Query after the dataset loaded into Microsoft Power BI Desktop:
- Each of the columns in the table were validated to have the correct data type for all 5 query tables
- Created date hierarchy in model view in date table.
- Filtered out missing values row from data set.
- Changed the currency type by changing the currency locale.
- Postal code column in customer table has error may drop
- Fill Null values in State column with actual values in Mysql Customer table
- SalesRepEmployeenumber – some values are null that needs to be delted from power query.
- Last name and first name can be combine in employee table.


## Data Modeling
## Data Analysis (DAX)

1.	Calculated Cols and measure in OREDER table: 
-	```Balance = 'order'[Revenue realized]-payments[Revenue collected]```
-	```Revenue realized = SUMX('order','order'[quantityOrdered]*'order'[priceEach])```
-	```Stock ratio = DIVIDE([Revenue realized],cost) ```
-	```Total Cost = SUM('order'[buyPrize])```
-	```Total Orders = DISTINCTCOUNT('order'[orderNumber])```
-	```Total Profit = SUM('order'[Profit])```
-	```Total Profit% = DIVIDE(([Revenue realized]-[Total cost]),[Revenue realized])```
-	``` total sales qty = SUM('order'[quantityOrdered])```
-	``` buyPrize = RELATED(products[buyPrice])```
-	``` Profit = 'order'[priceEach]-'order'[buyPrize]```
-	``` Profit% = DIVIDE(('order'[priceEach]-'order'[buyPrize]),'order'[priceEach],2)```
-	``` Revenue collected = SUM(payments[amount])```

2.	Calculated Cols and measure in PRODUCTS table: 
-	``` Total Stock = SUM(products[quantityInStock])```
-	``` Cost of stock = [buyPrice]*[quantityInStock]```
-	``` Cost of stock new = products[Cost of stock] * (1+[Stock change Value]/100)```
-	``` MSRP of Stock = [MSRP]*[quantityInStock]```
-	``` Profit margin = (products[MSRP of Stock]-products[Cost of stock])```


3.	Measures for what if analysis: 
-	``` Stock change = GENERATESERIES(-5, 5, 5)```
-	``` Stock change Value = SELECTEDVALUE('Stock change'[Stock change], 0)```
-	``` total sales qty new = [total sales qty]*(1+'Stock change'[Stock change Value]/100)````
-	``` Revenue realized new = [Revenue realized]*(1+'Stock change'[Stock change Value]/100)```
-	``` Total Profit new = [Total Profit]*(1+'Stock change'[Stock change Value]/100)```

## Dashboard (Data Visualization)
### Warehouse Analysis
![MintClassics_page-0001](https://github.com/AnjaliM-9/MintClassics-Vehicle-Car-Report-Analysis/assets/155083462/4b888256-6f09-45a8-b9c2-5e36a6b9a379)

### Sales Pattern
![MintClassics_page-0002](https://github.com/AnjaliM-9/MintClassics-Vehicle-Car-Report-Analysis/assets/155083462/71395b47-77a7-4f40-981d-1d1c6d15698b)

### Inventory analysis
![MintClassics_page-0003](https://github.com/AnjaliM-9/MintClassics-Vehicle-Car-Report-Analysis/assets/155083462/8419890f-bf8b-437d-8e50-837f8f6cc884)

### What-if analysis
![MintClassics_page-0004](https://github.com/AnjaliM-9/MintClassics-Vehicle-Car-Report-Analysis/assets/155083462/5cc3a448-caf1-47ef-a0c9-d840c69e54a4)

## Insights
- Stakeholders may consider to reshuffle vintage car product line. 
-	Inventory analysis suggest that there are some products under vintage car product line that aren’t moving or not in demand 1932 Alfa romeo8c2300 and 1932 Model A Ford j-coupe.
-	Classic cars is best selling category however top 10 analysis doesn’t specifically only show one category product as best selling.
-	In cluster analysis , USA , Spain and France are countries that generated most of revenue and profits.
## Further Analysis
I realized that realworld database or dateset is larger in terms of no. of records and ongoing in nature.. doing predictive analysis especially of sales (weekly,monthly,quarterly) could be done to provide realtime insights to stakeholders.Soon will add sql query analysis snapshots.
  


