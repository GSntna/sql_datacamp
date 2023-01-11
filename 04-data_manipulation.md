# Data Manipulation in SQL

This course covers how to shape, transform and manipulate data using:

* CASE statements
* Simple subqueries
* Correlated subqueries
* Window functions

<br />

## Summary

| Keyword | Use |
| ---: | :--- |
| CASE | *if-else* statement in SQL, allows to return different values upon given conditions |
| WHEN *condition* THEN *result* | part of the CASE statement where we set the condition and the value to return |
| ELSE | final part of the CASE statement, returns the default value if no condition is met, defualts to *null* |
| END | keyword used to close a CASE statement, goes at the end of it | 

<br />

# CASE statements

* In SQL, CASE statements work as a *if-then-else* clause in other programing
languages.
* **CASE** statements are written inside the **SELECT** clause and have the 
following syntax:
    1. WHEN
    2. THEN
    3. ELSE
    4. END
* They can have multiple conditions using the keywords AND and OR
* We can also use **CASE** statements in the **WHERE** clause to filter data but
note that since **WHERE** is executed first, we'd have to repeat the same code
on the **SELECT** statemet.

```sql
-- Query to see x relationship towards y
SELECT
    num_x,
    num_y,
    CASE WHEN num_x > num_y THEN 'x bigger than y'
         WHEN num_y < num_y THEN 'x smaller than y'
         ELSE 'x and y are the same' AS result
FROM numbers;

-- CASE with multiple conditions to see if a team won or lose their matches
SELECT match_date, hometeam_id, awayteam_id,
    CASE WHEN hometeam_id = 8455 AND home_goal > away_goal
        THEN 'Madrid home win!'
    CASE WHEN away_id = 8455 AND home_goal < away_goal
        THEN 'Madrid away win!'
    ELSE 'Loss or tie' END AS outcome
FROM matches
WHERE hometeam_id = 8455 OR awayteam_id = 8455 -- important to filter only
                                               -- Madrid matches
```

### CASE statements with aggregate functions

We can also use a **CASE** statement to aggregate data based on the result of a
logical test. In the example below we take advantage that **ELSE** defaults to 
*null* to count the ocurrences of an event (*null* values aren't counted)

```sql
-- Count the wins of a team in a season
SELECT 
    season, 
    COUNT(CASE WHEN hometeam_id = 8650 AND home_goal > awaygoal
            THEN id END) AS home_wins,
    COUNT(CASE WHEN away_id = 8650 AND home_goal < awaygoal
            THEN id END) AS away_wins
FROM matches
GROUP BY season;
```
NOTE: In other languages such as Python and R, we can sum *boolean* values, in
SQL we have to use a **CASE** statement to return 1 and 0 after a logical test
and wrap it on a **SUM()** function.

<br />

# Subqueries

## Simple subquery

A query *nested* inside another query, that can be run on its own. 

* Useful for intermediary transformations
* It is evaluated only once in the entire statement, *i.e.*, SQL processes first
the subquery and then moves on to the query. 

### What can it do?

* Can be placed in any part of a query (SELECT, FROM, WHERE, GROUP BY)
* Can return a variety of information
    * Scalar quantities
    * Lists
    * Table

### Why to use it

* Comparing groups to summarized values
* Reshaping data
* Combining data that cannot be joined

## Subqueries in WHERE

Allows us to filter information, gives us the advantage to use information from
different tables without joining them, or even if they don´t hold a direct
relationship. 

```sql
-- Return all scores higher than the average score from group A
SELECT group, score
FROM scores
WHERE 
    num > (SELECT AVG(num)
        FROM scores
        WHERE group = 'A')

-- Return the student names that don't belong to group B using 2 tables
SELECT id, student_name
FROM students
WHERE id NOT IN
    (SELECT id
    FROM groups
    WHERE group != 'B')
```

## Subqueries in FROM

* Restructure and transform your data: 
    * we might want to transfrom data from long to wide before selecting
    * prefiltering data
* Calculating *aggregates of aggregates*
    * finding the max average of a field grouped by another field.
* You can create multiple subqueries in one **FROM** statement, but you have to
*alias them and have a column to join them*

```sql
/* Return country and number of matches for countries with matches with 10 or
more goals.*/
SELECT
	-- Select country name and the count match IDs
    name AS country_name,
    COUNT(c.id) AS matches
FROM country AS c
-- Inner join the subquery onto country
-- Select the country id and match id columns
INNER JOIN (SELECT country_id, id 
           FROM match
           -- Filter the subquery by matches with 10+ goals
           WHERE (home_goal + away_goal) >= 10) AS sub
ON c.id = sub.country_id
GROUP BY country_name;
```

## Subqueries in SELECT

* Used to return a signle aggregate value
    * Include aggregate values to compare to individual values
* Used in mathematical calculations
    * Deviation from the average

```sql
-- Get the deviation from the average
SELECT
    id,
    record_value,
    (record_value -
        (SELECT AVG(record_value)
        FROM observations)) AS deviation
FROM observations
```

## Best practices

* Properly line up our quries (indentation)
* Use comments:
```sql
/* BLOCK OF COMMENT: this comment can be in
more than one line */

-- Single line comments
```
* Use casing (CAPITALIZE keywords)

NOTE: **Holywell's SQL Style Guide** for all the formatting conventions.

### Remember that:

Subqueries require computing power so we should consider:

* How big is the database?
* How big is the table we're querying from
* Is the subquery actually necessary