-------Retail Sales Analysis(SQL Project)--------
Project Overview:
Title:
       Retail Sales Analysis
DataBase:
      retail_analysis

Objective:
      1) Understand Sales Performance Over Time : Analyze sales trends over months to identify high-performing periods.
      2) Evaluate Product Category Effectiveness : Determine which categories generate the highest revenue.
      3) Analyze Customer Demographics : Understand the impact of gender and age on purchasing behavior.
      4) Data Quality and Integrity Check :  Identify and handle missing or incorrect values.


Project Structure: 
1) Database Creation and Table Creation:
     create database retail_analysis;

     create table Retail_analysis(
     transactions_id int primary key,sale_date text,sale_time time,
     customer_id int,gender varchar(50),age text,
     category varchar(50),quantiy text,
     price_per_unit text,cogs text,
     total_sale text);

2) Access Data (from datasets)
     LOAD DATA INFILE 'C:\\ProgramData\\MySQL\\MySQL Server 8.0\\Uploads\\SQL - Retail Sales Analysis_utf .csv'
     INTO TABLE retail_analysis
     FIELDS TERMINATED BY ','
     ENCLOSED BY '"'
     LINES TERMINATED BY '\n'
     IGNORE 1 ROWS;


Data Analysis & Findings:
    The following SQL queries were use to solved real world business problems.

   
3) Determine total count of records in table:
-->  select count(*) from retail_analysis;
      
4) Find how many unique customers are in the dataset:
-->  select  count(distinct customer_id) from retail_analysis;

5) Identify all unique product category in the dataset:
-->  select distinct category from retail_analysis;

6) check for any value is null if present then delete that record from table:
--> select COUNT(*) from retail_analysis 
    where 
    transactions_id IS NULL OR sale_date IS NULL 
    OR sale_time IS NULL OR customer_id IS NULL 
    OR gender IS NULL OR age IS NULL 
    OR  category IS NULL OR quantiy IS NULL 
    OR price_per_unit IS NULL OR cogs IS NULL OR total_sale IS NULL;


--> DELETE FROM retail_analysis
    WHERE 
    transactions_id IS NULL OR sale_date IS NULL 
    OR sale_time IS NULL OR customer_id IS NULL 
    OR gender IS NULL OR age IS NULL 
    OR  category IS NULL OR quantiy IS NULL 
    OR price_per_unit IS NULL OR cogs IS NULL OR total_sale IS NULL;


8) write a SQL query to retrive all column for sales made on '10/31/2022': 
--> SELECT * FROM retail_analysis where sale_date='10/31/2022';

9) Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022:
--> SELECT * FROM retail_analysis
    WHERE category='Clothing' AND date_format(sale_date,'%m-%Y')='May-2022' AND quantiy>=2 ;

10)  Write a SQL query to calculate the total sales (total_sale) for each category.:
--> select category,
    sum(total_sale) as net_sales ,
    count(*) as Order_Count from retail_analysis group by 1;

12) Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.:
--> select Round(avg(age),2)
    as Average_Age from retail_analysis
    where category='Beauty';

13) Write a SQL query to find all transactions where the total_sale is greater than 1000.:*/
--> select * from retail_analysis where total_sale>1000;

14) Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.:
--> select category, gender, count(transactions_id)
    as Transaction from retail_analysis
    group by category,gender order by 1;

15) Writw an SQL Query to calculate the Avarage sale for each month. Find out best selling month in each tear.:
--> SELECT year, month, total_sale AS avg_sale
     FROM (
     SELECT 
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale) AS total_sale,
        RANK() OVER (
            PARTITION BY EXTRACT(YEAR FROM sale_date)
            ORDER BY AVG(total_sale) DESC
        ) AS rar
     FROM retail_analysis
     GROUP BY EXTRACT(YEAR FROM sale_date), EXTRACT(MONTH FROM sale_date)
     ) AS t1
     WHERE rar = 1;

16) Write a SQL query to find the top 5 customers based on the highest total sales:
--> select customer_id, sum(total_sale) as
    Total_Sales from retail_analysis group by 1
    order by Total_Sales desc limit 5;

17) Write a SQL query to find the number of unique customers who purchased items from each category.:
--> select category, count(distinct customer_id)
      as Unique_Customer from retail_analysis group by category;

18) Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17):*/
--> WITH hourly_sale
    AS
    (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END as shift
    FROM retail_analysis
    )
    SELECT 
        shift,
        COUNT(*) as total_orders    
    FROM hourly_sale
    GROUP BY shift;










    
