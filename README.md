# SQL Datacamp Course Notes

This space was created to store my course notes for the SQL Specialization in
Datacamp. Some kind of a Cheat Sheet.

<br />

## Content

0. Introduction to Statistics
    * Types of data
    * Measures of center
    * Measures of spread
    * Probability and distributions
    * Skewness & Kurtosis
    * Hypothesis testing
    * Correlation

1. Introduction to SQL
    * SQL Basic queries
    * VIEW
    * SELECT, FROM, DISTINCT, AS

2. Intermediate SQL
    * Filtering
    * Aggregate functions
    * Arithmetic operations
    * Sorting and grouping
    * Filtering grouped records
    * Order of statements execution
    * WHERE, HAVING, LIKE, ORDER BY, ROUND()

3. Joining data
    * JOINS (INNER, LEFT/RIGHT, OUTER, CROSS)
    * Sets (UNION/UNION ALL, INTERCECT, EXCEPT)
    * Subqueries (inside SELECT, FROM & WHERE)

4. Data manipulation
    * CASE statements
    * Subqueries (nested & correlated)
    * Common table expressions (CTE)
    * Window functions
    * Window partitions
    * Sliding windows

5. Summary stats and window functions
    * Intro to window functions
    * OVER statement: PARTITION BY, ORDER BY
    * Fetching (LAG, LEAD, FIRST_VALUE, LAST_VALUE)
    * Ranking (ROW_NUMBER, RANK, DENSE_RANK)
    * Paging (NTILE)
    * Aggregate window functions
    * Frames (ROWS BETWEEN, RANGE BETWEEN)
    * Pivoting
    * GROUP BY extensions: ROLLUP and CUBE
    * COALESCE (null values replacement)
    * STRING_AGG (column to string)

6. Functions for manipulating data
    * Getting information from INFORMATION_SCHEMA
    * Arrays (creation, nesting, accessing, filtering)
    * CASTing
    * DATE/TIME functions
        * EXTRACT()
        * DATE_PART()
        * DATE_TRUNC()
        * INTERVAL
    * TEXT manipulation
        * Casing
        * Concatenation
        * Replacing
        * Trimming and padding
    * FULL-TEXT SEARCH
    * PostgreSQL Extensions

7. Exploratory data analysis
    * Foreign and Primary keys
    * Exploring data distributions
    * TEMP TABLES
        * CREATE
        * INSERT INTO
        * DELETE
    * WHERE, LIKE and **ILIKE**
    * TEXT manipulation
        * LEFT() and RIGHT()
        * SUBSTRING()
        * SPLIT_PART() (on delimiter)
    * Time series generation (GENERATE_SERIES)
    * Get time between dates

<br />
<br />
<br />

## How to push to Github

```bash
# Check what changes were done
git status

# Select elements to commit
git add file_name.extension

# Opt1: Message that will be added when commited-pushed
git commit -m "message_to_display"

# Opt2: Message that will be added when commited-pushed (opens text
# editor to add commit message + commit description)
git commit -vs

# Save changes
git push
```