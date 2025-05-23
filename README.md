# Sales Insights

## Problem Statement:
AtliQ Hardware is an Indian company specializing in production of computers and perepherals. The Sales Director of the company is facing challenges in tracking the sales in dynamically growing market across India. The Sales Director sees that the overall sales is declining but when communicated with the regional managers, they share vague insights verbally instead of showing the actual picture. Whenever the SD asks actual sales insights, the regional managers send him number of Excel files regarding sales which is simply hard to consume. Therefore, he decides to hire a data analyst to build the Dashboard in Power BI for the company to know the sales insights and make data-driven decisions.

## About AtliQ Hardware
- AtliQ Hardware is a computer and perepherals manufacturing company in India which manufactures PC, Laptops, Mouse, Keyboard, etc.
- AtliQ has regional offices in North India, Central India and South India headquarter in New Delhi.
- AtliQ's customers are companies like Excel Stores, Nomad Stores, Surge Stores, Electricalsara Stores across India.
- AtliQ's has customers on two Platforms
    - Brick and Mortar (Physical)
    - E-Commerce

## SQL Database dump file
- db_dump_version_2.sql
  
#### Tables in the Database
      - customers
      - dates
      - markets
      - products
      - transactions

## Steps for Project Execution
- Importing db_dump_version_2.sql dump file in MySQL
- Performing data analysis using SQL queries in MySQL for getting some insights from the database.
- Connecting Power BI to MySQL server for fetching the data from the database for doing ETL (Extraxt, Transform and Load).
- Reviewing the Database relationship created by Power BI by default and establishing new relationships between the tables.
- Data modelling is done by connecting different tables using a foreign key and primary key. In this project, Star Schema is used for Data Modelling where all the dimension tables are connected with Fact tables
- Using Power Query for data cleaning and transformation in Power BI.
- Creating calculated columns using more Power Query and DAX formulas (Formulas listed at the bottom). After the columns were created, verified them in either MySQL or Excel file.
- Starting creating the design work and building the dashboards. 
- Publishing the fully working dashboard to the Power BI service, providing access to pertinent teams to acquire critical business insights.

## Data Analysis using SQL
1. **Show all customer records**
   
``` sql 
SELECT *
FROM customers;                    
```

2. **Show total number of customers**                                        
```sql
SELECT count(*) as total_customers
FROM customers;
```
3. **Show transactions for Chennai market (market code for chennai is Mark001**

``` sql
SELECT * FROM transactions where market_code='Mark001';
``` 
4. **Show distrinct product codes that were sold in chennai**
``` sql
SELECT distinct product_code
FROM transactions
where market_code='Mark001';
``` 
5. **Show transactions where currency is US dollars**
``` sql
SELECT *
from transactions
where currency="USD";
``` 
6. **Show transactions in 2020 join by date table**                               
``` sql
SELECT transactions.*, date.*                                                                 
FROM transactions                                                    
INNER JOIN date ON transactions.order_date=date.date                                      
WHERE date.year=2020;
``` 

                                                                                                          
7. **Show total revenue in year 2020**                                   
``` sql
SELECT SUM(transactions.sales_amount)                                                                                   
FROM transactions INNER JOIN date ON transactions.order_date=date.date                                       
WHERE date.year=2020 and transactions.currency="INR\r" or transactions.currency="USD\r";
```
                                  
8. **Show total revenue in year 2020, January Month**                                       
``` sql
SELECT SUM(transactions.sales_amount)                                      
FROM transactions INNER JOIN date ON transactions.order_date=date.date                                          
WHERE date.year=2020 and and date.month_name="January" and (transactions.currency="INR\r" or transactions.currency="USD\r");
```
                                
9. **Show total revenue in year 2020 in Chennai**                                                    
``` sql
SELECT SUM(transactions.sales_amount)                                               
FROM transactions INNER JOIN date ON transactions.order_date=date.date                                                       
WHERE date.year=2020 and transactions.market_code="Mark001";
```                                                   

## Created Data Model View
![Model View](https://github.com/user-attachments/assets/2f76c989-d6c2-4e66-bc53-41af1b8f4090)

## Data Transformation in Power Query
- Creating new Cloumn in transaction table                                                                       
    - Table.AddColumn(sales_transactions, "norm_sales_amount", each if [currency] = "USD" then [sales_amount]*75 else [sales_amount])


## Created Dashboard
The created Dashboard basically contains 4 Views - Revenue View, Profit View, Customers View, and Performance View.

### Revenue View
![Revenue View](https://github.com/user-attachments/assets/bfdd2768-476c-4f83-a887-ea0a057ae976)

- From the Revenue View, we can see that 142.2 M was the revenue made by AtliQ in 2020 and 350 k Sales Quantity.
- The highest revenue was earned from Delhi NCR whilst Bhuwaneshwar was recorded the lowest.
- The highest sales was made in Delhi NCR while the lowest sales was in Patna.
- The Revenue Trend Chart shows decline in the Revenue earning by the company every coming month, this may be because of Covid outbreak.
- Electricalsara was the top customer giving highest revenue in 2020.
- The top revenue generating product from the View is blank, this is a an issue in data analytics industry, here we should talk to data engineers to do a backfilling in the missing data, then upon refresh, we will get that product.

### Profit View
![Profit View](https://github.com/user-attachments/assets/767bce8f-890e-4106-b2c0-8f7617793a9d)

- From Profit View, we can clearly see that Bhubaneshwar is giving the highest profit followed by Hyderabad and Chennai whilst Lucknow is a Loss making region in 2020.
- By market, Bhuwaneshwar is giving highest return but Mumbai is contributing the most in profit making across India which is nearly one quarter (example - If the company is making 100 $ profit across India then 24 $ is coming from Mumbai and 22 $ from Delhi) and then comes Delhi and Ahmedabad.
- AtliQ made total profit of 2.06 M in 2020 out of which 15.6 % is contributed by Electricalsbea Stores (highest profit) whilst, the lowest profit making customer is Electricalsquipo Stores which is negative 11 %.
- Excel Stores contributed the most towards profit making across India in 2020 i.e., 12.54 %
- Epic Stores is the most loos making customer in 2020.
- Electricalsara Stores is giving the profit of just 0.37 % but it is contributing 11.92 % towards total profit earned by India, here we can say that the company received excessive volume of orders from their consumers.

### Customers View
![Customers View](https://github.com/user-attachments/assets/34d9c9b8-7d89-4a41-b9fa-cbc75aed6000)

- Customers on Brick & Mortar platform are giving the highest revenue and sales quantity in every regions.
- Electricalsara Stores is the customer giving highest revenue of 65.6 M on Brick & Mortar platform in 2020. Also, this customer is making highest revenue in Delhi.
- Electricalslytical is the customer on E-Commerce platform giving highest revenue of 5.53 M in 2020. Also, this customer is making highest revenue in Mumbai.
- The customers presence is 80 % presence on Brick & Mortar platfrom whilst 20 % in E-Commerce platform in 2020.

### Performance View
![Performance View](https://github.com/user-attachments/assets/2384d193-9e30-45f7-a106-5c14e702806c)

- In 2020, South region/zone is making highest profit contribution, lowest is North.
- The profit % slider at the top indicates the target percentage of profi to be achieved. Currently it's set to 2 % profit, the bar for any regions or markets making profit below 2 % will turn red.
- Electricalsbea Stores shows makes more than 15 % profit contribution, whilst Electricalsquipo Stores made loss of more than 11 %.
- When drilled down to profit contribution by product then, we clearly see Prod151 giving 34 % profit across India in 2020 whilst product Product073 and Prod201 is the worst performing product which made loss of 25 % each.
- From the Revenue Trend chart we see that the revenue and Profit Margin % is declining every month when compared to the same time last year.
- Electricalsara Stores contributes nearly half of the revenue earned across India, it is the biggest customer for AtliQ. 

## Key Measures Created using DAX Formulas
1. **Revenue** = SUM('sales transactions'[norm_sales_amount])
2. **Sales Qty** = SUM('sales transactions'[sales_qty])
3. **Total Profit Margin** = SUM('sales transactions'[profit_margin])
4. **Profit Margin %** = DIVIDE(SUM('sales transactions'[profit_margin]),[Revenue],0)
5. **Profit Margin Contribution %** = DIVIDE([Total Profit Margin],CALCULATE([Total Profit Margin],ALL('sales markets'),ALL('sales customers'),ALL('sales products')))
6. **Revenue LY** = CALCULATE([Revenue],SAMEPERIODLASTYEAR('sales date'[date]))
7. **Revenue Contribution %** = DIVIDE([Revenue],CALCULATE([Revenue],ALL('sales markets'),ALL('sales customers'),ALL('sales products')))
8. **Profit Target** = GENERATESERIES(-0.05, 0.15, 0.01)
9. **Profit Target Value** = SELECTEDVALUE('Profit Target'[Profit Target])
10. **Target Diff** = [Profit Margin %]-'Profit Target'[Profit Target Value]
