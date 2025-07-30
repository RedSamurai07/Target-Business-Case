# Target Business Case study using SQL

## Table of contents
- [Project Overview](#project-overview)
- [Executive Summary](#executive-summary)
- [Goal](goal)
- [Data Structure](data-structure)
- [Tools](tools)
- [Data Analysis](#data-analysis)
- [Insights](insights)
- [Recommendations](recommendations)

### Project Overview

### Executive Summary

### Goal


### Data structure and initial checks
[Dataset](link)

 - The initial checks of your transactions.csv dataset reveal the following:

| Features | Description | Data types |
| -------- | -------- | -------- | 


### Tools
SQl: Big QueryStudio - Querying, manipulating, and managing data in relational databases 

### Data Analysis
1). SQL

I. Basic analysis
A. Get the time range between which the orders were placed. Hint: We want you to get the date & time when the first and last orders in our dataset were placed. 
Query: 
``` sql
SELECT
    MIN(order_approved_at) AS Start_date,
    MAX(order_approved_at) AS End_date
  FROM
  `target.orders`
```
Snapshots:

B. Count the Cities & States of customers who ordered during the given period. Hint: We want you to count the number of unique cities & states where orders were placed by the customers during the given time period. 
Query:
``` sql
SELECT
COUNT(DISTINCT customer_city) AS `cx_city`,
COUNT(DISTINCT customer_state) AS `cx_state`
FROM
`target.customers`
```
Snapshots:
______________________________________________________________________________ 
II. In-depth Exploration: 
A. Is there a growing trend in the no. of orders placed over the past years? Hint: We want you to find out if no. of orders placed has increased gradually in each month, over the past years. 
Query:
``` sql
WITH MonthlyOrderCounts AS ( 
 SELECT
    FORMAT_DATE('%B', order_purchase_timestamp) AS order_month,
    EXTRACT(MONTH FROM order_purchase_timestamp) AS order_months_1,
    EXTRACT(YEAR FROM order_purchase_timestamp) AS order_year,
    COUNT(order_id) AS order_count
  FROM `target.orders`
  GROUP BY 1,2,3)
SELECT
  order_month,
  order_year,
  LAG(order_count) OVER (PARTITION BY order_year ORDER BY order_months_1) AS previous_month_order_count,
  order_count
FROM MonthlyOrderCounts
ORDER BY order_year,order_months_1
```
Snapshots:

B. Can we see some kind of monthly seasonality in terms of the no. of orders being placed? Hint: We want you to find out if the no. of orders placed are at peak during certain months. 
Query:
``` sql
SELECT 
a.order_month,
a.order_year,
a.order_count,
  DENSE_RANK()OVER(PARTITION BY a.order_year ORDER BY a.order_count DESC) AS `Peak_months`
FROM 
(
  SELECT 
    FORMAT_DATE('%B',order_purchase_timestamp) AS order_month,
    EXTRACT(YEAR FROM order_purchase_timestamp) AS order_year,
    COUNT(order_id) AS order_count
  FROM  `target.orders`
  GROUP BY  order_month,order_year
  ORDER BY  order_month,order_year)as `a`
ORDER BY 2,4
```
Snapshots:

C. During what time of the day, do the Brazilian customers mostly place their orders? 
(Dawn, Morning, Afternoon or Night)
 ● 0-6 hrs : Dawn 
● 7-12 hrs : Mornings
 ● 13-18 hrs : Afternoon
 ● 19-23 hrs : Night 
Hint: We are categorizing the hours of a day into the given time intervals and finding out during which intervals the Brazilian customers usually order the most. 
Query:
``` sql
SELECT
(
  CASE
    WHEN EXTRACT(HOUR FROM order_purchase_timestamp) BETWEEN 0 AND 6 THEN 'Dawn'
    WHEN EXTRACT(HOUR FROM order_purchase_timestamp) BETWEEN 7 AND 12 THEN 'Morning'
    WHEN EXTRACT(HOUR FROM order_purchase_timestamp) BETWEEN 13 AND 18 THEN 'Afternoon'
    WHEN EXTRACT(HOUR FROM order_purchase_timestamp) BETWEEN 19 AND 23 THEN 'Night'
    ELSE 'Unknown'
  END 
  ) AS `time_of_day`,
  COUNT(*) AS order_count
FROM
  `target.orders`
GROUP BY
  time_of_day
ORDER BY
  order_count DESC
```
Snapshots:
______________________________________________________________________________ 
III. Evolution of E-commerce orders in the Brazil region: 
A. Get the month on month no. of orders placed in each state. Hint: We want you to get the no. of orders placed in each state, in each month by our customers. 
Query:
```
SELECT
  c.customer_state,
  EXTRACT(MONTH FROM o.order_purchase_timestamp) AS `order_month`,
  COUNT(o.order_id) AS `order_count`
FROM
  `target.orders` o
JOIN
  `target.customers` c
ON
  o.customer_id = c.customer_id
GROUP BY
  c.customer_state,
  order_month
ORDER BY
  order_month ASC
``` 
Snapshots:

B. How are the customers distributed across all the states? Hint: We want you to get the no. of unique customers present in each state. 
Query:
``` sql
SELECT
  customer_state,
  COUNT(DISTINCT customer_id) AS `unique_customers_count`
FROM
  `target.customers`
GROUP BY
  customer_state
ORDER BY
  unique_customers_count DESC
```
Snapshots:
______________________________________________________________________________ 
IV. Impact on Economy: Analyze the money movement by e-commerce by looking at order prices, freight and others.
Get the % increase in the cost of orders from year 2017 to 2018 (include months between Jan to Aug only). Hint: You can use the payment_value column in the payments table to get the cost of orders. 
Query:
``` sql
WITH OrderCost AS (
  SELECT
    EXTRACT(YEAR FROM o.order_purchase_timestamp) AS `order_year`,
    EXTRACT(MONTH FROM o.order_purchase_timestamp) AS `order_month`,
    SUM(p.payment_value) AS `total_payment_value`
  FROM
    `target.orders` AS `o`
  JOIN
    `target.payments` AS `p` 
    ON o.order_id = p.order_id
  WHERE
    EXTRACT(YEAR FROM o.order_purchase_timestamp) IN (2017, 2018)
    AND EXTRACT(MONTH FROM o.order_purchase_timestamp) BETWEEN 1 AND 8
  GROUP BY
    order_year,
    order_month
)
SELECT
  ROUND(((OC_2018.total_payment_value - OC_2017.total_payment_value) / OC_2017.total_payment_value * 100),2) AS `percentage_increase`
FROM
  OrderCost AS  `OC_2017`
JOIN
  OrderCost AS `OC_2018` 
  ON OC_2017.order_month = OC_2018.order_month
WHERE
  OC_2017.order_year = 2017 AND 
  OC_2018.order_year = 2018
```
Snapshots:

B. Calculate the Total & Average value of order price for each state. Hint: We want you to fetch the total price and the average price of orders for each state. 
Query:
``` sql
SELECT
c.customer_state,
ROUND(SUM(ot.price),2) AS `total_value`,
ROUND(AVG(ot.price),2) AS `avg_value`
FROM
`target.customers` c
JOIN `target.orders` o
ON c.customer_id = o.customer_id
JOIN `target.order_items` ot
ON o.order_id = ot.order_id
GROUP BY 1
ORDER BY 1
```
Snapshots:

C. Calculate the Total & Average value of order freight for each state. Hint: We want you to fetch the total freight value and the average freight value of orders for each state. 
Query:
``` sql
SELECT
c.customer_state,
ROUND(SUM(ot.freight_value),2) AS `total_value`,
ROUND(AVG(ot.freight_value),2) AS `avg_value`
FROM
`target.customers` c
JOIN `target.orders` o
ON c.customer_id = o.customer_id

JOIN `target.order_items` ot
ON o.order_id = ot.order_id
GROUP BY 1
ORDER BY 1
```
Snapshots:
__________________________________________________________________________________
V. Analysis based on sales, freight and delivery time. 
A. Find the no. of days taken to deliver each order from the order’s purchase date as delivery time. Also, calculate the difference (in days) between the estimated & actual delivery date of an order. Do this in a single query. Hint: You can calculate the delivery time and the difference between the estimated & actual delivery date using the given formula: 
● time_to_deliver = order_delivered_customer_date - order_purchase_timestamp
● diff_estimated_delivery = order_estimated_delivery_date - order_delivered_customer_date 

Query:
``` sql
SELECT
DATE_DIFF(order_delivered_customer_date, order_purchase_timestamp, DAY) AS `time_to_deliver`,
DATE_DIFF(order_estimated_delivery_date, order_delivered_customer_date, DAY) AS `diff_estimated_delivery`
FROM
`target.orders`
```
Snapshots:

Insights: NA
Recommendations: NA

B. Find out the top 5 states with the highest & lowest average freight value. Hint: We want you to find the top 5 & the bottom 5 states arranged in increasing order of the average freight value. 
Query:
``` sql
SELECT
 high.customer_state AS `High_state`, 
 high.avg_freight_value AS `High_avg_freight`, 
 low.customer_state AS `Low_state`, 
 low.avg_freight_value AS `Low_avg_freight`
FROM
(
 SELECT
 c.customer_state, 
 ROUND(AVG(ot.freight_value),2) AS `avg_freight_value`, 
 ROW_NUMBER() OVER(ORDER BY
(ROUND(AVG(ot.freight_value),2))DESC) AS `row_val_1`
 FROM `target.orders` AS o
 JOIN `target.order_items` AS ot
 ON o.order_id = ot.order_id
 JOIN `target.customers` AS c
 ON o.customer_id = c.customer_id
 GROUP BY 
 c.customer_state
 ORDER BY 
 avg_freight_value DESC
 LIMIT 
 5
) AS high
JOIN
(
 SELECT
 c.customer_state, 
 ROUND(AVG(ot.freight_value),2) AS avg_freight_value, 
 ROW_NUMBER() OVER(ORDER BY (ROUND(AVG(ot.freight_value),2)))
AS `row_val_2`
 FROM `target.orders` AS o
 JOIN `target.order_items` AS ot
 ON o.order_id = ot.order_id
 JOIN `target.customers` AS c
 ON o.customer_id = c.customer_id
 GROUP BY 
 c.customer_state
 ORDER BY avg_freight_value
 LIMIT 
 5
) AS low
ON high.row_val_1 = low.row_val_2
```
Snapshots:

C. Find out the top 5 states with the highest & lowest average delivery time. Hint: We want you to find the top 5 & the bottom 5 states arranged in increasing order of the average delivery time. D. Find out the top 5 states where the order delivery is really fast as compared to the estimated date of delivery. You can use the difference between the averages of actual & estimated delivery date to figure out how fast the delivery was for each state. Hint: Include only the orders that are already delivered. 
Query:
``` sql
WITH cte AS 
(
 SELECT
 c.customer_state, 
 ROUND(AVG(t1.del_time),2) AS `avg_delivery_time`
 FROM
 (
 SELECT 
 *, 
 TIMESTAMP_DIFF(order_delivered_customer_date,order_purchase_timestamp, day) AS `del_time`, 
 FROM 
 `target.orders`
 WHERE 
 order_status = 'delivered' AND
order_delivered_customer_date IS NOT NULL 
 ORDER BY 
 order_purchase_timestamp
) AS `t1`
JOIN 
 `target.customers` AS c
 ON t1.customer_id = c.customer_id
GROUP BY 
 c.customer_state
ORDER BY 
 avg_delivery_time
)
SELECT
 c1.customer_state AS `Low_state`, 
 c1.avg_delivery_time AS `Low_avg_delivery_time`, 
 c2.customer_state AS `High_state`, 
 c2.avg_delivery_time AS `High_avg_delivery_time`
FROM
(
 SELECT 
 *, 
 ROW_NUMBER() OVER (ORDER BY cte.avg_delivery_time DESC) AS `row_val_2`
 FROM 
 cte
 ORDER BY 
 row_val_2
) AS c2
JOIN
(
 SELECT 
 *, 
 ROW_NUMBER() OVER (ORDER BY cte.avg_delivery_time) AS `row_val_1`
 FROM 
 cte
 ORDER BY 
 row_val_1
) AS c1
ON 
 c1.row_val_1 = c2.row_val_2
LIMIT 
 5
```
Snapshots:
______________________________________________________________________________ 
VI. Analysis based on the payments: 
Find the month on month no. of orders placed using different payment types. Hint: We want you to count the no. of orders placed using different payment methods in each month over the past years.

Query:
``` sql
SELECT
 FORMAT_TIMESTAMP('%Y-%m', o.order_purchase_timestamp) AS `Month`, 
 p.payment_type, 
 COUNT(DISTINCT o.order_id) AS `order_count`
FROM `target.orders` AS o
JOIN `target.payments` AS p
ON o.order_id = p.order_id
GROUP BY Month, p.payment_type
ORDER BY Month
```
Snapshots:

Find the no. of orders placed on the basis of the payment installments that have been paid. Hint: We want you to count the no. of orders placed based on the no. of payment installments where at least one installment has been successfully paid. 

Query:
``` sql
SELECT 
payment_installments, 
COUNT(DISTINCT order_id) AS `order_count`
FROM 
`target.payments`
GROUP BY 1
ORDER BY 1
```
Snapshots:

### Insights

- The highest peak was on October 2016 with 324 orders.
- The highest peak was on November 2017 with 7544 orders.
- The Highest peak was on January 2018 with 7269 orders.
- Maximum number of orders were in the afternoon & the Minimum number of orders were in the Dawn.
- Then Lowest average delivery time was on the state SP & the Highest average delivery time is RR.
- Under Credit cards, most of the orders were on November 2017 with the count of 5867 
- Under UPI, most of the orders were on January 2018 with the count of 1518
- Under Debit cards, most of the orders were on August 2018 with the count of 277
- Under Voucher, most of the orders were on January 2018 with the count of 304
- The Most Number of payment installment were on 2nd installment with the count of 49060
- The least number of payment installment were on 22nd  & 23rd installment with the count of 1

### Recommendations
In order to raise more number of sales, in the dawn, you can fix a timeframe & provide some exciting offers & discounts, which will make them shop more as it is in that specific period. 
