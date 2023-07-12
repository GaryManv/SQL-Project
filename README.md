# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals

To check my ability to transform and analyse provided dataset. The goal is to process and show each step from data load to continues QA
## Process

Step1. Loaded csv files into Database as per instructions.

Step2. Data cleaning. For all tables was checking 
- the duplicates (rows and columns),
- nulls and negative values,
- values are in it's range (e.g. sentimentscore)
- different data types
- similar names with "typos" (Refferal and refferal)
- data values mathing to the pattern (sku lenght),
- extra spaces,
- matching similar information in different tables
- matching comparable data in the same table (e.g visitstarttime and orderdate)
- existance of the second step without existing first step (e.g. customer with null time on web site making order)

## Results
Step1.uploaded files
Step2. 
- duplicated rows were removed from analytics table; itemquantity, itemrevenue and transactionrevenur  columns were removed from all_sessions table
- negative data wasn't found; in product table Null value for sentimentscore was updated to 0.
- sentimentscore and sentimentmagnitude are in the range -1 to 1 and 0 to 100 accordingly
- different data types : not found
- typos : not found
- matching pattern: lengths for values in products.sku are different
- extra spaces: found and removed from products.product_name
- matching tables: sku's in sales_report and products are matching
- visitstarttime = orderdate, no differencies
- found 20 visits where items were sold without spending time on the site

## Challenges 

Step1.System was requesting autorisation for the path.After changing the path still was getting the same error. The bug was in my coding: scv was mentioned instead of csv 
Step2. 


## Future Goals
(what would you do if you had more time?)
