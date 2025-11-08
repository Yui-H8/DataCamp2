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
```sql
-- Divide 10 by 3
SELECT 10/3,
       -- Cast 10 as numeric and divide by 3
       10::numeric/3;
```
```
?column?	?column?
3	3.3333333333333333
```
3. Now cast numbers that appear as text as numeric. Note: 1e3 is scientific notation.
```sql
SELECT '3.2'::numeric,
       '-123'::numeric,
       '1e3'::numeric,
       '1e-3'::numeric,
       '02314'::numeric,
       '0002'::numeric;
```
```
numeric	numeric	numeric	numeric	numeric	numeric
3.2	-123	1000	0.001	2314	2
```
*Good job! Note that numbers cast as integer are rounded to the nearest whole number and division produces different results for integer values than for numeric values.*

### Summarize the distribution of numeric values
Was 2017 a good or bad year for revenue of Fortune 500 companies? Examine how revenue changed from 2016 to 2017 by first looking at the distribution of revenues_change and then counting companies whose revenue increased.
1. Use GROUP BY and count() to examine the values of revenues_change. Order the results by revenues_change to see the distribution.
```sql
-- Select the count of each value of revenues_change
SELECT revenues_change, COUNT(*)
  FROM fortune500
 GROUP BY revenues_change
  -- order by the values of revenues_change
 ORDER BY 1 DESC;
```
2. Repeat step 1, but this time, cast revenues_change as an integer to reduce the number of different values.
```sql
-- Select the count of each revenues_change integer value
SELECT revenues_change::integer, COUNT(*)
  FROM fortune500
 GROUP BY revenues_change::integer
 -- order by the values of revenues_change
 ORDER BY 1 DESC;
```
3. How many of the Fortune 500 companies had revenues increase in 2017 compared to 2016? To find out, count the rows of fortune500 where revenues_change indicates an increase.
```sql
-- Count rows 
SELECT COUNT(*)
  FROM fortune500
 -- Where...
 WHERE revenues_change > 0;
```
*You got it. Examining distributions and counting observations of interest are two first steps in exploring data. In the next chapter, we'll learn other functions and approaches for summarizing numeric data.*

### Division
* Compute revenue per employee by dividing revenues by employees; casting is used here to produce a numeric result.
* Take the average of revenue per employee with avg(); alias this as avg_rev_employee.
* Group by sector.
* Order by the average revenue per employee.
```sql
-- Select average revenue per employee by sector
SELECT sector, 
       avg(revenues/employees::numeric) AS avg_rev_employee
  FROM fortune500
 GROUP BY sector
 -- Use the column alias to order the results
 ORDER BY avg_rev_employee DESC;
```
*Sensational! You know to watch out for integer division problems, and that ordering your query results by the value of interest will help you make sense of the results.*

### Explore with division
In exploring a new database, it can be unclear what the data means and how columns are related to each other.

What information does the unanswered_pct column in the stackoverflow table contain? Is it the percent of questions with the tag that are unanswered (unanswered ?s with tag/all ?s with tag)? Or is it something else, such as the percent of all unanswered questions on the site with the tag (unanswered ?s with tag/all unanswered ?s)?

Divide unanswered_count (unanswered ?s with tag) by question_count (all ?s with tag) to see if the value matches that of unanswered_pct to determine the answer.
* Exclude rows where question_count is 0 to avoid a divide by zero error.
* Limit the result to 10 rows.
```SQL
-- Divide unanswered_count by question_count
SELECT unanswered_count/question_count::numeric AS computed_pct, 
       -- What are you comparing the above quantity to?
       unanswered_pct
  FROM stackoverflow
 -- Select rows where question_count is not 0
 WHERE question_count != 0
 LIMIT 10;
```
*Super! The values don't match. unanswered_pct is the percent of unanswered questions on Stack Overflow with the tag, not the percent of questions with the tag that are unanswered.*

### Summarize numeric columns
Summarize the profit column in the fortune500 table using the functions you've learned.

You can access the course slides for reference using the PDF icon in the upper right corner of the screen.
1. Compute the min(), avg(), max(), and stddev() of profits; don't use any aliases here.
```sql
-- Select min, avg, max, and stddev of fortune500 profits
SELECT min(profits),
       avg(profits),
       max(profits),
       stddev(profits)
  FROM fortune500;
```
2. Repeat Step 1, but this time, creating a grouped summary of profits by sector, ordering the results by the average profits for each sector; don't use any aliases here.

> Hint    
> Remember to select sector so you know which values go with which sector.    
> To order by the average, you can order by avg, which is the name the average column will have in the result.
```sql
-- Select sector and summary measures of fortune500 profits
SELECT sector,
       min(profits),
       avg(profits),
       max(profits),
       stddev(profits)
  FROM fortune500
 -- What to group by?
 GROUP BY sector
 -- Order by the average profits
 ORDER BY avg;
```

### Summarize group statistics
Sometimes you want to understand how a value varies across groups. For example, how does the maximum value per group vary across groups?

To find out, first summarize by group, and then compute summary statistics of the group results. One way to do this is to compute group values in a subquery, and then summarize the results of the subquery.

For this exercise, what is the standard deviation across tags in the maximum number of Stack Overflow questions per day? What about the mean, min, and max of the maximums as well?
* Start by writing a subquery to compute the max() of question_count per tag; alias the subquery result as maxval.
* Then compute the standard deviation of maxval with stddev().
* Compute the min(), max(), and avg() of maxval too.
> Hint
> Make sure to use the stddev() function and not one of its alternatives.    
> maxval should be the argument to all of the functions in the outer query.
```sql
-- Compute standard deviation of maximum values
SELECT stddev(maxval),
       -- min
       min(maxval),
       -- max
       max(maxval),
       -- avg
       avg(maxval)
  -- Subquery to compute max of question_count by tag
  FROM (SELECT max(question_count) AS maxval
          FROM stackoverflow
         -- Compute max by...
         GROUP BY tag) AS max_results; -- alias for subquery
```
```
stddev	min	max	avg
176458.37952720	30	1138658	52652.433962264151
```
*Great job summarizing! A subquery was necessary here because the tag maximums must be computed before you can summarize them.*

---
### Truncate
