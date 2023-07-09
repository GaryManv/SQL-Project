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
4.3) checking if ratio values are calculated correctly
   
**analytics**
5.1) checking for columns with null for every value (answer: userid column can be removed)
5.2) remove the first row with duplicated visitorid keep the 

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
5.2)




