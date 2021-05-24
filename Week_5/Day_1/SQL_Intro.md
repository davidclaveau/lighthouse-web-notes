# SQL Intro

* Why do we keep backend and databases separate?
  * You can scale the database independently from the back-end/front-end, or vice-versa

* Database Management Software (DBMS)
  * Software that provides an efficient storage mechanisms that allows organization, manipulation, and protection of highly structured data
    * You can have three copies of your data running - so if one goes down, you can access and use the copies to replace the database.

  ---

## Relational Databases

* The data is broken down into tables with associations between them (relationships)
  * Each table is like a spreadsheet with columns and rows (but it's not a spreadsheet)

* SQL is a *declarative* language as opposed to an *imperative* language
  * State what we need in an English-language-type format
  * Will receive specific errors from the system

* Data Definition Language (DDL)
  * Create and modify the structure of a database
  * So commands like `DROP TABLE IF EXISTS` are considered *queries* but they are mostly categorized as DDL as they affect the structure of the data

* Data Manipulation Language (DML)
  * Operations to manipulate the data
  * `INSERT`, `RETRIEVE`, `DELETE`, `UPDATE` (CRUD)

* Data Control Language (DCL)
  * Grants priveledges to access other tables

* Transactiona Control Language (TCL)
  * Deals with transactiton within a database
  * Only commit executions if all transactions succeed.

---

## SQL Command Line

* `psql` is a command line command that gives you *another* command line, but this will be the `PostgresSQL` command line
  * Use `help` and follow the options to learn more about the commands
  * `\i` is a command that executes a file, such as a script that resets a database
  * `\dt` to show a list of tables and relationships
  * `\c` to connect to a different database

### SQL Statement

* `SELECT` statements chooses the columns
* `FROM` will specify the table
* `JOIN` will allow us to pull from two tables at once
* `ON`
* `WHERE` is the data criteria that we're specifying
  * Allows for booleans, such as `AND` and `OR`
* `GROUP BY` allows us to group the data by a dataset
* `HAVING` 
* `ORDER BY` is a sorting clause, how to order the data

```sql
SELECT column list, function(), function(), ...
FROM table1
INNER JOIN table2

ON table1.col1 = table2.col2

WHERE criteria for row selection
GROUP BY column list
HAVING ...
GROUP BY ...
```

* Some examples

### Aggregate functions like `count` or `max`
  * Selecting the `count` of `id`s in a table
  * (`LIMIT` will specify the number of rows)

```sql
SEKECT count(id)
FROM objectives LIMIT 1;
```

### `WHERE` clause lets us be specific with what we want
  * Differences in the quotes: `''` will search for the data, `""` will look for column name

```sql
SELECT question, answer
FROM objectives
WHERE type = 'performance';
```

### `WHERE` clause with `AND`

* Use `AND` to add more specifics to the search using booleans

```sql
SELECT question, answer
FROM objectives
WHERE type = 'performance' AND sort > 5;
```

### `JOIN`

* We can join the tables with one query (10:16)
* Different kinds of joins - specify what happens when I do or don't have a row that corresponds to the other table's row (and vice versa)
* Do I get a row in my result if one or more are missing
  * `INNER JOIN` - values that match in both tables; give me *only* the rows where there's matching data
  * `LEFT JOIN` - show all the rows (in the `FROM`) table A *regardless* of values matching to (the table using `JOIN`) table B
  * `RIGHT JOIN` - inverse of `LEFT JOIN`
  * `FULL JOIN` - to combine all values with matches and without
  * We can use `WHERE A.key IS NULL` or `WHERE B.key IS NULL` to not include the values that don't match

* `JOIN days ON objectives.day_id = days.id`
  * The column header now corresponds to what's being called in the `SELECT` clause
  * We specify that the `objectives`' `day_id` column will correspond to the `days`' `id` column

```sql
SELECT day_description, question, answer
FROM objectives
JOIN days ON objectives.day_id = days.day_mnemonic;
```

### `GROUP BY`

* Let's you create a query where we specify the rows that are showing from SELECT with a relationship
  * 

```sql
SELECT count(day_id)
FROM objectives
GROUP BY day_id;
```

* Result of a `GROUP BY` we can't use a `WHERE` clause, we would need to use `HAVING`

```sql
SELECT count(day_id)
FROM objectives
GROUP BY day_id;
HAVING count(day_id) > 5
```

---

## SQL Exercise

* Enitity Relationship Diagram (ERD) allows you to create a diagram that specifies the relationship between tables using "crows foot" notation

* Creating tables from the command line:

```
$ createdb april26rox
$ psql !$
```

```sql
CREATE TABLE sample_table
(
  id bigint,
  day_id character varying(5),
  type character varying(12),
  question text,
  answer text,
  sort smallint
);
```

* `\dt` to show the table we've created

