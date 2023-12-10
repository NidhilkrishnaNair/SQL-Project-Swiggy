# SQL-Project-Swiggy

Analysed Swiggy data in MySQL.

Please find the SQL script below:

select * from swiggy limit 1000;

select count(*) from swiggy;

/*
QUESTION 1- HOW MANY RESTAURANTS HAVE A RATING GREATER THAN 4.5?*/

select count(distinct restaurant_name) as 'Restaurant with rating more than 4.5' from swiggy where rating>4.5;

/* QUESTION 2- WHICH IS THE TOP 1 CITY WITH THE HIGHEST NUMBER OF RESTAURANTS?*/

select city, count(distinct restaurant_name) as 'Restaurant count' from swiggy group by city order by 'Restaurant count' desc limit 1;

/* QUESTION 3- SHOW RESTAURANTS THAT HAVE WORD "PIZZA" IN THEIR NAME)?*/

select distinct restaurant_name from swiggy where restaurant_name like "%PIZZA%";

/* QUESTION 4- WHAT IS top 10 MOST COMMON CUISINE AMONG THE RESTAURANTS IN THE DATASET?*/

select cuisine,count(*) as cuisine_count from swiggy group by cuisine order by cuisine_count desc limit 10;

/* QUESTION 5-WHAT IS THE AVERAGE RATING OF RESTAURANTS IN EACH CITY?*/

select city, avg(rating) from swiggy group by city;

/* QUESTION 6-WHAT IS THE HIGHEST PRICE OF ITEM UNDER THE 'RECOMMENDED' MENU CATEGORY FOR EACH RESTAURANT?*/

select distinct restaurant_name,menu_category,max(price) from swiggy where menu_category="Recommended" group by restaurant_name;

/* QUESTION 7-FIND THE TOP 5 MOST EXPENSIVE RESTAURANTS THAT OFFER CUISINE OTHER THAN INDIAN CUISINE. */

select distinct restaurant_name,cost_per_person,cuisine from swiggy where cuisine<>"Indian" order by cost_per_person desc limit 5;

/* QUESTION 8-FIND THE RESTAURANTS THAT HAVE AN AVERAGE COST WHICH IS HIGHER THAN THE TOTAL AVERAGE COST OF ALL    
   RESTAURANTS TOGETHER.*/
   
   select distinct restaurant_name, cost_per_person from swiggy where cost_per_person>(select avg(cost_per_person) from swiggy) order by cost_per_person desc;
   
 /* QUESTION 9-  RETRIEVE THE DETAILS OF RESTAURANTS THAT HAVE THE SAME NAME BUT ARE LOCATED IN DIFFERENT CITIES.*/
 
 select distinct s.restaurant_name, s.city,a.city from swiggy s join swiggy a on a.restaurant_name=s.restaurant_name and s.city<>a.city;
 
 /*QUESTION 10- WHICH RESTAURANT OFFERS THE MOST NUMBER OF ITEMS IN THE MAIN COURSE CATEGORY*/
 
 select distinct restaurant_name,menu_category,count(item) from swiggy where menu_category="Main Course" group by restaurant_name order by count(item) desc limit 1;
 
 /*QUESTION 11-  THE NAMES OF RESTAURANTS THAT ARE 100% VEGEATARIAN IN ALPHABETICAL ORDER OF RESTAURANT NAME*/
 
select restaurant_name, count(*) as 'Total items offered',count(case  when veg_or_nonveg = 'Veg' then 1 end) as 'Total vegetarian items offered' from swiggy group by restaurant_name having count(*)= count(case  when veg_or_nonveg = 'Veg' then 1 end);

 /*QUESTION 12 WHICH IS THE RESTAURANT PROVIDING THE LOWEST AVERAGE PRICE FOR ALL ITEMS?*/
 
 select restaurant_name,avg(price) from swiggy group by restaurant_name order by avg(price) limit 1;
 
  /*QUESTION 13 WHICH TOP 5 RESTAURANT OFFERS HIGHEST NUMBER OF CATEGORIES? */
  
  select restaurant_name, count(distinct menu_category) from swiggy group by restaurant_name order by count(distinct menu_category) desc limit 5;
  
    /*QUESTION 14 Top 5 RESTAURANT with HE HIGHEST PERCENTAGE OF NON-VEGEATARIAN FOOD?*/
    
   select restaurant_name, (count(case when veg_or_nonveg = 'Non-veg' then 1 end)/ count(*))*100 as Percentage_of_Nonveg from swiggy group by restaurant_name order by Percentage_of_Nonveg  desc limit 5;
   
      /*QUESTION 15 Determine the Most Expensive and Least Expensive Cities for Dining:*/
      
select city,max(cost_per_person),min(cost_per_person) from swiggy group by city order by max(cost_per_person) desc;    

  /*QUESTION 16 find top 2 ranked restaurants based on ranking Within Its City*/
  
  with ratingrankbycity as (select distinct restaurant_name,city,rating, dense_rank() over(partition by city order by rating desc) as rating_rank from swiggy)
  select restaurant_name,city,rating,rating_rank from ratingrankbycity where rating_rank in (1,2);
