What are your risk areas? Identify and describe them.
1) Duplicated rows
2) Wrong data values (e.g. the leght of visitorid)
3) Wrong data type
4) Checking outliers
5) Nulls for the whole column / row
6) different data in different tables for the same atribute



QA Process:
Describe your QA process and include the SQL queries used to execute it.

Checking risk areas described above and investigating any failed assertion

1) Duplicated rows
      select sku, 'duplicated_sku' as result from products group by sku having count(*)>1
   
2) Wrong data values
      select sku,'Check the lenght of sku: to be 14 char' as result from products where CHAR_LENGTH(sku)<>14

3) Wrong data type
     select visitid,'wrong date format' as result from analytics where cast(pg_typeof(visitstarttime)as text) = 'date'

4) Checking outliers
        
         select sku,'sentiment score is out of (-1 to 1) range' as result 
         from products
         where sentimentscore not between -1 and 1

         union all

         select sku,'sentimentmagnitude is out of (0 to 100) range' as result
          from products
         where sentimentmagnitude not between 0 and 100
      

