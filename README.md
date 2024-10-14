# Online Banking Data Analysis Project

## Project Overview

This project is focused on performing a comprehensive analysis of an online banking system's customer and transaction data. The analysis addresses several key performance indicators (KPIs) and answers important business questions based on the provided customer joining and transaction datasets.

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
- **SQL**: For database creation and querying the datasets
- **Power BI**: For data visualization (optional)
- **Python**: For additional data processing (optional)

## Next Steps
The solutions to these business queries will be added in a future update to this repository. Stay tuned for the detailed SQL queries and analysis.

