# Online Banking Data Analysis Project

## Project Overview

This project is focused on performing a comprehensive analysis of an online banking system's customer and transaction data. The analysis addresses several key performance indicators (KPIs) and answers important business questions based on the provided customer joining and transaction datasets.

## Dashboard Overview (Power BI)

![Online Banking System Dashboard](https://github.com/FahimS/Online-Banking-Data-Analysis/blob/main/online_banking.jpg)

You can download and explore the interactive Power BI dashboard for this project [online banking](https://github.com/FahimS/Online-Banking-Data-Analysis/blob/main/dw_online_banking.pbix).


## Project Structure

The project consists of the following database tables:

### 1. **Region**
This table stores the regions where the bank operates:
- **region_id**: Integer (Primary Key)
- **region_name**: Name of the region (e.g., Dhaka, Khulna)

```sql
CREATE TABLE region (
  region_id INTEGER,
  region_name VARCHAR(9)
);

INSERT INTO region
  (region_id, region_name)
VALUES
  (1, 'Dhaka'),
  (2, 'Khulna'),
  (3, 'Rajshahi'),
  (4, 'Sylhet'),
  (5, 'Barishal');
```

### 2. **Area**
This table defines different customer areas:
- **area_id**: Integer (Primary Key)
- **name**: The name of the area (e.g., Union, Upazila, Pouroshova, etc.)

```sql
CREATE TABLE area (
  area_id INTEGER,
  name VARCHAR(20)
);

INSERT INTO area
  (area_id, name)
VALUES
  (1, 'Union'),
  (2, 'Upazila'),
  (3, 'Pouroshova'),
  (4, 'Ward'),
  (5, 'Village');
```

### 3. **Customer Joining Information**
This table contains data about customer joining:
- **customer_id**: Integer (Primary Key)
- **region_id**: Integer (Foreign Key from the Region table)
- **area_id**: Integer (Foreign Key from the Area table)
- **join_date**: Date the customer joined

```sql
-- You will get the data from Excel (No need to execute the below DDL)
CREATE TABLE customer_joining_info(
  customer_id INTEGER,
  region_id INTEGER,
  area_id INTEGER,
  join_date DATE
);
```

### 4. **Customer Transactions**
This table holds the transaction history for each customer:
- **customer_id**: Integer (Foreign Key)
- **txn_date**: Date of the transaction
- **txn_type**: Transaction type (e.g., Deposit, Withdrawal)
- **txn_amount**: Transaction amount in integer format

```sql
-- You will get the data from Excel (No need to execute the below DDL)
CREATE TABLE customer_transactions (
  customer_id INTEGER,
  txn_date DATE,
  txn_type VARCHAR(10),
  txn_amount INTEGER
);
```

## Key Performance Indicators (KPIs)

This project addresses the following business questions through SQL queries:

1. **Unique Customers**  
   How many unique customers exist in the database?

2. **Customers by Region**  
   How many unique customers are coming from each region?

3. **Customers by Area**  
   How many unique customers are coming from each area?

4. **Transaction Amount by Type**  
   What is the total transaction amount for each transaction type (e.g., Deposit, Withdrawal)?

5. **Customers with Multiple Transactions**  
   For each month, how many customers made more than 1 deposit and 1 withdrawal in a single month?

6. **Customer Closing Balance**  
   What is the closing balance for each customer?

7. **Monthly Closing Balance**  
   What is the closing balance for each customer at the end of each month?

8. **Latest Withdrawal Amounts**  
   What is the total withdrawal amount for the latest 5 days of transaction data?

9. **Five-Day Deposit Totals**  
   Calculate the total deposit amount for every five consecutive days. Assume a week consists of 5 days.

10. **Week-on-Week Deposit Comparison**  
    Compare the total deposit amounts of consecutive weeks by calculating the difference between one week and the next.

## Data Sources

The datasets used for this project are as follows:
- **Customer Joining Information**: `customer_joining_info.csv`
- **Customer Transactions**: `customer_transactions.csv`

## Technologies Used
- **MySQL**: For database creation and querying the datasets
- **Power BI**: For data visualization
- **Python**: For additional data processing (ETL)



## Key Performance Indicators (KPIs) Solutions

Here are the SQL queries used to solve the KPIs defined in the project.

### 1. How many unique customers are there?

```sql
SELECT COUNT(DISTINCT customer_id) AS unique_customers
FROM customer_joining_info;
```

### 2. How many unique customers are coming from each region?

```sql
SELECT r.region_name, COUNT(DISTINCT customer_id) AS unique_customers
FROM customer_joining_info cj
JOIN region r USING (region_id)
GROUP BY r.region_name;
```

### 3. How many unique customers are coming from each area?

```sql
SELECT name AS area_name, COUNT(DISTINCT customer_id) AS unique_customers
FROM customer_joining_info cj
JOIN area USING (area_id)
GROUP BY area_name;
```

### 4. What is the total amount for each transaction type?

```sql
SELECT txn_type AS Transaction_Name, SUM(txn_amount) AS Total_Amount
FROM customer_transactions
GROUP BY txn_type;
```

### 5. For each month - how many customers make more than 1 deposit and 1 withdrawal in a single month?

```sql
SELECT DATE_FORMAT(txn_date, '%Y-%m') AS txn_month,
       COUNT(customer_id) AS Customer_count
FROM customer_transactions
GROUP BY txn_month
HAVING COUNT(CASE WHEN txn_type = 'deposit' THEN 1 ELSE 0 END) > 1
   AND COUNT(CASE WHEN txn_type = 'withdrawal' THEN 1 ELSE 0 END) > 1;
```

### 6. What is the closing balance for each customer?

```sql
SELECT customer_id,
       SUM(CASE WHEN txn_type = 'deposit' THEN txn_amount ELSE 0 END) -
       SUM(CASE WHEN txn_type = 'withdrawal' THEN txn_amount ELSE 0 END) AS closing_balance
FROM customer_transactions
GROUP BY customer_id;
```

### 7. What is the closing balance for each customer at the end of the month?

```sql
SELECT customer_id,
       DATE_FORMAT(txn_date, '%Y-%m') AS transaction_month,
       SUM(CASE WHEN txn_type = 'deposit' THEN txn_amount ELSE 0 END) -
       SUM(CASE WHEN txn_type = 'withdrawal' THEN txn_amount ELSE 0 END) AS closing_balance
FROM customer_transactions
GROUP BY customer_id, transaction_month;
```

### 8. Please show the latest 5 days total withdraw amount.

```sql
SELECT txn_date,
       SUM(txn_amount) AS total_withdrawal
FROM customer_transactions
WHERE txn_type = 'withdrawal'
GROUP BY txn_date
ORDER BY txn_date DESC
LIMIT 5;
```

### 9. Find out the total deposit amount for every five days consecutive series. You can assume 1 week = 5 days.

```sql
WITH ranked_transactions AS (
  SELECT txn_date,
         SUM(txn_amount) AS total_amount,
         ROW_NUMBER() OVER (ORDER BY txn_date) AS row_num
  FROM customer_transactions
  WHERE txn_type = 'deposit'
  GROUP BY txn_date
)
SELECT CEIL(row_num / 5) AS week_number,
       SUM(total_amount) AS total_deposit
FROM ranked_transactions
GROUP BY week_number
ORDER BY week_number;
```

### 10. Compare every week's total deposit amount with the previous week.

```sql
WITH ranked_transactions AS (
  SELECT txn_date,
         SUM(txn_amount) AS total_amount,
         ROW_NUMBER() OVER (ORDER BY txn_date) AS row_num
  FROM customer_transactions
  WHERE txn_type = 'deposit'
  GROUP BY txn_date
), weekly_deposits AS (
  SELECT CEIL(row_num / 5) AS week_number,
         SUM(total_amount) AS total_deposit
  FROM ranked_transactions
  GROUP BY week_number
)
SELECT week_number,
       total_deposit,
       LAG(total_deposit) OVER (ORDER BY week_number) AS previous_week_deposit,
       total_deposit - LAG(total_deposit) OVER (ORDER BY week_number) AS deposit_difference
FROM weekly_deposits
ORDER BY week_number;
```
