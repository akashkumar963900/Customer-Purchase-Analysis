USE customer;
SELECT 
    *
FROM
    customer_shopping_behavior;
-- Q1.what is the total revanue generated from male vs female customers
SELECT 
    Gender, SUM(`Purchase Amount (USD)`) AS revenue
FROM
    customer_shopping_behavior
GROUP BY Gender;
-- Q2.which customers used a discount but still spent more then the average purchase amount
SELECT 
    `customer id`, `purchase amount (usd)`
FROM
    customer_shopping_behavior
WHERE
    `discount applied` = 'Yes'
        AND `purchase amount (usd)` > (SELECT 
            AVG(`Purchase Amount (USD)`)
        FROM
            customer_shopping_behavior);

-- Q3.what are the top 5 products with the highest avg review rating
SELECT 
    `item purchased`,
    ROUND(AVG(`review rating`), 2) AS avg_review_rating
FROM
    customer_shopping_behavior
GROUP BY `item purchased`
ORDER BY avg_review_rating DESC
LIMIT 5;

-- Q4.compare average puchased amount between standard shipping and express shipping
SELECT 
    `Shipping Type`,
    ROUND(AVG(`Purchase Amount (USD)`), 2) AS avg_purchased_amount
FROM
    customer_shopping_behavior
WHERE
    `Shipping Type` IN ('Standard' , 'Express')
GROUP BY `Shipping Type`;

-- Q5.Do subcribed customers spends more? compare avg spends and total revenue
-- between subcribers and non-subscribers
SELECT 
    `subscription status`,
    ROUND(AVG(`purchase amount (usd)`), 2) AS avg_purchase_amount,
    SUM(`purchase amount (usd)`) AS total_revenue,
    COUNT(*) AS number_of_subscribers
FROM
    customer_shopping_behavior
GROUP BY `subscription status`;

-- Q6.which 5 products have highest percentage of purchases with discounts applied?
SELECT 
    `Item Purchased`,
    ROUND((SUM(CASE
                WHEN `discount applied` = 'Yes' THEN 1
                ELSE 0
            END) / COUNT(*)) * 100,
            2) AS percentage_of_purchases
FROM
    customer_shopping_behavior
GROUP BY `Item Purchased`
ORDER BY percentage_of_purchases DESC
LIMIT 5;

-- Q7.segment customer into new,returning,and loyal based on their total number of previous
-- purchases and show the count of each segment
with customer_type as (
select `customer id`,`previous purchases`,
case 
when `previous purchases`=1 then 'New'
when `previous purchases` between 2 and 10 then 'Returning'
else 'Loyal'
end as customer_segment
from customer_shopping_behavior
)
select customer_segment,count(*) as number_of_customer
from customer_type
group by customer_segment
order by number_of_customer desc;

-- Q8.what are the 3 most purchased products wihtin in each category
with item_count as (
select category ,`item purchased` as item_purchased,
count(`customer id`) as total_orders,
row_number() over(partition by category order by count(`customer id`) desc) as item_rank
from customer_shopping_behavior
group by category,`Item Purchased`
)
select item_rank,category,item_purchased,total_orders
from item_count 
where item_rank<=3;

-- Q9.Are customers who are repeat buyers (more then 5 previous purchases) also likely to subscribe?
SELECT 
    `subscription status`, COUNT(`customer id`) AS repeat_buyers
FROM
    customer_shopping_behavior
WHERE
    `previous purchases` > 5
GROUP BY `subscription status`;

-- Q10.What is the revenue contributin of each age group
SELECT 
    age_group, SUM(purchase_amount) AS total_revenue
FROM
    customers
GROUP BY age_group
ORDER BY total_revenue DESC;
