# Intermediate SQL

## Summary

| Keyword | Use |
| --- | --- |
| COUNT () | returns the number of records with a value on a field |
| WHERE | adds a condition to filter the records |
| % | *regex*: wildcard to match zero-to-many characters |
| _ | *regex*: wildcard for one charcacter |
| IN | allows to use a list in a condition |

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
3. SELECT
4. LIMIT

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