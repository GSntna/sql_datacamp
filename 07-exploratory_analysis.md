# Exploratory Data Analysis in SQL

The objective of this course is to use what we've learn so far to explore a new
database.

* Basic exploration
* Temp tables
* Text and date handling

<br />

## Summary

| Keyword | Use |
| ---: | :--- |
| ```COALESCE(col_1, col_2, ...)``` | checks the arguments in order and returns the first non-NULL if one exists |
| ```min(), max()``` | min and max of a numeric column |
| ```avg()``` | average |
| ```percentile_disc(.5) WITHIN GROUP (ORDER BY val)``` | returns the median value (the element in the middle) |
| ```percentile_cont(.5) WITHIN GROUP (ORDER BY val)``` | returns the calculated,median (average between the values in the middle, even if the element doesn't exist on the column)
| ```var_pop(), var_samp()``` | population and sample variance |
| ```stddev_pop(), stddev_samp()``` | population and sample standard deviation |
| ```corr(col1, col2)``` | returns the correlation between 2 columns |
| ```round(value, decimals)``` | rounds a value to the specified decimal places |
| ```TRUNC(value, decimals)``` | keeps only the specified decimals (DOESN'T ROUND), ex. ```TRUNC(42.1256, 2): 42.12```, ```TRUNC(12345, -3): 12000``` |
| ```generate_series(start, end, step)``` | generates series with specified parameters |
| ```LOWER(text), UPPER(text)``` | change the casing of a given text; they have no effects on punctuations or numbers |
| ```TRIM(text, chars)``` | removes the given chats from the end and start of the
given text. Chars defaults to whitespace |
| ```SUBSTRING(string FROM start FOR length)```| retruns a substring of length "length" in "string" from char in position "start" |
| ```SPLIT_PART(string, delimiter, part)``` | splits the string in N parts by the given delimiter and returns the part in position "part" given in the function |
| ```CONCAT(t1, t2, ...)``` | concatenates given arguments as a string |


<br />

## Reminders

| Code | Note |
| ---: | :--- |
| ```NULL``` | missing |
| ```IS NULL```, ```IS NOT NULL```| don't use  ```= NULL``` |
| ```count(*)``` | number of rows |
| ```count(column_name)``` | number of non-NULL values |
| ```count(DISTINCT column_name)``` | number of different non-NULL values |
| ```SELECT DISTINCT column_name ... ``` | distinct values, including NULL |


## Basic Exploration

### HEAD

Look at the head (first rows) of a table:

```postgresql
SELECT *
FROM table,
LIMIT 5;
```

### Understading the links between tables

* **Foregin keys**: reference another row in a different table or the same table
via a unique id. They are restricted to values ina  referenced column or
```NULL```, wich indicates there is no relationship.
* **Primary keys** are specially designated columns where each row has a unique
non-null value.

### Exploring distributions

We can use the ```TRUNC(value, decimals)``` and the
```generate_series(start, end, step)``` functions to get the distribution of a
column.

```postgresql
-- get a column with the start of the range and
      -- another with the frequency
SELECT trunc(unanswered_count, -1) as trunc_ua, count(*)
FROM stackoverflow
WHERE tag='amazon-ebs'
GROUP BY trunc_ua
ORDER BY trunc_ua;
```

## Temp table

A table that lasts for the duration of a session. We can create, insert
information and eliminate a temp table using the follwing sintax:

```postgresql
/* CREATE TABLE */
-- create table as
CREATE TEMP TABLE temp_tablename AS
-- query results to store in the table
SELECT col1, col2
FROM my_table;

-- select existing columns
SELECT col1, col2
-- clause to direct results to a new temp table
INTO TEMP TABLE temp_tablename
-- existing table with existing columns
FROM my_table;

/* INSERT INFO INTO TEMP TABLE */
INSERT INTO temp_tablename
SELECT col1, col2
FROM my_table
WHERE col2 IN (val1, val2, val3);

/* DELETE TEMP TABLE */
DROP TABLE temp_tablename;
-- confirm if it exists first (recommended)
DROP TABLE IF EXISTS temp_tablename;
```

## Exploring categorial data and unstructured text

Text types can be either:

* Categorical: Fixed number of classifications
* Unstructured text: text as opininions, reviews, phrases, that do not follow
a determined list of parameters

### Common issues when working with text

#### Casing

The ```WHERE``` and ```LIKE``` operators do not ignore casing. To go around this
issue we can follow these strategies:

* use the ```LOWER()``` and ```UPPER()``` functions, making comparisons
case insensitive.
* use ```ILIKE()``` for **I**nsensitive searches. Note that these queries
take longer to run.

```postgresql
SELECT *
FROM fruits
WHERE fruit ILIKE '%apple%';
```

#### Whitespaces

We can remove whitespaces from the start and end of a string using the
```TRIM()``` function. We can also use ```RTRIM()``` and ```LTRIM()``` to remove
whitespaces from the right or the left side of the string only.

```TRIM('Hello!', '!Hho'): ell``` also allows to specify the character or
characters we want to remove (whitespace is default).

#### Splitting and concatenating text

```postgresql
/* RIGHT and LEFT */
SELECT left('abcde', 2),  -- first 2 chars
      right('abcde', 2)   -- last 2 chars

/* SUBSTRING */
SELECT SUBSTRING('abcdef' FROM 2 FOR 3)
-- bcd

/* SPLITTING ON DELIMITER */
SELECT split_part('a,bc,d', ',', 2)
-- bc

/* CONCATENATION */
SELECT concat('a', 2, 'cc')
SELECT 'a' || 2 || 'cc'      -- same effect
```

#### Update values using a mapping temporary table

```postgresql
/* CREATE TEMP TABLE */
CREATE TEMP TABLE recode AS
      SELECT DISTINCT fruit AS original,
            fruit As standardized
      FROM fruits

/* UPDATE standardized COLUMN */
-- lower case and remove whitespace on ends
UPDATE recode
      SET standardized=trim(lower(original))

-- correct misspelling on specific rows
UPDATE recode
      SET standardized='banana'
      WHERE standardized LIKE '%nn%'

-- remove any 's' in the end
UPDATE recode
      SET standardized=rtrim(standardized, 's')

-- code to join to the original table and get the standardize values
```

## Dates

To add to the date section in [06-functions](https://github.com/GSntna/sql_datacamp/blob/main/06-funcionts.md).

```postgresql
-- Generate series of dates between an interaval:
SELECT generate_series('2018-02-01',
                        '2018-02-01'
                        '1 month'::interval) -- 1 month between dates
```

### Time between events

When we have a single column with dates, and we want to know the difference
between them (for example, a table that registers the time of a sale in one
column, and the amount in another) we can use the ```lag``` and ```lead```
functions to get the difference:

```postgresql
SELECT date,
      date - lag(date) OVER (ORDER BY date) AS gap
FROM sales;

-- get average gap
SELECT avg(gap)
FROM (SELECT date,
            date - lag(date) OVER (ORDER BY date) AS gap
      FROM sales) AS gaps;
```