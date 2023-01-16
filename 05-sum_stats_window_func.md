# PostgreSQL Summary Stats and Window Functions

This course covers:

* Introduction to window functions
* Fetching, ranking and paging
* Aggregate window functions and frames
* Beyond window functions

<br />

## Summary

| Keyword | Use |
| ---: | :--- |
| ROW_NUMBER() | window function that adds a index-like column $(n+1)$ |
| LAG(*column*, *n*) | returns *column*'s value *n* rows before the current row. *n* defaults to 1|
<br />

# Introduction to window functions

### Window functions:

* Perform an operation across as et of rows that are somehow related to the 
current row
* Similar to **GROUP BY** aggregate functions, but all rows remain in the output

### Uses:

* Fetch values from preceding or following rows (e.g. fetching the previous row's
value)
* Addign ordinal ranks (1st, 2nd, etc.) to rows based on their values' positions
in a sorted list
* Running totals and moving averages

### Examples:

* ROW_NUMBER() .- adds an index column to the output, i.e. a column that will
follow $n + 1$ algorithm for each row.
* LAG(*column*, *n*) .- returns *column*'s value *n* rows before the current row.
*n* defaults to 1.

```sql
/* Return year, name of the event and athlete's country from summer sports
database for first places*/
SELECT
    ev_year, event_name, country,
    ROW_NUMBER() OVER() AS row_n
FROM summer_medals
WHERE medal = 'Gold';
```

### Anatomy of window function

* FUNCTION_NAME() OVER(...) AS alias
    * OVER(...) can contain statements such as:
        * ORDER BY
        * PARTITION BY
        * ROWS/RANGE PRECEDING/FOLLOWING/UNBOUNDED

## OVER(ORDER BY)

This subclause of OVER, orders the rows related to the current row by one or
more specified fields. 

NOTE: It will order the data automatically just as if the ORDER BY clause was at
the end as long as there's not another ORDER BY clause outside the OVER()
statement.

```sql
/* This query will sort the data by year and event and assign the index
column based on this new order*/
SELECT
    ev_year, event_name, country,
    ROW_NUMBER() OVER(ORDER BY ev_year DESC, event_name) AS row_n
FROM summer_medals
WHERE medal = 'Gold';
```

## OVER(PARTITION BY)

It splits the table into partitions based on a column's unique values, similar
to GROUP BY. We can think of it as an expanded GROUP BY statement.

* PARTITION BY affects the functions we've seen before:
    * ROW_NUMBER() will reset for each partition
    * LAG() will return the same value as the previous row only if it belongs to
    the same partition. 

```sql
/* This query will return the reset the index for each different event*/
SELECT
    ev_year, event_name, country,
    ROW_NUMBER() OVER(
        PARTITION BY event_name
        ORDER BY event_name ASC) AS row_n
FROM summer_medals
WHERE medal = 'Gold';
```

<br />

# Fetching, ranking and paging

There are 2 types of fetching functions: **relative** and **absolute**, the first
one is dependent on the current row, while the second depends on the table or 
partition. 

| Type | Function | Use |
| --- | --- | --- |
| Relative | LAG(*column*, *n*)   | returns row value *n* times before |
| Relative | LEAD(*column*, *n*)  | returns row value *n* times after |
| Absolute | FIRST_VALUE(*column*)| returns first value of table/partition |
| Absolute | LAST_VALUE(*column*) | returns last value of table/partition |

```sql
/* Year and city of event in the first 2 columns, the first and the last 
city of the table in the last 2 columns */
SELECT
    ev_year, city,
    FIRST_VALUE(city) OVER(
        ORDER BY ev_year ASC
    ) AS first_city
    LAST_VALUE(city) OVER(
        ORDER BY ev_year ASC
        RANGE BETWEEN
            UNBOUNDED PRECEDING AND
            UNBOUNDED FOLLOWING
    ) AS last_city
FROM hosts
ORDER BY ev_year ASC
```

Note: By default, the **LAST_VALUE()** will be considered as the current row. The
**RANGE BETWEEN** clause extends the window to the end of the table or partition. 