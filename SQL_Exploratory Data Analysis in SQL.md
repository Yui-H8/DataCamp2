# Exploratory Data Analysis in SQL
---
### Explore table sizes
Count the number of columns in a table by selecting a few rows and manually counting the columns in the result.

Which table has the most rows?
*Great start! Knowing how much data you have is a first step in exploratory data analysis.*

### Count missing values
Which column of fortune500 has the most missing values? To find out, you'll need to check each column individually, although here we'll check just two: ticker and industry.

Course Note: While you're unlikely to encounter this issue during this exercise, note that if you run a query that takes more than a few seconds to execute, your session may expire or you may be disconnected from the server. You will not have this issue with any of the exercise solutions, so if your session expires or disconnects, there's an error with your query.

1. Subtract the count of the non-null ticker values from the total number of rows in fortune500; alias the difference as missing
> Hint   
> When you supply a column name as the input to count(), it returns the number of non-null values.    
> Aliasing a column by placing a new name after AS renames it.
```sql
-- Count the number of null values in the ticker column
SELECT count(*) - count(ticker) AS missing
  FROM fortune500;
```

2. Repeat for the industry column: subtract the count of the non-null industry values from the total number of rows in fortune500; alias the difference as missing.
```sql
-- Count the number of null values in the industry column
SELECT count(*) - count(industry) AS missing
FROM fortune500;
```
*Good work! ticker contained quite a few more missing values than industry; this is vital information as you begin to explore your data.*

### Join tables
Part of exploring a database is figuring out how tables relate to each other. The company and fortune500 tables don't have a formal relationship between them in the database, but this doesn't prevent you from joining them.

To join the tables, you need to find a column that they have in common where the values are consistent across the tables. Remember: just because two tables have a column with the same name, it doesn't mean those columns necessarily contain compatible data. If you find more than one pair of columns with similar data, you may need to try joining with each in turn to see if you get the same number of results.

Reference the entity relationship diagram (https://assets.datacamp.com/production/repositories/3567/datasets/be54d796caa17b30ad02d180eed75f067d4aa4f8/erdiagram.png) if needed.
* Closely inspect the contents of the company and fortune500 tables to find a column present in both tables that can also be considered to uniquely identify each company.
* Join the company and fortune500 tables with an INNER JOIN.
```sql
SELECT company.name
-- Table(s) to select from
  FROM company
       INNER JOIN fortune500
       ON company.ticker = fortune500.ticker;
```
*You got it! You can join tables when they share a column with consistent data values.*

### Foreign keys
