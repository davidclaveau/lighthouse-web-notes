# Data Manipulation Language (DML)

* SQL commands that let us interact with our data are sometimes referred to as **Data Manipulation Language (DML)**. These include
  * SELECT
  * INSERT
  * UPDATE
  * DELETE

### `INSERT`

* We can add data into our table using `INSERT`

```sql
INSERT INTO users (column_name1, column_name2) VALUES (value_1, value_2);
```

* The column names correspond to the values respectively

### `DELETE`

* We usually wouldn't want to delete data from a production database. It would be better to have something like a `deleted_at` column that stores a `DATE`. That way we can still filter out deleted results without having to lose the data. Storage is not expensive.

```sql
DELETE FROM table_name 
WHERE condition;
```

* Always include a `WHERE` in your `DELETE` statements, otherwise you'll yeet everything from your table.

### `UPDATE`

* Updating data means that we use the `UPDATE` command. We can specify a number of values in columns to update, and use `WHERE` to indicate which row to make the change on.

```sql
UPDATE students
SET name='Callisto Caiazzo', email='ccaiazzo@gmail.com', github='callcazz'
WHERE id = 3;
```

* Again, **always** have a `WHERE` clause so you don't make sweeping changes to the database