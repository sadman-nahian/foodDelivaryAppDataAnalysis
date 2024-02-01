# Food Delivery App Database Analysis
i have created a dummy data for a food delivary app and ran some sql operations on these datasets which can be found on #script.sql file to analyze the data. 

This repository contains the database schema and operations for the Food Delivery App. The database consists of the following tables:

## Tables

### 1. Sales Table

#### Schema

```sql
CREATE TABLE sales (
    userid INT,
    created_date DATE,
    product_id INT
);
Description:
The sales table records information about the products sold, including the user ID, creation date, and product ID.

CREATE TABLE product (
    product_id INT,
    product_name TEXT,
    price INT
);

Description:
The product table stores details about each product, such as the product ID, product name, and price.

CREATE TABLE users (
    userid INT,
    signup_date DATE
);

Description:
The users table maintains information about the users, including the user ID and signup date.


CREATE TABLE goldusers_signup (
    userid INT,
    gold_signup_date DATE
);

Description:
The goldusers_signup table contains details about users who have signed up for a premium or gold membership, including the user ID and gold signup date.


Database Operations:

# 1. Find the Last Person Who Bought Membership

SELECT * FROM goldusers_signup ORDER BY gold_signup_date DESC LIMIT 1;

# 2. Count Total Products in Service

SELECT COUNT(DISTINCT(product_id)) AS count FROM product;

#3. Identify User with the Most Purchased Items

SELECT userid, COUNT(product_id) AS count FROM sales GROUP BY userid ORDER BY count DESC LIMIT 1;

#4. what is the total amount each customer spend on app

select a.userid ,sum(b.price) total_amt_spent from sales a inner join product b on a.product_id=b.product_id
 -> group by a.userid;

#5. how many days each customers visited app

select userid,count(distinct created_date) as total_days from sales group by userid;

#6. what is the first product purchased by each customers

SELECT * FROM ( SELECT *,  RANK() OVER (PARTITION BY userid ORDER BY created_date) AS rnk FROM sales ) a WHERE rnk = 1;

#7. what is the most purchased item

select product_id,count(product_id) as total_bought from sales group by product_id limit 1;

#8.  what is the most purchased item and how many time each user has bought it

select userid,count(product_id)from sales where product_id =( select product_id from sales group by product_id limit 1) group by userid;

Contributors:
Sadman AL Nahian