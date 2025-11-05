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
Recall that foreign keys reference another row in the database via a unique ID. Values in a foreign key column are restricted to values in the referenced column OR NULL.

Using what you know about foreign keys, why can't the tag column in the tag_type table be a foreign key that references the tag column in the stackoverflow table?
> answer: stackoverflow.tag contains duplicate values

*Brillant! Foreign keys must reference a column with unique values for each row so the referenced row can be identified.*

### Read an entity relationship diagram
The information you need is sometimes split across multiple tables in the database.

What is the most common stackoverflow tag_type? What companies have a tag of that type? (https://assets.datacamp.com/production/repositories/3567/datasets/be54d796caa17b30ad02d180eed75f067d4aa4f8/erdiagram.png)

To generate a list of such companies, you'll need to join three tables together.
1. First, using the tag_type table, count the number of tags with each type. Order the results to find the most common tag type.
```sql
-- Count the number of tags with each type
SELECT type, COUNT(type) AS count
  FROM tag_type
 -- To get the count for each type, what do you need to do?
 GROUP BY type
 -- Order the results with the most common tag types listed first
 ORDER BY count DESC;
```
2. Join the tag_company, company, and tag_type tables, keeping only mutually occurring records.
* Select company.name, tag_type.tag, and tag_type.type for tags with the most common type from the previous step.
```sql
-- Select the 3 columns desired
SELECT company.name, tag_type.tag, tag_type.type
  FROM company
  	   -- Join to the tag_company table
       INNER JOIN tag_company
       ON company.id = tag_company.company_id
       -- Join to the tag_type table
       INNER JOIN tag_type
       ON tag_company.tag = tag_type.tag
  -- Filter to most common type
  WHERE type='cloud';
```
*Superb! You could combine these steps in a single query by using a subquery in the WHERE clause instead of the value 'cloud'.*

### Coalesce
The coalesce() function can be useful for specifying a default or backup value when a column contains NULL values.

coalesce() checks arguments in order and returns the first non-NULL value, if one exists.
- coalesce(NULL, 1, 2) = 1
- coalesce(NULL, NULL) = NULL
- coalesce(2, 3, NULL) = 2

In the fortune500 data, industry contains some missing values. Use coalesce() to use the value of sector as the industry when industry is NULL. Then find the most common industry.
* Use coalesce() to select the first non-NULL value from industry, sector, or 'Unknown' as a fallback value.
* Alias the result of the call to coalesce() as industry2.
* Count the number of rows with each industry2 value.
* Find the most common value of industry2.
  
> Hint: To see the most common values, ORDER BY count DESC.
> You can use an alias in the GROUP BY clause.
```sql
-- Use coalesce
SELECT coalesce(industry, sector, 'Unknown') AS industry2,
       -- Don't forget to count!
       count(*) 
  FROM fortune500 
-- Group by what? (What are you counting by?)
 GROUP BY industry2
-- Order results to see most common first
 ORDER BY count DESC
-- Limit results to get just the one value you want
 LIMIT 1;
```
```
industry2	count
Utilities: Gas and Electric	22
```
*Terrific! coalesce is essential when the value you need could be in more than one column. In the next exercise, you'll use coalesce as part of a self join.*

### Effects of casting
When you cast data from one type to another, information can be lost or changed. See how the casting changes values and practice casting data using the CAST() function and > the :: syntax.
> SELECT CAST(value AS new_type);
> SELECT value::new_type;
1. Select profits_change and profits_change cast as integer from fortune500. Look at how the values were converted.
```sql
-- Select the original value
SELECT profits_change, 
	   -- Cast profits_change
       CAST(profits_change AS integer) AS profits_change_int
  FROM fortune500;
```
2. Compare the results of casting of dividing the integer value 10 by 3 to the result of dividing the numeric value 10 by 3.
