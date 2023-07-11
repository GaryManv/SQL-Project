What issues will you address by cleaning the data?
**products table**
1) Checking for duplicated 'sku' in products table (answer: no duplicates)
2) Standartizing sku's
2.1) Noticed that there 'sku's without letters and some of them are shorter/longer than in general. After checking the number of characters decided to stop trying to standardize them as finding a pattern seemed time-consuming to me and I didn't really think that it will have a huge impact on the validity of the data.
2.2) Checking for nulls (answer: one result null in sentimentscore and sentimentmagnitude )
2.3) Converting null to 0 which means neutral  sentiment
2.4) Noticed that some product_names start with a space and removed them (run query to avoid any spaces in fronal or rearal parts of the string (804 rows trimmed)
2.5) To check if sentimentscore is between -1 and 1 and sentimentmagnitude is between 0 and 100 (answer : no )
2.6) To check negative values for orderquantity, stocklevel and restockingleadtime (answer: no)
**sales_by_sku table**
3.1) checking nulls and <0 (answer : no)
3.2) checking if there are skus in sales_by_sku which are not exist in products table (answer: no)
**sales_report**
4.1) checking if there are skus in sales_report which are not exist in products table (answer: no) 
4.2) checking if there are values in columns in sales_reports which are identical with same columns in products table.(answer: no)
4.3) checking if ratio values are calculated correctly (answer: yes)
   
**analytics**
5.1) checking for columns with null for every value (answer: userid column can be removed)
5.2) removing duplicate rows filtering by visitid (unique id number is 148625)
5.3) converting date integer to date format
5.4) checking similar names with typos or case difference in columns 
	- channelgrouping:good
	- socialEngagementType: good (only one type, whats the reason to keep it?)
5.5) check if date and visitstarttime date are equal (answer : yes,ok)
5.6) check if visitor with null timeonsite buy something or visit some pages
5.7) All rows except unit_price are grouped. Unit_price column can be removed
5.8) to compare revenues in tables "analytics" and "all_sessions" create a table with fullvisitorid and total sales amount for each where nulls are filtered (299 rows). To check which value and from which table is more relieable.

** all_sessions **
6.1) check if all visitors ids' have the same length (answer: basic length has 19 digits, but there are also 18,17 and 16). As it doesn't appear in other tables I just ignore it for now and will not reduce the table rows.
6.2) check if all visitors ids' don't have duplicates.(answer: there are duplicates but it's not a visitor table it's just a combined data - so duplicates are fine.
  
Queries:
1)select sku, count(*)>1 as duplicate_sku from products group by sku having count(*)>1
2.1) select CHAR_LENGTH(sku), count(CHAR_LENGTH(sku)) from products group by CHAR_LENGTH(sku)
2.2) select * from products where orderedquantity is null 
							or stocklevel is null
							or restockingleadtime is null
							or sentimentscore is null
							or sentimentmagnitude is null
2.3)  UPDATE products SET sentimentscore = 0 WHERE sentimentscore IS NULL;
      UPDATE products SET sentimentmagnitude = 0 WHERE sentimentmagnitude IS NULL;
2.4) select product_name,length(product_name),trim(both from product_name),length(trim(both from product_name)) from products
2.4.1)update products set product_name = trim(both from product_name)
2.5) select * from products where (sentimentscore not between -1 and 1)	or (sentimentmagnitude not between 0 and 100)
2.6) select * from products where orderedquantity<0 or stocklevel<0 or restockingleadtime<0
***
3.1) select * from sales_by_sku where productsku is null or total_ordered is null or total_ordered<0
3.2) select sku,product_name,productsku,total_ordered 
	from products
	right join sales_by_sku on products.sku = sales_by_sku.productsku
	where productsku is null
***
4.1) select * 
	from products
	right join sales_report on products.sku = sales_report.productsku
	where productsku is null
 4.2) select * 
	from products
	join sales_report on products.sku = sales_report.productsku
	where products.stocklevel <> sales_report.stocklevel
	or products.restockingleadtime <> sales_report.restockingleadtime
	or products.sentimentscore <> sales_report.sentimentscore
	or products.sentimentmagnitude <> sales_report.sentimentmagnitude
4.3) 	select productsku,total_ordered,stocklevel,ratio,
		(case when stocklevel = 0 then null else cast(total_ordered as real)/stocklevel end) ratio_check
     	from sales_report
	where COALESCE(ratio,0) - COALESCE((case when stocklevel = 0 then null else cast(total_ordered as real)/stocklevel end),0)<>0
 ***
 5.1) select * from analytics where userid is not null
 	select * from analytics where timeonsite is not null
  	select * from analytics where units_sold is not null
  	select * from analytics where units_sold is  null
   5.1.1) delete column: alter table analytics2 drop column userid
5.2) checking duplicates: select distinct on (visitid) analytics.* from analytics order by visitid
5.2.1) deleting: CREATE TABLE analytics_temp (LIKE analytics);
		INSERT INTO analytics_temp(visitid,visitstarttime,date,userid,channelgrouping,socialengagementtype,units_sold,pageviews,timeonsite ,
    						bounces ,revenue, unit_price, visitnumber , fullvisitorid )
	select distinct on (visitid) visitid,visitstarttime,date,userid,channelgrouping,socialengagementtype, units_sold ,pageviews ,timeonsite ,bounces,
	 revenue ,unit_price ,visitnumber , fullvisitorid 
 	from analytics;

	
5.3) checking :select visitid,date, to_date(date :: text, 'YYYYMMDD') from analytics 
5.3.1)updating: update analytics set date_gm = to_date(date :: text, 'YYYYMMDD')

5.4) select distinct on (channelgrouping) analytics.channelgrouping from analytics 
5.5 )with date_check as (select visitid,date,to_date(date :: text, 'YYYYMMDD') AS	 
         		visit_date,
	   		visitstarttime,CAST(TO_TIMESTAMP(visitstarttime) AS DATE) as visit_start_date 
			from analytics)
     select visit_date,visit_start_date from date_check where visit_date<>visit_start_date
5.6) 	select distinct (visitid),timeonsite,units_sold
	from analytics
	where timeonsite is null and units_sold>0 or pageviews>0
 5.7) ALTER TABLE analytics DROP COLUMN unit_price
 5.8) create table visitor_by_amount as select fullvisitorid,sum(revenue) from analytics group by fullvisitorid having sum(revenue) is not null

 ***
 6.1) select length(fullvisitorid),count(length(fullvisitorid)) from all_sessions group by length(fullvisitorid)
 6.2) select fullvisitorid, count(*)>1 from all_sessions group by fullvisitorid having count(*)>1 



