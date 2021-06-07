# Data Definition Language (DDL)

* SQL commands that let us setup and define our database and tables is sometimes referred to as **Data Definition Language (DDL)**

## Altering Data using `ALTER`

* We can create a table quite easily with the `CREATE TABLE` command

* To `ALTER` the table, there are a series of commands that we can use from [this helpful resource](https://www.postgresqltutorial.com/postgresql-alter-table/).

* For example, we can additional columns to our `users` table:

```sql
ALTER TABLE users
ADD COLUMN name VARCHAR(255), 
ADD COLUMN birth_year SMALLINT, 
ADD COLUMN member_since
```

* Or we can change the default to our `DEFAULT` values, such as `member_since` requiring it to be `Now()` unless otherwise stated:

```sql
ALTER TABLE users
ALTER COLUMN member_since
SET DEFAULT Now();
```

## Auto Incrementing / Serial

* The value for the `id` column always needs to be one more than the previous entry
  * Essentially, we want to tell the database to assign a default value for the id column that is an auto incrementing integer

* In Postgre, we can set the id's type to be `SERIAL`, where `SERIAL` is a *pseudo-type* that sets the column to be a `NOT NULL INTEGER` that's value will be automatically increment.

```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY, -- <---
  name VARCHAR(255) NOT NULL,
  birth_year SMALLINT NOT NULL,
  member_since TIMESTAMP NOT NULL DEFAULT Now()
);
```

## Foreign Keys

* If we add another table for our users' pets, then we'll call this table `pets`
  * Our user will be able to have many pets, the pets table will have a special column to reference their owner's id. This will be a **foreign key** column because it references the **primary key** from the `user` table

* When we add the `pets` table, we add the `owner_id` (as the users' id) and then we need to use `REFERENCES` to indicate that it's coming from our `users` database.

```sql
CREATE TABLE pets (
  id SERIAL PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  owner_id INTEGER NOT NULL REFERENCES users(id) -- <---
);
```

* This means if we try to add a pet with an `owner_id` that doesn't exist, we'll get an error.

* If we try to delete a user from our `users` table

```sql
DELETE FROM users WHERE id = 1;
```

* We'll get an error that says

```
Key (id)=(1) is still referenced from table "pets".
```

* This is because setting the `owner_id` to `NULL` would break our own rules, so we need to tell the `pets` database to delete the pet when the owner is deleted.

* We add `ON DELETE CASCADE` to ensure that the row will be deleted when the thing it's referencing is deleted.

```sql
CREATE TABLE pets (
  id SERIAL PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  owner_id INTEGER NOT NULL REFERENCES users(id) ON DELETE CASCADE
);
```