# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals

To check my ability to transform and analyse provided dataset. The goal is to process and show each step from data load to continues QA
## Process

Step1. Loaded csv files into Database as per instructions.

Step2. Data cleaning. For all tables was checking 
2.1 the duplicates (rows and columns),
2.2 nulls and negative values,
2.3 values are in it's range (e.g. sentimentscore)
2.4 different data types 
2.5 similar names with "typos" (Refferal and refferal)
2.6 data values mathing to the pattern (sku lenght),
2.7 extra spaces,
2.8 matching similar information in different tables
2.9 matching comparable data in the same table (e.g visitstarttime and orderdate)
2.10 existance of the second step without existing first step (e.g. customer with null time on web site making order)

## Results
Step1.uploaded files
Step2. 
2.1 duplicated rows were removed from analytics table; itemquantity, itemrevenue and transactionrevenur  columns were removed from all_sessions table
2.2 negative data wasn't found; in product table Null value for sentimentscore was updated to 0.
2.3 sentimentscore and sentimentmagnitude are in the range -1 to 1 and 0 to 100 accordingly
- 

## Challenges 
Step1.System was requesting autorisation for the path.After changing the path still was getting the same error. The bug was in my coding: scv was mentioned instead of csv !

## Future Goals
(what would you do if you had more time?)
