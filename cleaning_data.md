What issues will you address by cleaning the data?
1) Checking for duplicated 'sku' in products table (answer: no duplicates)
2) Standartizing sku's
2.1) Noticed that there 'sku's without letters and some of them are shorter/longer than in general. After checking the number of characters decided to stop trying to standardize them as finding a pattern seemed time-consuming to me and I didn't really think that it will have a huge impact on the validity of the data.
2.2) Checking for nulls (answer: one result null in sentimentscore and sentimentmagnitude )
2.3) Converting null to 0 which means neutral  sentiment
2.4) Noticed that some product_names start with a space and removed them (run query to avoid any spaces in fronal or rearal parts of the string (804 rows trimmed)
     




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
    
