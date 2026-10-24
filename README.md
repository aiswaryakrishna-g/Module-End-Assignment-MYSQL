# E-Commerce Customer Churn Analysis (MySQL)

**Author:** Aiswarya Krishna G
**Tool:** MySQL Workbench (MySQL 8)
**Assignment:** Module End Assignment 2 - MySQL

## Overview

Customer churn (customers who stop buying) is one of the biggest threats to an e-commerce business. This project uses SQL to clean and analyse historical customer data and uncover what is linked to churn: tenure, payment mode, satisfaction score, complaints, distance from the warehouse, and purchase behaviour. The goal is to give the business insights it can use for targeted retention strategies.

## Dataset

- Database: `ecomm`
- Table: `customer_churn` (one row per customer, 20 columns)
- Key columns: `CustomerID`, `Churn`, `Tenure`, `PreferredLoginDevice`, `CityTier`, `WarehouseToHome`, `PreferredPaymentMode`, `Gender`, `HourSpendOnApp`, `NumberOfDeviceRegistered`, `PreferedOrderCat`, `SatisfactionScore`, `MaritalStatus`, `NumberOfAddress`, `Complain`, `OrderAmountHikeFromlastYear`, `CouponUsed`, `OrderCount`, `DaySinceLastOrder`, `CashbackAmount`
- The raw data contains missing values, outliers and inconsistent labels, which are fixed in the cleaning step.

## Project Structure

```
.
├── E-Commerce_Customer_churn_db.sql        # Creates the ecomm database and loads the raw data
├── churn_analysis_solution.sql             # Cleaning, transformation and analysis queries
├── Churn_Analysis_Step_by_Step_Guide.pdf   # Explanation of every step
└── README.md
```

## How to Run

1. Open MySQL Workbench and connect to your local server.
2. Open `E-Commerce_Customer_churn_db.sql` and run the whole script. This creates `ecomm` and `customer_churn`.
3. Open `churn_analysis_solution.sql` and run it in order (select all, then `Ctrl+Shift+Enter`).
4. Run the analysis queries one at a time (`Ctrl+Enter`) to see each result.

> Run the cleaning and transformation steps once only. To start over, re-run the original database script, which rebuilds everything from scratch.

## What Was Done

### 1. Data Cleaning
- Replaced missing values with the **mean** (rounded) in `WarehouseToHome`, `HourSpendOnApp`, `OrderAmountHikeFromlastYear` and `DaySinceLastOrder`.
- Replaced missing values with the **mode** in `Tenure`, `CouponUsed` and `OrderCount`.
- Removed outliers: rows with `WarehouseToHome` greater than 100.
- Standardised labels: `Phone` and `Mobile` became `Mobile Phone`; `COD` became `Cash on Delivery`; `CC` became `Credit Card`.

### 2. Data Transformation
- Renamed `PreferedOrderCat` to `PreferredOrderCat` and `HourSpendOnApp` to `HoursSpentOnApp`.
- Created `ComplaintReceived` (Yes/No) from `Complain` and `ChurnStatus` (Churned/Active) from `Churn`.
- Dropped the original `Churn` and `Complain` columns.

### 3. Data Exploration and Analysis
Seventeen business questions were answered with SQL, covering:

- Churned vs active customer counts
- Average tenure and total cashback of churned customers
- Share of churned customers who complained
- City tier, payment mode and gender patterns
- Coupon usage and cashback by order category
- Churn breakdown by distance from the warehouse (`CASE` categories)
- Married City Tier 1 customers with above-average order counts
- A new `customer_returns` table joined with customer data to show churned customers who complained

## SQL Concepts Used

`UPDATE` and `DELETE`, `ALTER TABLE` (add, rename, drop), subqueries and derived tables, user variables, `GROUP BY` and `HAVING`, aggregate functions (`COUNT`, `SUM`, `AVG`, `MAX`), `CASE`, `IF`, `ORDER BY ... LIMIT`, `DISTINCT`, `CREATE TABLE`, `INSERT`, `JOIN`.

## Key Insights

*Add your own findings here after running the queries, for example:*

- Churn rate overall: ...
- Payment mode most common among active customers: ...
- Effect of complaints on churn: ...
- Effect of distance from the warehouse on churn: ...

## Screenshots

*Add screenshots of your query outputs here, for example `![Churn count](screenshots/q1.png)`.*

## Learnings

- Cleaning data first (nulls, outliers, inconsistent labels) is essential, because messy values split groups and distort averages.
- MySQL has no `MODE()` function and blocks updates that read the same table directly, so variables and derived tables are needed.
- Conditions on aggregated values belong in `HAVING`, not `WHERE`.
- Run order matters: new columns must be filled before the source columns are dropped.
