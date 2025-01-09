# Swiggy_Data_Analysis_SQL
This project aims to explore and analyze a dummy dataset from Swiggy, a leading platform for restaurant discovery and 
food delivery. The objective is to leverage SQL queries to extract meaningful insights that will be valuable for both 
food lovers and data analysts.
# Dataset
The dataset, available in the swiggy.csv file, provides detailed information about a variety of restaurants, including 
their unique identifiers, names, locations, cuisines, menus, and more. The schema of the dataset is organized as follows:

•	restaurant_no     : A unique identifier for each restaurant. <br>
•	restaurant_name   : The name of the restaurant.<br>
•	city              : The city where the restaurant is located.<br>
•	address           : The restaurant’s address.<br>
•	rating            : The rating of the restaurant.<br>
•	cost_per_person   : The cost per person for dining at the restaurant.<br>
•	cuisine           : The type of cuisine offered by the restaurant.<br>
•	restaurant_link   : A link to the restaurant's page on Swiggy.<br>
•	menu_category     : The category of items on the restaurant’s menu.<br>
•	item              : The name of a specific menu item.<br>
•	price             : The price of the menu item.<br>
•	veg_or_nonveg     : Indicates whether the item is vegetarian or non-vegetarian.<br>

This dataset offers a valuable resource for analyzing trends in restaurant offerings and customer preferences 
on the Swiggy platform.

# Project Objectives
•	Identifying the highest-rated restaurants to gain insights into customer preferences and quality benchmarks.<br>
•	Determining the city with the highest concentration of restaurants to pinpoint key market areas.<br>
•	Analyzing the most popular cuisines across different cities to align offerings with local tastes.<br>
•	Examining menu categories and items to understand the variety and range of available options.<br>
•	Evaluating the cost per person for dining to assess pricing strategies and affordability in various locations.<br>

# Data Analysis Using SQL Queries
#### Q1. HOW MANY RESTAURANTS HAVE A RATING GREATER THAN 4.5? <br>
SELECT count(distinct restaurant_name) as High_rated_restaurants <br>
FROM swiggy<br>
WHERE rating>4.5; <br>

Output: <br>
![image](https://github.com/user-attachments/assets/0b22d8d8-930a-4a48-8f12-0d6ddfd5ba96)

#### Q2. WHICH IS THE TOP 1 CITY WITH THE HIGHEST NUMBER OF RESTAURANTS? <br>
SELECT city,count(distinct restaurant_name) as Restaurant_count <br>
FROM swiggy<br>
GROUP BY city<br>
ORDER BY restaurant_count desc limit 1; <br>

Output: <br>
![image](https://github.com/user-attachments/assets/509cfcd4-2e0c-4247-b53a-32155850d75d)

#### Q3. HOW MANY RESTAURANTS SELL( HAVE WORD "PIZZA" IN THEIR NAME)? <br>
SELECT count(distinct restaurant_name) as Pizza_Restaurants<br>
FROM swiggy <br>
WHERE restaurant_name like '%Pizza%'; <br>

Output: <br>
![image](https://github.com/user-attachments/assets/8b9a1fa3-cc14-4749-87dd-eb3dd40154cf)

#### Q4. WHAT IS THE MOST COMMON CUISINE AMONG THE RESTAURANTS IN THE DATASET? <br>
SELECT cuisine,count(*) as Cuisine_Count<br>
FROM swiggy<br>
GROUP BY cuisine<br>
ORDER BY cuisine_count desc limit 1; <br>

Output: <br>
![image](https://github.com/user-attachments/assets/29a65bfb-2085-4926-bd61-b1eb4310932d)

#### Q5. WHAT IS THE AVERAGE RATING OF RESTAURANTS IN EACH CITY? <br>
SELECT city, avg(rating) as Average_Rating<br>
FROM swiggy <br>
GROUP BY city; <br>

Output: <br>
![image](https://github.com/user-attachments/assets/e14dd4b2-8764-4b09-8314-ee4084c3b03d)

#### Q6. WHAT IS THE HIGHEST PRICE OF ITEM UNDER THE 'RECOMMENDED' MENU CATEGORY FOR EACH RESTAURANT? <br>
SELECT distinct restaurant_name, menu_category, max(price) as Highest_Price<br>
FROM swiggy <br>
WHERE menu_category='Recommended'<br>
GROUP BY restaurant_name,menu_category; <br>

Output: <br>
![image](https://github.com/user-attachments/assets/3bfa4b7e-817f-4cad-a477-cdba582bf9d6)

#### Q7. FIND THE TOP 5 MOST EXPENSIVE RESTAURANTS THAT OFFER CUISINE OTHER THAN INDIAN CUISINE. <br>
SELECT distinct restaurant_name,cost_per_person<br>
FROM swiggy <br>
WHERE cuisine<>'Indian'<br>
ORDER BY cost_per_person desc<br>
limit 5; <br>

Output: <br>
![image](https://github.com/user-attachments/assets/dc09703a-dd91-4a7e-b2f4-8577a7ec6be8)

#### Q8. FIND THE RESTAURANTS THAT HAVE AN AVERAGE COST WHICH IS HIGHER THAN THE TOTAL AVERAGE COST OF ALL RESTAURANTS TOGETHER. <br>
SELECT distinct restaurant_name,cost_per_person<br>
FROM swiggy <br>
WHERE cost_per_person>(SELECT avg(cost_per_person) FROM swiggy); <br>

Output: <br>
![image](https://github.com/user-attachments/assets/fe7dc3a7-031a-4563-a3fd-9dccff8cf509)

#### Q9. RETRIEVE THE DETAILS OF RESTAURANTS THAT HAVE THE SAME NAME BUT ARE LOCATED IN DIFFERENT CITIES. <br>
SELECT distinct t1.restaurant_name,t1.city,t2.city<br>
FROM swiggy <br>
t1 join swiggy t2 on t1.restaurant_name=t2.restaurant_name and t1.city<>t2.city; <br>

Output: <br>
![image](https://github.com/user-attachments/assets/76c9816e-b1c0-444d-8a05-b204b36177cc)

#### Q10. WHICH RESTAURANT OFFERS THE MOST NUMBER OF ITEMS IN THE 'MAIN COURSE' CATEGORY? <br>
SELECT distinct restaurant_name,menu_category,count(item) as No_of_Items <br>
FROM swiggy<br>
WHERE menu_category='Main Course' <br>
GROUP BY restaurant_name,menu_category<br>
ORDER BY no_of_items desc limit 1; <br>

Output: <br>
![image](https://github.com/user-attachments/assets/dee804fb-3c34-49af-b585-57d29af927d3)

#### Q11. LIST THE NAMES OF RESTAURANTS THAT ARE 100% VEGEATARIAN IN ALPHABETICAL ORDER OF RESTAURANT NAME
SELECT distinct restaurant_name, <br>
(count(case when veg_or_nonveg='Veg' then 1 end)*100/ count(*)) as Vegetarian_Percetage<br>
FROM swiggy<br>
GROUP BY restaurant_name<br>
HAVING vegetarian_percetage=100.00<br>
ORDER BY restaurant_name; <br>

Output: <br>
![image](https://github.com/user-attachments/assets/a7ba688d-0d4e-4aaf-b2a9-e28e8dc5bd8d)

#### Q12. WHICH IS THE RESTAURANT PROVIDING THE LOWEST AVERAGE PRICE FOR ALL ITEMS?
SELECT distinct restaurant_name, avg(price) as Average_Price<br>
FROM swiggy <br>
GROUP BY restaurant_name<br>
ORDER BY average_price limit 1; <br>

Output: <br>
![image](https://github.com/user-attachments/assets/ac427cdd-6560-4a25-841f-ff998f9fbb47)

#### Q13. WHICH TOP 5 RESTAURANT OFFERS HIGHEST NUMBER OF CATEGORIES?
SELECT distinct restaurant_name, count(distinct menu_category) as No_of_Categories<br>
FROM swiggy<br>
GROUP BY restaurant_name<br>
ORDER BY no_of_categories desc limit 5; <br>

Output: <br>
![image](https://github.com/user-attachments/assets/4dd36f24-232f-4592-9193-61a26d98e200)

#### Q14. WHICH RESTAURANT PROVIDES THE HIGHEST PERCENTAGE OF NON-VEGEATARIAN FOOD?
SELECT distinct restaurant_name, <br>
(count(case when veg_or_nonveg='Non-veg' then 1 end)*100/count(*)) As Nonvegetarian_Percentage<br>
FROM swiggy<br>
GROUP BY restaurant_name<br>
ORDER BY nonvegetarian_percentage desc limit 1; <br>

Output: <br>
![image](https://github.com/user-attachments/assets/108c1623-7cbd-446d-9f18-8590e68a4c12)

#### Q15. DETERMINE THE MOST EXPENSIVE AND LEAST EXPENSIVE CITIES FOR DINING.
WITH CityExpense AS (<br>
    SELECT city, <br>
        MAX(cost_per_person) AS max_cost, <br>
        MIN(cost_per_person) AS min_cost<br>
    FROM swiggy<br>
    GROUP BY city<br>
) <br>
SELECT city,max_cost,min_cost<br>
FROM CityExpense<br>
ORDER BY max_cost DESC; <br>

Output: <br>
![image](https://github.com/user-attachments/assets/1d9489f7-7530-4c68-a95d-14440a9db2df)

#### Q16. CALCULATE THE RATING RANK FOR EACH RESTAURANT WITHIN ITS CITY

WITH RatingRankByCity AS (<br>
    SELECT distinct<br>
        restaurant_name, <br>
        city, <br>
        rating, <br>
        DENSE_RANK() OVER (PARTITION BY city ORDER BY rating DESC) AS rating_rank<br>
    FROM swiggy<br>
) <br>
SELECT<br>
    restaurant_name, <br>
    city, <br>
    rating, <br>
    rating_rank<br>
FROM RatingRankByCity<br>
WHERE rating_rank = 1; <br>

Output: <br>
![image](https://github.com/user-attachments/assets/d2dead89-052e-4ce4-b1b0-c2fb31df3ccc)





 










