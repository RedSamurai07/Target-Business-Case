# Target Business Case using SQL

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
Target is a globally renowned brand and a prominent retailer in the United States. Target makes itself a preferred shopping destination by offering outstanding value, inspiration, innovation and an exceptional guest experience that no other retailer can deliver.
This particular business case focuses on the operations of Target in Brazil and provides insightful information about 100,000 orders placed between 2016 and 2018. The dataset offers a comprehensive view of various dimensions including the order status, price, payment and freight performance, customer location, product attributes, and customer reviews.
By analyzing this extensive dataset, it becomes possible to gain valuable insights into Target's operations in Brazil. The information can shed light on various aspects of the business, such as order processing, pricing strategies, payment and shipping efficiency, customer demographics, product characteristics, and customer satisfaction levels.

### Executive Summary
This analysis provides a comprehensive overview of the e-commerce operations in Brazil, offering actionable insights for various departments:

**Sales Department:**
The analysis reveals that the afternoon (13-18 hrs) is the peak time for customer orders, while dawn (0-6 hrs) sees the minimum number of orders. This critical information allows the sales team to optimize staffing and sales efforts during high-demand periods and potentially strategize promotions for slower times like dawn to boost sales.

**Product Management Department:**
While specific price elasticity or performance comparisons between premium and budget products are not directly provided, the analysis does highlight the highest order peaks by month and year, such as November 2017 with 7544 orders and January 2018 with 7269 orders. This can indirectly inform product launch timings or promotional strategies based on historical sales volume.

**Branch Managers:** 
The analysis provides a clear distribution of unique customers across states, with SP having the highest number (41746) and states like GO (2020) and ES (2033) having lower counts. This data is crucial for branch managers to understand regional customer density, optimize inventory based on local demand, and tailor product offerings to specific state preferences. The total and average order prices and freight values per state (e.g., SP with a total value of 511349.99 ) further aid in local inventory and sales planning.

**Marketing Team:** 
The identification of peak ordering times (afternoon) and monthly order seasonality (e.g., November 2017 and January 2018 being high-order months)  offers valuable insights for strategic promotion planning. Understanding the dominant payment types, with credit cards being the most used , and the prevalence of single-installment payments (49060 orders), can help the marketing team design effective combo offers, tailor discount strategies, and optimize payment-specific promotions to increase customer visits and boost revenue. The recommendation to provide exciting offers and discounts during dawn hours to increase sales is a direct marketing application derived from the analysis.

### Goal
The primary goal of this project is to provide a comprehensive understanding of the e-commerce landscape in Brazil by:

1. Exploratory Data Analysis: To understand the structure and characteristics of the dataset, including data types, time ranges, and geographical distribution of customers.
2. In-depth Order Analysis: To identify trends and seasonality in order placement, including monthly growth and peak ordering times.
3. Geographical Distribution: To analyze the distribution of orders and customers across different states.
4. Economic Impact Assessment: To evaluate the financial aspects of orders, such as price and freight value, and analyze the percentage increase in costs over time.
5. Delivery Performance Evaluation: To calculate delivery times and the difference between estimated and actual delivery dates, and identify states with the fastest and slowest average delivery times.
6. Payment Analysis: To understand the month-on-month order distribution based on different payment types and the number of payment installments.

### Data structure and initial checks
[Dataset](https://drive.google.com/drive/folders/1TGEc66YKbD443nslRi1bWgVd238gJCnb)

The data is available in 8 csv files:

- customers.csv
- sellers.csv
- order_items.csv
- geolocation.csv
- payments.csv
- reviews.csv
- orders.csv
- products.csv

The column description for these csv files is given below.

1. The customers.csv contain following features:

| Features | Description |
| -------- | -------- | 
| customer_id | ID of the consumer who made the purchase |  
| customer_unique_id | Unique ID of the consumer | 
| customer_zip_code_prefix | Zip Code of consumer’s location | 
| customer_city | Name of the City from where order is made | 
| customer_state | State Code from where order is made (Eg. são paulo - SP) | 

2. The sellers.csv contains following features:

| Features | Description | 
| -------- | -------- | 
| seller_id | Unique ID of the seller registered | 
| seller_zip_code_prefix | Zip Code of the seller’s location | 
| seller_city | Name of the City of the seller |  
| seller_state | State Code (Eg. são paulo - SP) |  

3. The order_items.csv contain following features:

| Features | Description | 
| -------- | -------- |  
| order_id | A Unique ID of order made by the consumers | 
| order_item_id | A Unique ID given to each item ordered in the order |  
| product_id | A Unique ID given to each product available on the site | 
| seller_id | Unique ID of the seller registered in Target | 
| shipping_limit_date | The date before which the ordered product must be shipped |  
| price | Actual price of the products ordered |  
| freight_value | Price rate at which a product is delivered from one point to another |  

4. The geolocations.csv contain following features:
   
| Features | Description |
| -------- | -------- |  
| geolocation_zip_code_prefix | First 5 digits of Zip Code |  
| geolocation_lat | Latitude |  
| geolocation_lng | Longitude |  
| geolocation_city | City |  
| geolocation_state | State |  

5. The payments.csv contain following features:

| Features | Description |
| -------- | -------- |  
| order_id | A Unique ID of order made by the consumers |  
| payment_sequential | Sequences of the payments made in case of EMI |  
| payment_type | Mode of payment used (Eg. Credit Card) |  
| payment_installments | Number of installments in case of EMI purchase |  
| payment_value | Total amount paid for the purchase order |  

6. The orders.csv contain following features:

| Features | Description |
| -------- | -------- |  
| order_id | A Unique ID of order made by the consumers |  
| customer_id | ID of the consumer who made the purchase |  
| order_status | Status of the order made i.e. delivered, shipped, etc. |  
| order_purchase_timestamp | Timestamp of the purchase |  
| order_delivered_carrier_date | Delivery date at which carrier made the delivery |  
| order_delivered_customer_date | Date at which customer got the product |  
| order_estimated_delivery_date | Estimated delivery date of the products |  

7. The reviews.csv contain following features:
   
| Features | Description |
| -------- | -------- |  
| review_id | ID of the review given on the product ordered by the order id |  
| order_id | A Unique ID of order made by the consumers |  
| review_score | Review score given by the customer for each order on a scale of 1-5 |  
| review_comment_title | Title of the review |  
| review_comment_message | Review comments posted by the consumer for each order |
| review_creation_date | Timestamp of the review when it is created |  
| review_answer_timestamp | Timestamp of the review answered |  

8. The products.csv contain following features:
   
| Features | Description |
| -------- | -------- |  
| product_id | A Unique identifier for the proposed project. |  
| product_category_name | Name of the product category |  
| product_name_lenght | Length of the string which specifies the name given to the products ordered |  
| product_description_lenght | Length of the description written for each product ordered on the site |  
| product_photos_qty | Number of photos of each product ordered available on the shopping portal |  
| product_weight_g | Weight of the products ordered in grams |  
| product_length_cm | Length of the products ordered in centimeters |  
| product_height_cm | Height of the products ordered in centimeters |  
| product_width_cm | Width of the product ordered in centimeters |  

Data Schema
<img width="1600" height="962" alt="image" src="https://github.com/user-attachments/assets/fd84b2a2-cd70-4435-8c77-1cbc1325c56f" />

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

<img width="555" height="145" alt="image" src="https://github.com/user-attachments/assets/334cd14c-4f02-4509-8a44-4a50e073ce6b" />

B. Count the Cities & States of customers who ordered during the given period. Hint: We want you to count the number of unique cities & states where orders were placed by the customers during the given time period. 

``` sql
SELECT
COUNT(DISTINCT customer_city) AS `cx_city`,
COUNT(DISTINCT customer_state) AS `cx_state`
FROM
`target.customers`
```
Snapshots:

<img width="487" height="107" alt="image" src="https://github.com/user-attachments/assets/352a5f59-6b50-44b2-a5df-0753ecc4ba9d" />

II. In-depth Exploration: 
A. Is there a growing trend in the no. of orders placed over the past years? Hint: We want you to find out if no. of orders placed has increased gradually in each month, over the past years. 

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

<img width="668" height="290" alt="image" src="https://github.com/user-attachments/assets/7583402f-d47e-4a0f-a2cd-e47c490b2072" />

B. Can we see some kind of monthly seasonality in terms of the no. of orders being placed? Hint: We want you to find out if the no. of orders placed are at peak during certain months. 

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

<img width="681" height="390" alt="image" src="https://github.com/user-attachments/assets/c0f1f567-e2f3-4caa-837a-f1bfeaa37c40" />

C. During what time of the day, do the Brazilian customers mostly place their orders? 
(Dawn, Morning, Afternoon or Night)
 ● 0-6 hrs : Dawn 
● 7-12 hrs : Mornings
 ● 13-18 hrs : Afternoon
 ● 19-23 hrs : Night 
Hint: We are categorizing the hours of a day into the given time intervals and finding out during which intervals the Brazilian customers usually order the most. 

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

<img width="493" height="180" alt="image" src="https://github.com/user-attachments/assets/c86b390d-8f2f-465f-b577-eaa013fd216b" />

III. Evolution of E-commerce orders in the Brazil region: 
A. Get the month on month no. of orders placed in each state. Hint: We want you to get the no. of orders placed in each state, in each month by our customers. 

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

<img width="530" height="347" alt="image" src="https://github.com/user-attachments/assets/bb768071-3c03-4ee2-a2bc-cd3bfd863126" />

B. How are the customers distributed across all the states? Hint: We want you to get the no. of unique customers present in each state. 

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

<img width="451" height="354" alt="image" src="https://github.com/user-attachments/assets/c3c7313a-e6a4-413a-ac3b-be19d24d8a11" />

IV. Impact on Economy: Analyze the money movement by e-commerce by looking at order prices, freight and others.
Get the % increase in the cost of orders from year 2017 to 2018 (include months between Jan to Aug only). Hint: You can use the payment_value column in the payments table to get the cost of orders. 

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

<img width="238" height="293" alt="image" src="https://github.com/user-attachments/assets/cf64985d-7ff2-4a89-9f60-b02a18cdc0a7" />

B. Calculate the Total & Average value of order price for each state. Hint: We want you to fetch the total price and the average price of orders for each state. 

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

<img width="571" height="346" alt="image" src="https://github.com/user-attachments/assets/30958633-9ff9-4d4b-95aa-41e3429d9ce7" />

C. Calculate the Total & Average value of order freight for each state. Hint: We want you to fetch the total freight value and the average freight value of orders for each state. 

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

<img width="638" height="360" alt="image" src="https://github.com/user-attachments/assets/ac5a56a0-4387-4e5e-9cae-9fc9f9cb016c" />

V. Analysis based on sales, freight and delivery time. 
A. Find the no. of days taken to deliver each order from the order’s purchase date as delivery time. Also, calculate the difference (in days) between the estimated & actual delivery date of an order. Do this in a single query. Hint: You can calculate the delivery time and the difference between the estimated & actual delivery date using the given formula: 
● time_to_deliver = order_delivered_customer_date - order_purchase_timestamp
● diff_estimated_delivery = order_estimated_delivery_date - order_delivered_customer_date 

``` sql
SELECT
DATE_DIFF(order_delivered_customer_date, order_purchase_timestamp, DAY) AS `time_to_deliver`,
DATE_DIFF(order_estimated_delivery_date, order_delivered_customer_date, DAY) AS `diff_estimated_delivery`
FROM
`target.orders`
```
Snapshots:

<img width="406" height="360" alt="image" src="https://github.com/user-attachments/assets/ea165a23-9a76-4eac-9714-e54c5d07a37e" />

B. Find out the top 5 states with the highest & lowest average freight value. Hint: We want you to find the top 5 & the bottom 5 states arranged in increasing order of the average freight value. 

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

<img width="682" height="233" alt="image" src="https://github.com/user-attachments/assets/353207c7-8d63-414b-8a01-48cffe5710b9" />

C. Find out the top 5 states with the highest & lowest average delivery time. Hint: We want you to find the top 5 & the bottom 5 states arranged in increasing order of the average delivery time. D. Find out the top 5 states where the order delivery is really fast as compared to the estimated date of delivery. You can use the difference between the averages of actual & estimated delivery date to figure out how fast the delivery was for each state. Hint: Include only the orders that are already delivered. 

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

<img width="675" height="181" alt="image" src="https://github.com/user-attachments/assets/a0f6c536-c116-4b5c-9067-1e55128c0892" />

VI. Analysis based on the payments: 
Find the month on month no. of orders placed using different payment types. Hint: We want you to count the no. of orders placed using different payment methods in each month over the past years.

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

<img width="639" height="389" alt="image" src="https://github.com/user-attachments/assets/615853b5-2b3f-4def-8782-b910461f0d94" />

Find the no. of orders placed on the basis of the payment installments that have been paid. Hint: We want you to count the no. of orders placed based on the no. of payment installments where at least one installment has been successfully paid. 

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

<img width="388" height="333" alt="image" src="https://github.com/user-attachments/assets/96dec01b-87b0-486a-ac91-201b30b25a3b" />

### Insights
- Orders in the dataset were placed between September 15, 2016, at 12:16:38 UTC and September 3, 2018, at 17:40:06 UTC.
- Customers placed orders from 4119 unique cities and 27 unique states within the given period.
- There is an observable growing trend in the number of orders placed over the years, with most months showing an increase in order count compared to the previous month within the same year (e.g., from 800 orders in January 2017 to 1780 in February 2017, then to 2682 in March 2017).
- The highest peak in 2016 was in October with 324 orders.
- The highest peak in 2017 was in November with 7544 orders.
- The highest peak in 2018 was in January with 7269 orders.
- This indicates clear monthly seasonality, with different months experiencing peak order volumes in different years.
- The maximum number of orders were placed in the Afternoon (13-18 hrs) with 38,135 orders.
- The minimum number of orders were placed during Dawn (0-6 hrs) with 5,242 orders.
- The dataset provides a breakdown of the number of orders placed month-on-month for each state, showing variations in order volume across different states and months. For instance, in month 1 (January), SP had 3351 orders while CE had 99 orders.
- Customers are unevenly distributed across states, with SP having the highest number of unique customers (41,746), followed by RJ (12,852) and MG (11,635). This indicates a strong concentration of the customer base in certain states.
- The cost of orders showed significant percentage increases from 2017 to 2018 for months between January and August, with some months seeing increases over 100%, and one month experiencing a 705.13% increase.
- The analysis provides the total and average order price for each state, highlighting states with higher overall revenue (e.g., BA with 511349.99 total value) and average order values.
- The total and average freight values are provided for each state, indicating the cost of shipping associated with orders in different regions.
- The query successfully calculates the time taken to deliver each order and the difference between the estimated and actual delivery dates, providing a per-order view of delivery performance.
- The top 5 states with the highest average freight value are RR (42.98), PB (42.72), RO (41.07), AC (40.07), and PI (39.15).
- The top 5 states with the lowest average freight value are SP (15.15), PR (20.53), MG (20.63), RJ (20.96), and DF (21.04).
- The state with the lowest average delivery time is SP (8.3 days), indicating faster deliveries.
- The state with the highest average delivery time is RR (28.98 days), indicating slower deliveries.
- Credit card payments are the most frequent payment type, with the highest order count of 5867 in November 2017.
- For UPI, the peak was in January 2018 with 1518 orders.
- Debit card orders peaked in August 2018 with 277 orders.
- Voucher payments saw their highest count in January 2018 with 304 orders.
- The majority of orders are placed with a single payment installment (49,060 orders).
- The least number of orders are associated with 22nd and 23rd installments, each with a count of 1.

### Recommendations
- To increase sales during off-peak hours, specifically Dawn, consider implementing targeted exciting offers and discounts within that specific timeframe to incentivize customers to shop more.
- Focus marketing campaigns and staffing during peak afternoon hours to maximize sales conversions. For the "dawn" period, implement targeted promotions, early bird specials, or exclusive discounts to incentivize purchases and drive incremental sales during traditionally slow hours.
- Plan marketing campaigns, inventory, and promotional strategies well in advance of these peak months. Consider creating special events or product launches around these periods to maximize revenue. Analyze what factors might have driven the peaks in different months across years to replicate successes.
- Allocate marketing budgets and sales efforts proportionally to these high-density customer areas to ensure maximum reach and impact. Simultaneously, explore opportunities for growth in states with lower customer penetration through localized campaigns or partnerships
- Investigate the reasons for high freight costs in certain regions. This could involve exploring alternative shipping partners, optimizing logistics routes, or negotiating better rates for high-cost areas. Consider dynamic pricing for freight based on location to ensure profitability.
- Analyze the end-to-end delivery process in states with longer delivery times to identify bottlenecks and areas for improvement. This could involve optimizing warehouse locations, improving last-mile delivery, or enhancing communication with delivery partners. Faster and more reliable delivery can significantly improve customer satisfaction and repeat business. For states with fast delivery, highlight this as a competitive advantage in marketing.
- Conduct a detailed investigation into the drivers of these cost increases. Identify whether they are due to increased product costs, operational expenses, or other factors. Implement cost-control measures, renegotiate supplier contracts, or adjust pricing strategies to maintain healthy profit margins.
- Ensure a seamless and secure credit card payment process. While single installments are dominant, consider offering flexible installment options with clear terms to potentially increase average order value for larger purchases, especially for specific product categories, without incurring excessive risk.
- By implementing these recommendations, Target can leverage its data-driven insights to optimize operations, enhance customer satisfaction, and ultimately drive higher profitability in the Brazilian e-commerce market.
