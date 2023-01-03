# Intermediate SQL

## Summary

| Keyword | Use |
| ---: | :--- |
| WHERE | adds a condition to filter the records |
| HAVING | adds a condition to filter records **when they are filtered** |
| % | *regex*: wildcard to match zero-to-many characters |
| _ | *regex*: wildcard for one charcacter |
| IN | allows to use a list in a condition |
| LIKE | used to compare text formats in the WHERE clause |
| AVG() | returns the average value of a field |
| SUM() | returns the result of adding all values on a field |
| MIN() | returns the lowest value of a field |
| MAX() | returns the highest value of a field |
| COUNT () | returns the number of records with a value on a field |
| ROUND(*value, decimals*) | rounds a *value* to the specified *decimals* |
| ORDER BY | sorts results by one or more fields; sorts in *ascending* by default |
| DESC | used after the field name in the ORDER BY statement, changes sorting to be *descending* |

<br />

### Count()

This will return the number of records **with a value** on a field. To use count
on more than one field you have to use the function multiple times.

Using the function with an asteriks (*) will return the **number of records** 
in a table.

It can also be used with the DISTINCT keyword to return the count of unique 
values on a field. 

```sql
-- for one column, number of values
SELECT COUNT(field_name) AS count_field_name
FROM table_name;

-- for two or more columns, number of values
SELECT COUNT(field_1) AS count_field_1, COUNT(field_2) AS count_field_2
FROM table_name;

-- total number of records in a table
SELECT COUNT(*) AS total_records
FROM table_name;

-- distinct count
SELECT COUNT(DISTINCT field_name) AS count_distinct_field
FROM table_name;
```

### SQL order of execution

Unlike other programming languages, SQL is not executed in the order it's 
written. It follows these steps:

1. FROM
2. WHERE
3. GROUP BY
4. HAVING 
5. SELECT
6. ORDER BY
7. LIMIT

<br />

## Filtering

```sql
-- Multiple filters example
SELECT field_1, field_2, field_3
FROM table_name
WHERE (field_1 <> 'condition' OR field_2 = 10)
    AND field_2 BETWEEN 5 AND 75;
```

### Filtering numbers

Arithmetic operators:

| Sign | Operation |
| :---: | --- |
| < | less than or before |
| <= | less than or equal to |
| = | equals, it can also be used for *'strings'* |
| >= | greater than or equal to |
| > | greater than or after |
| <> | not equal |

### Multiple filters (*AND, OR, BETWEEN*)

Multiple filters can be set using the **AND, OR** or **BETWEEN** keywords 
depending on what we're trying to retrieve. 

If a query has multiple filtering conditions we will need to enclose each
condition in parenthesis (refer to multiple filters example code block above).

### Filtering text

To filter string data we can use the following keywords:

| Keyword | Operation |
| --- | --- |
| LIKE | equals (works the same as the = operator) |
| NOT LIKE | not equals/different from |
| IN | allows us to specify multiple values (of any type) in a WHERE clause, works as multiple OR conditions |

```sql
-- wildcard for string starting with 'Ade'
SELECT field
FROM table_name
WHERE field LIKE 'Ade%'

-- wildcard for a 3 character string ending with 've'
SELECT field
FROM table_name
WHERE field LIKE '_ve'

-- using a list in condition
SELECT field
FROM table_name
WHERE field in ('string_1', 'string_2', 'string_3')
```

*Remember that strings are case-sensitive

### NULL values

In SQL, *null* represents a missing or invalid value. To handle these values,
we can use:

* **IS NULL** condition to return all NULL records of a filed
* **IS NOT NULL** condition for not NUll values

With these operations we can count the missing values in a dataset for example.

<br />

## Agregate functions

Performs a calculation on several values and return a single value. These
functions come after the SELECT clause.

| Function | Operation | Data types |
| ---: | --- | --- |
| COUNT() | returns the number of not null values | various |
| AVG() | returns the average value of a field | numeric only |
| SUM() | returns the result of adding all values on a field | numeric only |
| MIN() | returns the lowest value of a field | various |
| MAX() | returns the highest value of a field | various |
| ROUND(*value, decimals*) | rounds a *value* to the specified *decimals*. *Decimals* defaults to 0. If decimals is negative, it rounds to the left of the decimals, *i.e* ROUND(15908, -1) = 15,910 | numeric only |

```sql
-- Average value from a numeric field
SELECT AVG(field_name)
FROM table_name
```

## Arithmetic

We can perform arithmetic operations (+, -, * and /) in a query. This will
allow us to peform operations between different fileds.

```sql
-- Perform arithmetic operation
SELECT (1+2)

-- For divisions remember to use floats to get a float result
SELECT (1.0 / 3.0)

-- Operations between fields
SELECT (profit - cost) AS margin
FROM sales
```
<br />

## Sorting and grouping

### Sorting

Output data in a specific order. We can sort data by using **ORDER BY**, it will
sort results by one or more fields. 

It sorts in *ascending* by default (smallest to biggest, A-Z). It is possible to
sort by a field that's not being selected, although it is not recommended. 

When sorting by multiple fields, it will respect the given order of the fields.
Also, if we want to sort one field in ascending order and the other one in 
descending order we can specify this parameter for each field (look example 
below).

```sql
-- Sort in ascending order
SELECT field_1, field_2
FROM table_name
ORDER BY field_1;

-- Sort in descending order
SELECT field_1, field_2
FROM table_name
ORDER BY field_1 DESC;

-- Sort by multiple fields
SELECT field_1, field_2
FROM table_name
ORDER BY field_1, field_2 DESC;
```

### Grouping

Allows us to summarize data for a particular group of results. It's commonly
used with aggregate functions.

NOTE that SQL will return an error if we try to SELECT a field that is not in 
our GROUP BY clause. We'll need to correct this by adding an aggregate function
around that field.

We can GROUP BY multiple fields, and the order in which we group the fields 
changes the results. 

```sql
-- Group a result by a field and sort it by the aggregate function result
SELECT text_field, SUM(num_field) as field_sum
FROM table_name
GROUP BY text_field
ORDER BY field_sum DESC;
```

### Filtering and grouping

**WHERE can't be used for grouped records**, so we use **HAVING** instead. It
works the same way.

We could say that:
* **WHERE** filters individual records, while
* **HAVING** filters grouped records

```sql
-- Return movies grouped by release year with a duration over 100 mins
SELECT release_year, AVG(duration) AS avg_duration
FROM films
GROUP BY release_year
HAVING AVG(duration) > 100; -- since SELECT executes after HAVING, we can't 
                            --reference the alias.

-- Query using all learned so far
SELECT release_year, AVG(budget) AS avg_budget, AVG(gross) AS avg_gross
FROM films
WHERE release_year > 1990
GROUP BY release_year
HAVING AVG(budget) > 60000000
ORDER BY avg_gross DESC
LIMIT 1;                           
```
