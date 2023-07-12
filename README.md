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

Step3. Answering questions in starting_with_questions.md

Step4. Creating my own questions in starting_with_data.md

Step5. Developing QA process with queries checking data conformity to the stated rules. 

Step6. Creating and uploading ERD schema.png to github (SQL-Project)

## Results
Step1.uploaded files
Step2. 
- duplicated rows were removed from analytics table; itemquantity, itemrevenue and transactionrevenur  columns were removed from all_sessions table; removed unit_price from analytics table
- negative data wasn't found; in product table Null value for sentimentscore was updated to 0.
- sentimentscore and sentimentmagnitude are in the range -1 to 1 and 0 to 100 accordingly
- different data types : not found
- typos : not found
- matching pattern: lengths for values in products.sku are different
- extra spaces: found and removed from products.product_name
- matching tables: sku's in sales_report and products are matching
- visitstarttime = orderdate, no differencies
- found 20 visits where items were sold without spending time on the site

Step3. Answers (queries) are in starting_with_questions.md 

Step4. Answers (queries) are in  in starting_with_data.md

Step5. 

Step 6. Schema.png was uploaded to github

## Challenges 

Step1.System was requesting autorisation for the path.After changing the path still was getting the same error. The bug was in my coding: scv was mentioned instead of csv  

Step2.   
-The main challange is that the provided database is not relational.So you're trying to check data existing in 2 tables overlapping each other (all_sessions and analytics.  
- Rising/finding the right question
- Lack of information

Step3.
Choosing between revenues presented in different tables and columns.   
What to do with nulls? Without them just 80 rows which is very small quantity to analyse.

Step4. Choosing interesting question and trying to use window function

Step5. Queries were similar to ones I used while cleaning the data in step2.  
Lack of time

Step 6. Schema.png was uploaded to github



## Future Goals
I would probably try to make the database relational. Create new tables like visitors, channelgrouping, orders, etc.
Removing columns or transfer them to the other more relevant tables. E.g. ordered quantity from products table can be removed or transfered to orders table
