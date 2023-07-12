Question 1: 

Which product category is most profitable?

SQL Queries:

SELECT v2productcategory, SUM(totaltransactionrevenue) as profit
FROM ALL_SESSIONS 
group by v2productcategory 
having SUM(totaltransactionrevenue) is not null
order by profit desc
limit 1

Answer: 
"Home/Nest/Nest-USA/"	4355090000

Question 2: 

Present ratio of revenue per product group vs total revenue using window function (list the first 5)

SELECT v2productcategory,
		((SUM(totaltransactionrevenue)/SUM(SUM(totaltransactionrevenue)) over ())*100)::integer as ratio
		FROM ALL_SESSIONS 
group by v2productcategory 
having SUM(totaltransactionrevenue) is not null
order by ratio desc
limit 5

SQL Queries:

SELECT v2productcategory,
		((SUM(totaltransactionrevenue)/SUM(SUM(totaltransactionrevenue)) over ())*100)::integer as ratio
		FROM ALL_SESSIONS 
group by v2productcategory 
having SUM(totaltransactionrevenue) is not null
order by ratio desc
limit 5

Answer:

"Home/Nest/Nest-USA/"	30
"Nest-USA"	14
"Bags"	12
"Electronics"	8
"Home/Office/Notebooks & Journals/"	6

Question 3: 

--How much time people from different cities spend on the site on average? Rank it.

SQL Queries:

Select country,city,avg(timeonsite)::integer as avg_per_city,rank() over (order by avg(timeonsite)desc) as rank
from all_sessions
where timeonsite is not  null 
		and city !='not available in demo dataset' and city !='(not set)'
group by country,city
order by avg_per_city desc
limit 5

Answer:

"Japan"	"Sakai"	2382	1
"Ireland"	"Cork"	2366	2
"France"	"Villeneuve-d'Ascq"	2280	3
"France"	"Singapore"	2061	4
"United States"	"East Lansing"	1733	5

Question 4: 
--How much money people from different cities spend on the site on average? Rank it.

SQL Queries:

Select country,city,avg(totaltransactionrevenue)::integer as avg_rev_per_city, 
		rank () over (order by avg(totaltransactionrevenue) desc) as rank

from all_sessions
where totaltransactionrevenue is not  null 
		and city !='not available in demo dataset' and city !='(not set)'
--order by timeonsite desc
group by country,city
order by avg_rev_per_city desc
limit 5

Answer:
"Israel"	"Tel Aviv-Yafo"	602000000	1
"United States"	"Atlanta"	427220000	2
"Australia"	"Sydney"	358000000	3
"United States"	"Seattle"	358000000	3
"United States"	"Sunnyvale"	248057500	5


Question 5: 
--Does more profitable channel brings more people?

SQL Queries:
select channelgrouping,sum(totaltransactionrevenue) as revenue_by_channel,
                        count(fullvisitorid) as visits_by_channel
from all_sessions
group by channelgrouping
having sum(totaltransactionrevenue) is not null 

Answer: No. The web site gets more profit from "refferal" channel 6'018'680'000 UDS vs 2580 visits, while more people visits site from "Organic Search" 8653.

"Organic Search"	3163670000	8653
"Referral"	6018680000	2580
"Direct"	4704190000	2997
"Paid Search"	394770000	509
