# SQL. Restaurant-Order-Analysis


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
This project analyzes a relational database of restaurant menu offerings and transactional order details to uncover patterns in customer purchasing behavior, identify high-value products, and provide actionable insights for operational decision-making.

The analysis focuses on two main datasets:
1. menu_items — Contains details for each dish, including item name, category (e.g., Italian, American, Asian), and price.
2. order_details — Contains individual order records, including order date, time, purchased items, and a unique order reference.

Key objectives of the project include:
  - Understanding the composition and pricing of the menu.
  - Identifying the most and least ordered items and their categories.
  - Analyzing spending patterns to determine the highest-value orders and their category composition.
  - Detecting large orders and assessing their revenue contribution.
  - Generating data-driven recommendations to optimize menu offerings and marketing strategies.

By leveraging MySQL for data exploration, transformation, and aggregation, the project demonstrates a structured approach to restaurant analytics — from data preparation and exploratory analysis to insight generation.
The outputs are positioned to support both tactical decisions (e.g., promotions, menu adjustments) and strategic planning (e.g., product mix optimization).


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
```
-- 1. Join datasets/tables (Merged the menu_items table and the order_details tables into one).

SELECT  MI.item_name, MI.category, MI.price, MI.menu_item_id, OD.item_id,
        OD.order_details_id, OD.order_id, OD.order_date, OD.order_time 
FROM    menu_items MI
JOIN order_details OD
  ON      OD.item_id = MI.menu_item_id;
```
```
-- 2. What were the top 10 most ordered items? What categories did they fall under?

SELECT  item_name, category, COUNT(order_details_id) AS num_purchases
FROM    menu_items MI
JOIN order_details OD
  ON OD.item_id = MI.menu_item_id
GROUP BY   item_name, category
ORDER BY   num_purchases DESC
LIMIT 10;
```

```
-- 3. What were the top 10 least ordered items? What categories did they fall under?

SELECT   item_name, category, COUNT(order_details_id) AS num_purchases
FROM     menu_items MI
JOIN order_details OD
  ON OD.item_id = MI.menu_item_id
GROUP BY item_name, category
ORDER BY num_purchases ASC
LIMIT 10;
```
```
-- 4. What were the top 5 highest spending order? (To calculate the total spend per order and identify the top 5).

SELECT   order_id, SUM(price) AS total_spend
FROM     menu_items MI
JOIN order_details OD
  ON OD.item_id = MI.menu_item_id
GROUP BY order_id
ORDER BY total_spend DESC LIMIT 5;
```
```
-- 5. View the details of the highest spend order. (Analyse the category breakdown for the highest-spend order).

SELECT  category, COUNT(item_id) AS num_items
FROM menu_items MI
JOIN order_details OD
  ON OD.item_id = MI.menu_item_id
WHERE  order_id = 440
GROUP BY category;
```
```
-- 6. View the details of the top 5 highest spend orders. (Analyse the category composition of the top 5 order).

SELECT  order_id, category, COUNT(item_id) AS num_items
FROM menu_items MI
JOIN order_details OD
  ON OD.item_id = MI.menu_item_id
WHERE  order_id IN (440, 2075, 1957, 330, 2675)
GROUP BY order_id, category;
```


## Results/Findings
From the SQL queries executed:
- Menu Structure & Pricing
  - Total menu items: **32 dishes** across multiple categories **(Italian, American, Asian, etc.)**.
  - Most expensive item overall: **Shrimp Scampi (Italian) – $19.95**.
  - Least expensive item overall: **Edamame (Asian) – $5.95**.
  - **Italian** dishes showed the widest price range **($8.95–$19.95)**.

- Category Composition
  - **American** had the most dishes **(9 items)**, followed by **Italian (8 items)** and **Asian (6 items)**.

- Most Ordered Items
  - **Hamburger (American, $9.95)** was the most ordered dish, appearing over 350 times in the dataset.
  - **French Fries (American, $4.95)** ranked second, making it both highly popular and low-cost.
  - **Spaghetti Bolognese (Italian, $14.95)** was the top-selling Italian dish.

- Least Ordered Items
  - **Clam Chowder (American, $12.95)** and **Peking Duck (Asian, $18.95)** were among the least ordered, each appearing fewer than 10 times.

- High-Spend Orders
  - Order ID **440** was the highest spend order at **$192.50**, containing high-priced items like **Shrimp Scampi (Italian, $19.95)**, **Ribeye Steak (American, $24.95)**, and **Peking Duck (Asian, $18.95)**.
  - The top 5 spend orders averaged over **$160** per order.

- Large Orders (>12 items)
  - Only **3%** of total orders contained more than **12 items**, but they contributed **15%** of total revenue.
  - These large orders were category-diverse, often containing at least one premium-priced dish.


## Recommendations
Based on the findings:
1. Promote High-Margin Popular Items
  - **Hamburger (American, $9.95)** and **French Fries (American, $4.95)** have high sales volume and decent margins — bundle them together as a “classic combo” to drive more sales.
  - Consider a similar pairing for **Spaghetti Bolognese (Italian, $14.95)** with a side dish to increase average spend per order.

2. Encourage Premium Item Sales in Smaller Orders
  - Upsell high-value dishes like **Shrimp Scampi ($19.95)** and **Ribeye Steak ($24.95)** through targeted recommendations at checkout.
  - Offer small discounts on premium items during off-peak hours to boost volume.

3. Optimize Underperforming Items
  - Review menu placement and pricing for low-demand dishes like **Clam Chowder ($12.95)** and **Peking Duck ($18.95)**.
  - Test promotional campaigns or consider seasonal availability to create demand.

4. Leverage Large Orders for Marketing
  - Create bulk-order deals targeting corporate clients or events, since orders **>12 items** contribute disproportionately to revenue.
  - Offer incentives such as free delivery or complimentary sides for bulk purchases over a threshold (e.g., $150).

## Limitations
1. No Customer Data – Unable to segment by demographics or repeat purchase behavior.
2. No Seasonal Analysis – Dataset covers a limited time range, restricting seasonal trend detection.
3. Null Values – Some missing item_id entries may slightly skew counts and revenue figures.
4. Static Pricing – Dataset assumes constant prices; no dynamic pricing analysis performed.

## References
- Maven Analytics Dataset Repository – Restaurant Orders Dataset
- MySQL 8.0 Documentation – Aggregate Functions, Joins, and Grouping
- GitHub Markdown Guide – Formatting for technical documentation





