# Pizza-Sales-Analysis
Create database Pizzahut;

--   Q1   Retrieve the total number of orders placed.


use pizzahut;

Select count(order_id) as totalorder from orders 




-- Q2   Calculate the total revenue generated from pizza sales.
SELECT 
    ROUND(SUM(order_details.quantity * pizzas.price),
            2) AS total_sales
FROM
    order_details
        JOIN
    pizzas ON pizzas.pizza_id = order_details.pizza_id




-- Q3  Identify the highest-priced pizza.

Select pizza_types.name, pizzas.price 
from pizza_types join pizzas 
on pizzas.pizza_type_id = pizza_types.pizza_type_id 
order by pizzas.price desc Limit 1



-- Q4 Identify the most common pizza size ordered.

SELECT 
    pizzas.size,
    COUNT(order_details.order_details_id) AS order_count
FROM
    pizzas
        JOIN
    order_details ON pizzas.pizza_id = order_details.pizza_id
GROUP BY pizzas.size
ORDER BY order_count DESC;



 
 

 -- Q5 List the top 5 most ordered pizza types along with their quantities.
 
SELECT 
    pizza_types.name, SUM(order_details.quantity) AS quantity
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.name
ORDER BY quantity DESC
LIMIT 5;



-- Q6 Join the necessary tables to find the total quantity of each pizza category ordered.
SELECT 
    pizza_types.category,
    SUM(order_details.quantity) AS quantity
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.category
ORDER BY quantity DESC;


-- Q7 Determine the distribution of orders by hour of the day.
SELECT 
    hour(orders_time) AS hour, COUNT(order_id) AS order_count
FROM
    orders
GROUP BY HOUR(orders_time);


-- Q8 Join relevant tables to find the category-wise distribution of pizzas.

SELECT 
    Category, COUNT(name)
FROM
    pizza_types
GROUP BY CATEGORY;



-- Q9 Group the orders by date and calculate the average number of pizzas ordered per day.

select 
round(avg(QUANTITY),0) as aveg_pizza_ordered_per_day
from 
(select orders.date, Sum ( order_details.qantity) as quantity 
from orders join order_details on orders.order_id = order_details.order_id 
group by orders.order_date) as order_quantity;


-- Q10 Determine the top 3 most ordered pizza types based on revenue.
SELECT 
    pizza_types.name,
    SUM(order_details.quantity * pizzas.price) AS revenue
FROM
    pizza_types
        JOIN
    pizzas ON pizzas.pizza_type_id = pizza_types.pizza_type_id
        JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.name
ORDER BY revenue DESC
LIMIT 3;
