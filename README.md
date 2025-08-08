# Restaurant-Order-Analysis. SQL
The Taste of the World Café debuted a new menu at the start of the year and you’ve been asked to dig into the customer data to see which menu items are doing well versus those not doing well and what the top customers seem to like best.

## Table of Contents
- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools Used](#tools-used)
- [Data Preparation And Cleaning](#data-preparation-and-cleaning)
- [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)
- [Data Analysis](#data-analysis)
- [Results/Findings](#resultsfindings)
- [Recommendations](#recommendations)
- [Limitations](#limitations)
- [References](#references)


## Project Overview
This project focuses on analyzing restaurant order data to extract business insights that can guide operational and strategic decisions. Using a relational database with menu items and order details, the goal was to understand menu composition, sales trends, customer preferences, and high-revenue orders.

The analysis covers:
- Menu structure and pricing distribution
- Popularity of dishes and categories
- High-spending orders and their composition
- Order volume patterns

## Data Sources
The dataset was sourced from the Maven Analytics platform.
It consists of two primary tables:
1. menu_items – Contains metadata on menu offerings, categories, and prices.
2. order_details – Contains transactional order-level details including order date, time, and items purchased.

## Tools Used
1. SQL (MySQL) – For all querying, aggregation, and data joins.
2. MySQL Workbench – As the primary interface for executing SQL queries and inspecting data.
  
## Data Preparation and Cleaning
- Database Creation – Defined schema restaurant_db with menu_items and order_details tables.
- Primary Keys – Ensured unique identifiers (menu_item_id and order_details_id) for referential integrity.
- Data Type Verification – Dates stored as DATE, times as TIME, and prices as DECIMAL.
- Missing Value Checks – Identified and noted null values in item_id in the order_details table (possible cancelled or incomplete entries).
- Join Keys Validation – Confirmed all item_id values in order_details correspond to valid menu_item_id entries in menu_items.

## Exploratory Data Analysis (EDA)
Key EDA steps:
- **Menu Composition** – Counted total menu items and breakdown by category.

- **Price Analysis** – Identified the least and most expensive dishes overall and within each cuisine type.

- **Order Timeline** – Established the date range of orders and quantified order and item volumes.

- **Item Popularity** – Ranked dishes by total purchases.

- **Order Size Distribution** – Counted items per order and identified large orders (>12 items).

## Data Analysis
Advanced SQL queries were used to:
- Join datasets/tables
```
SELECT  MI.item_name, MI.category, MI.price, MI.menu_item_id, OD.item_id, 
OD.order_details_id, OD.order_id, OD.order_date, OD.order_time 
FROM menu_items MI JOIN order_details OD ON OD.item_id = MI.menu_item_id;
```
- 
## Results/Findings

## Recommendations

## Limitations

## References




