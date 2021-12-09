# ITCS6160

Team Member 

Parth Patel 

Het Thakkar 

Tharani Kumaresan 

Rishab Semla

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Project Introduction:

The goal of our projects is to create that manages campus controlled food delivery 
Service using SQL.we  will be enhancing the current database with a rating system for both restaurants and delivery drivers .The database includes for person  (like Student,Faculty ,Driver) , different locations, different restaurants and vehicles,delivery. MYSQL Workbench is utilized to create this  environment. MySQL was used for a majority of the SQL code that allowed us to have a visual representation of the tables, schemas and databases that we created. 

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Business Rules :
•	Person can be a Student, Faculty or Staff.

•	Only students can be drivers.

•	Each person must be a registered user.

•	Each person must place an order before rating a restaurant.

•	A delivery must be made before a person can rate a driver.

•	A delivery must be made before a person can rate a restaurant.

•	Each person can rate the delivery only one time per order.

•	Each person can rate the restaurant only one time per order.

•	Administrators can view driver ratings and restaurant ratings.

•	The ratings will be based on a 5-level rating system.

•	Each restaurant supplies one to many items.

•	Restaurants are limited to offer up to 10 items.

•	Each order should have one to many items.

•	Order can only be from one restaurant only

•	Order can only be from one person only.

•	Each order will have an optional rating.


-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Use Case for Ratings System :

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

EERD: 
![EERD](https://user-images.githubusercontent.com/78390137/141709100-fba4d7c5-658d-45fd-838f-15cd4a94fef9.png)

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Data Dictionary:


![image](https://user-images.githubusercontent.com/78390137/141708834-0618f6f8-25eb-407a-863b-555759d0f846.png)

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Queries:


[1] display the max, min and average ratings for each feature when given a restaurant ID for all orders for that restaurant

select restaurant_id, order_id, max(restaurant_review) as max_rr,
min(restaurant_review) as min_rr,
avg(restaurant_review) as avg_rr,
max(driver_review) as max_dr,
min(driver_review) as min_dr,
avg(driver_review) as avg_dr
from ratings
where restaurant_id = 1

[2] display a count of the orders made by a customer for a specified date range when given a customer id

SELECT count(o.person_id) 
FROM campus_eats_fall2020.order o 
Join order_items oi 
on o.order_id = oi.order_id 
where o.person_id = 1
and date between '2020-10-22' and '2021-10-22'
group by o.person_id;



[3] display total price of the orders by each customer (distinct) for a specified date range

SELECT sum(o.total_price) 
FROM campus_eats_fall2020.order o 
Join order_items oi 
on o.order_id = oi.order_id
where date between '2020-10-22' and '2021-10-22'
group by o.person_id;

[4] display a particular customer’s rating for a restaurant

Select p.person_name,res.restaurant_name,rat.restaurant_review 
from campus_eats_fall2020.restaurant res
Join campus_eats_fall2020.ratings rat
on res.restaurant_id = rat.restaurant_id
Join campus_eats_fall2020.order o
on rat.order_id = o.order_id
Join campus_eats_fall2020.person p
on p.person_id = o.person_id
where p.person_id=1
and res.restaurant_id=1
group by o.order_id;


-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Stored Procedures and Functions

DROP PROCEDURE IF EXISTS total_order_by_customer
DELIMITER //
CREATE PROCEDURE total_order_by_customer ( in cus_id int, in datea VARCHAR(100), in dateb
VARCHAR(100), out total_order int)
BEGIN
SELECT COUNT(*)
INTO total_order
FROM campus_eats_fall2020.order o
Join order_items oi 
on o.order_id = oi.order_id 
WHERE o.person_id = cus_id 
AND date BETWEEN datea AND dateb;
END //
DELIMITER ;
set @cus_id=1, @datea='2020-10-22', @dateb='2021-10-22';
CALL total_order_by_customer(@cus_id, @datea, @dateb, @total_order);
select @total_order;
