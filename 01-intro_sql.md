# Intro SQL

## SQL

* Short for Structured Query Language
* Most widely used programming language for databases

### Tables

* Organized in rows (*records*) and columns (*fields*). 
* Fields are set at database creation; there's no limit to number of records

### Syntax rules

table names should be:

1. lowercase
2. have no spaces - use underscore ( _ )
3. refer to a collective group or be in plural

field names should be:

1. lowercase
2. have no spaces - use underscore ( _ )
3. be singular
4. be different from other filed names
5. be different from the table names

### Unique identifiers

They are used to identify each record in a table, they must be **unique**. They
are often numbers. 

### SQL data types

when a table is created, a data type must be indicated for each field. The data
type of a filed influences on the storage space. Some of the data types that can
be stored in a database are:

* Integers (INT)
* Floats (NUMERIC)
* Strings (VARCHAR)
* Dates

When you look at a **database schema** you can see the tables, the datatype of 
each field and the relationships between the tables. 

<br />

# SQL Queries

### Keywords

*Keywords* are reserved words for operations. *Ex.* SELECT, FROM, LIMIT, JOIN,
AS, WHERE, etc. These words shouldn't be used for table or field names. 

In SQL, *keywords are usually capitalized*.

## Querying

### Return data from table

To return **all** the data from a field in a table, we use SELECT and FROM.

```sql
SELECT field_name
FROM table_name;
```

### Aliasing 

```sql
SELECT field_1 AS alias_1, field_2
FROM table_name;
```

### Unique records

To get all the unique values of a field (without duplicated values), we use
the DISTINCT keyword. 

```sql
SELECT DISTINCT field_name
FROM table_name;
```
To return the *unique combinations of multiple fields* we can list multiple 
fields after the SELECT DISTINCT command.

```sql
-- return unique combinations
SELECT DISTINCT field_1, field2
FROM table_name;
```

## Views

* A *view* is a virtual table (not stored in the database)that is a result of a 
saved SQL SELECT statement.
* When accessed, views automatically update in response to updates in the
underlying data. 
* Once a view is created, we can query it just as we would a normal table. 

```sql
-- Creating the view
CREATE VIEW view_name AS
SELECT *
FROM table_name

-- Interacting with the view
SELECT field_1, field_2
FROM view_name
```
<br />

## Summary

| Keyword | Use |
| --- | --- |
| SELECT | **selects the fields** that we want to return. Use the *asterisk (\*)* to bring all the fields of the table |
| DISTINCT | returns the unique values of the field(s) |
| AS | gives an alias to a field, the field can be referenced by this alias in the query |
| FROM | specifies the table where we're taking the fields from |
| CREATE VIEW | creates a virtual table from a query and stores it in memory |
| ; | the semicolon should be used at the end of every query |