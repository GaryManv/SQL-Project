Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**
Thoughts: productrevenue (null),totaltransactionrevenue and revenue are the same. Will use revenue from analytics table. What to do with many nulls? Can we somehow calculate them , at least put some average value? 

SQL Queries:
-- creating union tables from analytics and all_sessions with less columns and grouped revenues in order to check y countryies and cities
create table tbl_revenue as 
with union_tables as (select fullvisitorid,date_gm,totaltransactionrevenue as revenue,'all_sessions' as tablename from all_sessions 
where totaltransactionrevenue is not null

union all

select fullvisitorid,date_gm,revenue,'analytic'as tablename from analytics 
where revenue is not null

order by date_gm)

select fullvisitorid,union_tables.date_gm,sum(revenue) as t_revenue,country,city
from union_tables 
left join all_sessions using (fullvisitorid)
group by fullvisitorid,country,city,union_tables.date_gm
having country is not null
order by fullvisitorid

-- checking the country with the highest revenue
select country,sum(t_revenue) from tbl_revenue group by country order by  sum(t_revenue) desc limit 1
-- checking the city with the highest revenue
select city,sum(t_revenue) from tbl_revenue where city !='not available in demo dataset' group by city order by  sum(t_revenue) desc limit 1


Answer:
"United States"	16646430000
"San Francisco"	1877510000    


**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
select country,city,avg(productquantity) :: integer as avg_quantity
from all_sessions 
where productquantity is not null
group by country,city
order by avg_quantity desc


Answer:
"Spain"	"Madrid"	10

**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
select country,city,v2productcategory,count(v2productcategory) as order_freq
from all_sessions
where productquantity is not null
group by country,city,v2productcategory
order by order_freq desc


Answer:
no clear preferencies. The max number is 3 per category for USA, Mountain View




**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
select country,productsku, count(productsku) as top_sell
	from all_sessions
	where totaltransactionrevenue is not null
	group by country,productsku
	order by top_sell desc



Answer:
	termostat is most selling product in US



**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

	select country,sum(t_revenue)/(select sum(t_revenue) from tbl_revenue)*100::integer as ratio
	from tbl_revenue
	group by country
	order by ratio desc


Answer:
US has the most impact. its share is 93%.






