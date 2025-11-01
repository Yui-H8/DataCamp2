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
```sql
-- Count the number of null values in the ticker column
SELECT count(*) - count(ticker) AS missing
  FROM fortune500;
```
